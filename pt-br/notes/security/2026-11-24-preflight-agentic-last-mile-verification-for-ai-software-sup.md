---
type: Paper Summary
title: 'Preflight: verificação agêntica de última milha para cadeias de suprimento de software de IA'
description: Preflight executa uma dependência pelas chamadas que um programa realmente faz, expondo comportamentos maliciosos que sondagens baseadas apenas em importação deixam passar.
resource: https://arxiv.org/abs/2611.31110
tags:
- security
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.31110
  title: 'Preflight: Agentic Last-Mile Verification for AI Software Supply Chains'
  author: Yaman Jandali, Nojan Sheybani, Farinaz Koushanfar, Tara Javidi
status: stable
source_url: https://arxiv.org/abs/2611.31110
source: arXiv
source_type: arxiv
published: '2026-11-23'
domain: security
concepts:
- ai-agent
- threat-modeling
- zero-trust-architecture
- supply-chain-attack
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81b8a039eac726c8a074
lang: pt-br
translation_of: /notes/security/2026-11-24-preflight-agentic-last-mile-verification-for-ai-software-sup.md
---

## 🎯 Em uma frase
O Preflight verifica uma dependência Python executando-a pelas chamadas que a aplicação realmente faz. Ele retorna quatro vereditos de malicioso em cinco contextos reais de incidentes, zero falsos positivos em 100 pacotes benignos e vereditos maliciosos em 2 de 2 casos de ablação nos quais a sondagem apenas por importação não encontrou nada.

## ❓ O problema
Aplicações Python importam pacotes de terceiros e os executam com os privilégios da própria aplicação. Em software ligado a IA, essas dependências podem acessar tokens de provedores de modelos, credenciais de nuvem, arquivos locais e dados de usuários, então uma versão comprometida executa dentro da fronteira de confiança da aplicação. As verificações atuais respondem perguntas diferentes: proveniência (de onde veio um artefato, comprovada por atestações), análise estática (se o código parece suspeito) e comportamento em nível de pacote (o que o pacote faz em geral). Nenhuma delas diz como uma versão específica se comporta para o programa específico que vai executá-la. Código malicioso pode permanecer dormente até que uma função, construtor ou caminho de plugin seja chamado, e uma aplicação costuma tocar apenas uma pequena parte da API de um pacote. Os autores chamam essa lacuna entre a garantia em nível de pacote e a execução específica da aplicação de "última milha" da verificação de cadeia de suprimento.

## 💡 A ideia
Verifique a dependência pela forma como o seu programa a usa, e não pela forma como o pacote se apresenta. O Preflight lê o programa, encontra as funções do pacote que ele referencia, gera um pequeno harness que chama essas funções, executa o harness em um sandbox restrito e compara o comportamento observado com uma versão anterior conhecida. O ponto central é que um import não é uma chamada: muitos comportamentos só aparecem quando uma função referenciada é executada, então sondar a superfície referenciada vê o que a sondagem apenas por importação deixa passar. O artigo não oferece uma analogia para essa ideia.

## ⚙️ Como funciona
### Extração da superfície da aplicação
O Preflight analisa o programa a jusante com a árvore de sintaxe abstrata (AST) do Python e registra cada pacote importado e cada símbolo do pacote que o programa referencia, incluindo aliases e acessos por atributo de módulo. Esses registros são cruzados com versões exatas de requirements.txt, uv.lock ou Pipfile.lock, e dependências sem versão fixada são rejeitadas, porque o veredito precisa se vincular a uma única versão. A saída é a superfície de dependência referenciada: os construtores e funções que as sondagens devem chamar. Por exemplo, se o programa chama requests.get, a sondagem chama requests.get e não apenas importa requests.

### Síntese de harness agêntico
Para cada superfície referenciada, um modelo de linguagem (gpt-5.4-mini pela API Responses da OpenAI, temperature 0, no máximo 4.096 tokens de saída, timeout de 120 segundos) recebe o contexto do código-fonte do pacote, a identidade e a versão do pacote, os símbolos referenciados e as restrições de invocação, e propõe código Python que os exercita. Um validador determinístico aceita ou rejeita cada candidato: ele precisa ser Python válido com no máximo 100 linhas, importar o alvo, invocar todos os símbolos referenciados, respeitar as restrições fixas e manter a cobertura de símbolos após a instrumentação da AST. Um candidato rejeitado recebe feedback e uma revisão, dentro de um orçamento de duas rodadas de validação; os traces de execução não são fornecidos ao modelo. Se isso falhar, o pipeline usa um harness de fallback determinístico, mas a avaliação com agente ao vivo exclui as gerações de fallback. O espaço de ações do modelo se limita a escrever e revisar o código-fonte do harness; ele não escolhe pacotes, a política do sandbox, os traces nem o fim da análise.

### Observação comportamental em sandbox
Cada artefato é instalado e executado em um contêiner Docker restrito, rodando como usuário não-root, com sistema de arquivos raiz somente leitura, no-new-privileges, capabilities Linux removidas, recursos limitados e uma política seccomp. O sandbox precisa permitir ptrace, porque o strace é usado para rastrear atividade de processos e de chamadas de sistema; o tráfego de rede é capturado, o DNS é registrado por meio de um sinkhole interno, e o sistema de arquivos é comparado antes e depois da execução. Os traces brutos são ruidosos, então são reduzidos a uma impressão digital comportamental canônica: endpoints normalizados, leituras de variáveis de ambiente sensíveis, escritas persistentes, executáveis iniciados e categorias de syscall. A evidência bruta continua disponível para inspeção, enquanto a impressão digital permite comparação estável entre versões.

### Verificação diferencial
Acesso à rede, escritas em arquivos e subprocessos são normais em software benigno, então os autores tratam a mudança entre versões, e não a presença de uma capacidade, como o principal sinal. Com F(P, A) como o conjunto de capacidades canônicas observadas quando o pacote P é exercitado pela superfície da aplicação A, o diferencial é:

```text
ΔA(Pv, Pb) = F(Pv, A) \ F(Pb, A)
```

Pv é a versão alvo, Pb a versão de referência e A a aplicação. F retorna o conjunto de capacidades observadas, e a barra invertida é a diferença de conjuntos, então ΔA é o que o alvo faz para esta aplicação que a referência não fazia. Leituras de variáveis de ambiente com credenciais, endpoints não vistos ou não declarados, escritas persistentes e subprocessos recém-observados recebem severidade maior. Quando não existe um predecessor defensável, a ferramenta executa apenas a análise do alvo e não faz nenhuma afirmação sobre comportamento recém-introduzido.

### Vereditos e certificados assinados
A política de veredito é simples: pelo menos dois achados críticos de capacidade implicam malicioso, um implica suspeito, e nenhum implica limpo, a menos que um achado estático crítico substitua o resultado. Bandit, Semgrep e um verificador de typosquatting fornecem achados estáticos, mas achados estáticos de alta severidade, sozinhos, não são críticos. O resultado é codificado como uma declaração in-toto em um envelope DSSE e assinado com Ed25519. Um verificador separado checa a assinatura, os digests do sujeito, a validade do certificado, o hash do programa, o hash do lockfile e o veredito, e só permite a execução para um certificado limpo e válido; assim, a geração de evidências fica separada de uma etapa pequena e determinística de imposição. Uma tentativa de fazer o gpt-5.4 sintetizar vereditos falhou nos cinco contextos, porque todas as requisições excederam o limite de tamanho de entrada ou de contexto, então todos os vereditos finais foram determinísticos.

## 🧪 Evidências
| Configuração | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Contexto de cliente LiteLLM | 1.82.6 → 1.82.7 | limpo | veredito; 0 achados críticos de capacidade |
| Contexto de proxy LiteLLM | 1.82.6 → 1.82.7 | malicioso | veredito; 14 achados críticos (dois endpoints de metadados, 12 executáveis) |
| Ultralytics | 8.3.40 → 8.3.41 | malicioso | veredito; 6 achados críticos (três endpoints, três caminhos curl) |
| ctx | 0.1.2 → 0.2.2 | malicioso | veredito; 3 achados críticos (endpoints) |
| DurableTask | apenas análise do alvo (sem predecessor benigno) | malicioso | veredito; 3 endpoints, dois deles infraestrutura de sandbox |
| Corpus benigno | 100 pacotes PyPI fixados, relacionados a IA, dados e ML | 100 limpos, 0 sinalizados | taxa de falsos positivos 0/100 (0%) |
| Ablação, Ultralytics | sondagem apenas por importação: limpo | harness em nível de função: malicioso | 2 de 2 achados pré-registrados observados (apenas importação: nenhum) |
| Ablação, ctx | sondagem apenas por importação: limpo | harness em nível de função: malicioso | 1 de 2 achados pré-registrados observados (apenas importação: nenhum) |

O par LiteLLM é o teste mais limpo: as mesmas duas versões recebem veredito limpo para a carga de cliente e veredito malicioso para a carga de proxy, então o veredito segue a superfície exercitada. O resultado benigno cobre 100 pacotes e apenas as cargas que foram exercitadas, e os autores dizem que ele não cobre todos os caminhos da API. A ablação tem apenas dois casos e varia apenas o modo do harness. A cobertura dos achados esperados é parcial: LiteLLM proxy e ctx corresponderam apenas a parte dos achados pré-registrados, e DurableTask não corresponderam a nenhum, embora seu veredito seja malicioso.

## ⚠️ Limitações
- Reflexão, importações construídas dinamicamente, callbacks, plugins, ramos sensíveis a entrada e comportamento dependente do ambiente podem ficar fora da superfície extraída ou exercitada.
- A contenção só consegue interromper estágios posteriores de execução maliciosa depois que o comportamento anterior já foi observado, e a impressão digital canônica não atribui comportamento à instalação, à importação ou à execução explícita da API.
- Uma versão anterior é uma referência comportamental, não uma prova de benignidade, e casos apenas de alvo, como DurableTask, não mostram o que uma versão específica introduziu de novo.
- A ablação não isola a síntese iterativa da geração de uma única tentativa, e não mostra que harnesses gerados por LLM superem harnesses determinísticos em nível de função com o mesmo conhecimento de API.
- O laço de síntese fecha sobre a aceitação pelo validador, não sobre o comportamento em tempo de execução; alimentar traces de volta na escolha das sondagens é trabalho futuro.
- O alias do modelo foi registrado sem um snapshot imutável nem seed, e parâmetros de amostragem além da temperature não foram definidos.
- Artefatos não confiáveis rodam em Docker; os autores observam que um isolamento mais forte, como microVMs descartáveis, poderia ser adicionado sem mudar a arquitetura.
- 🔎 Contexto adicional: zero sinalizados entre 100 pacotes benignos dá um limite superior de 95% de cerca de 3% para a taxa de falsos positivos (regra dos três), então esta amostra não consegue mostrar uma taxa muito abaixo disso.
- 🔎 Contexto adicional: os quatro incidentes têm artefatos maliciosos conhecidos com proveniência verificada, o que é um teste mais fácil do que malware desconhecido; a pergunta mais difícil é se o método encontra comportamentos para os quais não foi ajustado.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. O lugar natural para essa verificação é um gate no CI ou na implantação: a cada atualização de dependência, ler o lockfile e o código da aplicação, executar os harnesses gerados em um sandbox e bloquear a liberação, a menos que exista um certificado assinado e limpo e válido para aquele hash de programa e aquele hash de lockfile. O custo de modelo é pequeno (o artigo relata 14 chamadas de API, cerca de US$ 0,027 estimados, para cinco contextos), então os custos reais são a execução no sandbox, a validação dos harnesses e a manutenção de uma versão benigna de referência para cada pacote. Para confiar em um gate assim, seria preciso confiar no validador determinístico, na configuração do sandbox, nas chaves de assinatura e na escolha da referência, e seria preciso saber quais superfícies referenciadas o harness não exercitou.

## 🔑 Principais conclusões
- A mesma versão de pacote pode receber vereditos diferentes para programas diferentes: LiteLLM 1.82.6 para 1.82.7 é limpo para a carga de cliente e malicioso para a carga de proxy.
- Sondar as funções que o programa referencia expôs comportamento malicioso em 2 de 2 casos de ablação, enquanto a sondagem apenas por importação não expôs nenhum.
- O modelo fica restrito a escrever o código-fonte do harness; a validação determinística, o sandbox e a assinatura é que produzem o veredito.
- Nenhum de 100 pacotes PyPI benignos foi sinalizado, mas apenas para as cargas que o artigo exercitou, não para todos os caminhos da API.
- Veredito e cobertura dos achados esperados são registrados separadamente; DurableTask foi malicioso sem corresponder a nenhuma das categorias pré-registradas.
- Sem um predecessor benigno, a ferramenta relata o comportamento observado, mas não consegue dizer o que uma versão introduziu de novo.

## 📚 Plano de leitura
### Do artigo
- [Dataset de pacotes de software malicioso da Datadog](https://github.com/DataDog/malicious-software-packages-dataset) — o dataset revisado por humanos de onde vêm os artefatos dos incidentes; leia para ver como os rótulos são construídos.
- [GuardDog](https://github.com/DataDog/guarddog) — o scanner estático e baseado em metadados com o qual o artigo contrasta sua abordagem comportamental.
- [OpenSSF Package Analysis](https://openssf.org/package-analysis/) — análise comportamental em escala de registro, a alternativa mais próxima de uma execução restrita a um programa.
- [Atestações digitais do PyPI](https://docs.pypi.org/attestations/) — a camada de proveniência que o Preflight complementa, sem substituir.
- [SLSA v1.2](https://slsa.dev/spec/v1.2/) — a especificação de garantias de cadeia de suprimento por trás da discussão de proveniência.
- [Análise estática diferencial para atualizações maliciosas (SCORED '23)](https://doi.org/10.1145/3605770.3625211) — a ideia de comparação de versões, aplicada de forma estática.
- [CoverUp](https://doi.org/10.1145/3729398) — geração de testes com LLM guiada por feedback de cobertura, o análogo mais próximo do laço do harness.
### Para ir além
- Duan et al., "Towards measuring supply chain attacks on package managers for interpreted languages" (NDSS 2021) — como esses ataques chegam aos gerenciadores de pacotes.
- Ferreira et al., "Containing malicious package updates in npm with a lightweight permission system" (ICSE 2021) — limites de permissão em tempo de execução para atualizações suspeitas.
- "in-toto: Providing farm-to-table guarantees for bits and bytes" (USENIX Security 2019) — o framework de atestação por trás da declaração assinada.
- DySec, análise dinâmica baseada em ML para pacotes PyPI maliciosos (IEEE TIFS 2026) — uma alternativa aprendida aos vereditos comportamentais baseados em regras.

## 🔗 Conceitos
- [ai-agent](/pt-br/concepts/ai-agent.md)
- [threat-modeling](/pt-br/concepts/threat-modeling.md)
- [zero-trust-architecture](/pt-br/concepts/zero-trust-architecture.md)
- [Supply-chain attack](/pt-br/concepts/supply-chain-attack.md)
