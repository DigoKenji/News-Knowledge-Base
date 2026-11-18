---
type: Paper Summary
title: 'Agent Trust Infrastructure (ATI): identidade federada e atestação para agentes de IA entre organizações'
description: ATI acrescenta verificações determinísticas de identidade, confiança, atestação e política fora do modelo do agente, elevando a contenção de ataques de 43% para 84%.
resource: https://arxiv.org/abs/2611.23202
tags:
- security
- appsec
- multi-agent-orchestration
- model-context-protocol
- oauth2
- oidc
- threat-modeling
- indirect-prompt-injection
- zero-trust-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.23202
  title: 'Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents'
  author: Viet K. Nguyen, Mohammad I. Husain
status: stable
source_url: https://arxiv.org/abs/2611.23202
source: arXiv
source_type: arxiv
published: '2026-11-17'
domain: security
subdomain: appsec
concepts:
- multi-agent-orchestration
- model-context-protocol
- oauth2
- oidc
- threat-modeling
- indirect-prompt-injection
- zero-trust-architecture
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d815495c5cd37068a21c5
lang: pt-br
translation_of: /notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md
---

## 🎯 Em uma frase
ATI é uma camada de segurança independente de framework que verifica agentes de IA entre organizações fora do modelo de linguagem deles, elevando a contenção de ataques de 43% para 84% em 14 cenários de ataque, com cerca de 174 µs de sobrecarga por requisição.

## ❓ O problema
Agentes de IA hoje chamam ferramentas e repassam trabalho a agentes executados por outras organizações. Quando uma requisição dessas chega, o lado receptor normalmente não consegue responder a três perguntas: o chamador é realmente o agente que diz ser, ele está executando o modelo, o prompt e as ferramentas aprovados, e esta ação específica é permitida? Frameworks de agentes como CrewAI, AutoGen, LangGraph e o OpenAI Agents SDK coordenam agentes, mas nenhum deles dá a um agente uma identidade que outro framework ou organização possa verificar. As ferramentas de identidade existentes foram construídas para outros tipos de principal: certificados X.509 vinculam nomes a chaves para hosts e pessoas, o OAuth 2.0 autoriza aplicações que agem em nome de um usuário, e o SPIFFE emite identidades de curta duração para workloads. A parte difícil é que o comportamento de um agente depende de qual modelo e de qual prompt de sistema estão em execução naquele momento, e um modelo trocado ou um prompt editado deixa a chave e o certificado inalterados. Dois termos: **atestação** significa comparar fatos medidos sobre o sistema em execução com valores aprovados; **zero trust** significa verificar cada requisição no ponto em que ela chega, sem confiar nela por causa da sua localização na rede.

## 💡 A ideia
A percepção central é tirar toda decisão de segurança do modelo e colocá-la em código determinístico. ATI posiciona um monitor de referência na frente dos agentes de cada organização, e toda requisição e toda chamada de ferramenta precisa passar pelas suas verificações. O modelo não consegue argumentar para passar por uma verificação, porque a verificação não lê a saída do modelo. Os autores adaptam o padrão de medir-e-atestar dos TPMs, tratando o modelo, as ferramentas aprovadas e o prompt de sistema como itens medidos. Isso permite que a organização receptora verifique a configuração do agente chamador, o que as ferramentas de governança examinadas pelos autores não permitiam.

## ⚙️ Como funciona
### Plano de identidade
Cada agente possui um par de chaves Ed25519, e seu Agent Identifier (AID) é o hash SHA-256 da chave pública, de modo que o AID não pode ser escolhido livremente. Cada requisição precisa ser assinada com essa chave, e o plano verifica a assinatura contra a chave pública. Uma requisição sem chave é recusada na entrada, antes que qualquer agente seja executado. A simulação encurta o hash para 64 bits, e os autores observam que uma implantação usaria o hash completo. A identidade comprova a posse de uma chave, não a pertença a uma organização; o plano de confiança fornece isso.

### Plano de confiança: AgentCA e federação
Cada organização mantém sua própria autoridade certificadora, uma AgentCA, que emite certificados no estilo X.509 que vinculam um AID a uma organização, a um papel e à assinatura da CA. As oito CAs das organizações formam uma federação plana, sem raiz comum, e cada CA atesta apenas por sua própria organização, imposto por uma restrição de nome que exige que o emissor corresponda à organização do certificado. Uma requisição só passa se seu certificado for assinado por uma CA da federação, a assinatura for verificada, o AID corresponder à chave que assinou a requisição, o certificado estiver dentro do período de validade, e nem o AID nem a organização parceira estiverem revogados. Os autores mantêm apenas os campos que uma verificação lê: AID, organização, papel, emissor, período de validade e assinatura.

### Plano de atestação
Identidade e confiança estabelecem quem está chamando, mas não o que está rodando por trás dessa identidade. Logo antes de o modelo ser chamado, o plano de atestação compara três coisas com valores aprovados: o modelo em execução, a ferramenta fixada web_search (verificada pelo nome do módulo e da função), e um hash SHA-256 do prompt de sistema. Qualquer divergência é recusada antes de o modelo ser invocado. Na simulação, a verificação roda uma vez quando a requisição entra na federação, e não em um temporizador, e usa comparações simples em vez de relatórios assinados.

### Plano de política
A política verifica cada chamada de ferramenta, como cada db_read e db_write, que os planos anteriores nunca veem. Ela impõe três regras determinísticas. Primeiro, uma escrita só pode ir para a organização onde a requisição começou, de modo que outras organizações podem ler, mas não escrever. Segundo, uma transação já marcada como processada não pode ser processada de novo. Terceiro, o escritório de auxílio financeiro recusa qualquer leitura que devolveria mais de um registro, como uma barreira contra exportação em massa. Uma regra só é disparada na chamada que ela corresponde: a regra entre organizações só dispara se o agente chamar db_write, por isso os números de replay e de delegação ficam abaixo de 100%. Injeção de prompt não é tratada aqui.

### Plano de responsabilização
Este plano não bloqueia nada. Para cada chamada de ferramenta permitida, ele acrescenta uma entrada de log contendo o AID, o modelo, um timestamp, a ferramenta e um hash SHA-256 dos argumentos, de modo que o log mostra o que aconteceu sem expor o conteúdo. Cada entrada inclui o hash da entrada anterior e é assinada com a chave da CA da organização, então editar ou apagar uma entrada no meio quebra a cadeia, e uma etapa de verificação a detecta. O plano também sinaliza qualquer requisição única que leia dados privados de dois ou mais escritórios, o padrão de conluio (A11), para revisão posterior em vez de recusá-la, porque uma tarefa legítima de múltiplos escritórios tem a mesma aparência.

### Entre duas organizações
As duas organizações compartilham apenas as chaves públicas de suas CAs. Quando o agente liaison da Org 1 chama a Org 2, ele assina a requisição e anexa seu certificado. O monitor da Org 2 verifica a assinatura contra o AID, o certificado contra a CA e a lista de revogação da Org 1, atesta seu próprio agente que responde, e aplica suas regras de política; a requisição pode ler os registros da Org 2, mas não escrevê-los. Se alguma verificação falhar, a Org 1 recebe uma recusa e nada é escrito. Cada organização mantém seu próprio log. Os autores disponibilizam as verificações como um pacote Apache-2.0, mas afirmam que os resultados vêm do ambiente de testes, e não desse pacote.

## 🧪 Evidências
| Configuração | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Contenção geral de ataques, 14 casos de ataque, todos os quatro planos de bloqueio | 43% | 84% | parcela de tentativas de ataque interrompidas |
| Igual, 11 casos de ataque avaliados de forma consistente em todas as configurações | 40% | 80% | parcela de tentativas de ataque interrompidas |
| Atacante externo (A1) | 20% | 100% | parcela interrompida |
| Substituição de modelo (A7) | 0% | 100% | parcela interrompida |
| Adulteração do prompt de sistema (A14) | 27% | 100% | parcela interrompida |
| Ataque de replay (A5) | 15% | 75% | parcela interrompida |
| Delegação não autorizada (A9) | 16% | 71% | parcela interrompida |
| Sucesso em tarefas benignas, 6 casos | 91% | 92% | parcela de tentativas corretas |
| Tempo de verificação por requisição, uma chamada de ferramenta (Tabela III) | não se aplica | aproximadamente 174 µs (média 173,8 µs, mediana 167,7 µs) | tempo por requisição |

Os autores executaram os ataques em uma simulação de processo único, com 75 tentativas por caso de ataque, e um painel de cinco modelos juízes pontuou cada tentativa. A ablação incremental mostra que cada plano eleva os ataques que mira: Identidade, Confiança, Atestação e Política acrescentam 6, 12, 15 e 8 pontos à contenção geral nas colunas cumulativas da tabela, sendo que a contenção geral atingiu 76% após a Atestação. Os resultados dependem do modelo sem ATI: a injeção de prompt A6 foi interrompida 0% das vezes em dois modelos e 93% em outro, então testes com um único modelo podem induzir ao erro. Os autores também relatam resultados entre modelos: o mesmo ataque foi recusado por alguns modelos e executado por outros, enquanto as verificações determinísticas na entrada impediram quase todos os ataques direcionados, independentemente do modelo. O teste entre frameworks executou uma tarefa benigna em cada um dos seis frameworks, e todos os seis devolveram o resultado correto, mas os 14 ataques foram executados apenas no CrewAI e no AutoGen. Esses resultados não mostram o desempenho sob carga pesada nem com milhares de agentes, nem o comportamento com organizações que administram a própria infraestrutura.

## ⚠️ Limitações
- A injeção de prompt (A6) continua baixa com os quatro planos de bloqueio ativados, porque nenhum plano atua sobre dados não confiáveis no que o agente lê, e os autores tratam isso como um problema separado.
- O conluio (A11) não pode ser bloqueado sem bloquear também o trabalho legítimo de múltiplos escritórios, então o plano apenas o sinaliza.
- A atestação depende de medições informadas pelo host do agente chamador, então não consegue detectar relatórios falsificados de um host comprometido, e o monitor roda no mesmo processo que o agente.
- Os metadados de validade do certificado ficam fora do payload assinado, e o log não detecta a remoção de entradas no fim da cadeia.
- Quatro dos cinco modelos juízes também estão entre os modelos testados, e os autores afirmam que eles não são totalmente independentes.
- Certificate transparency, revogação distribuída e raízes de confiança em hardware não foram construídas, e a criptografia do tráfego e a disponibilidade sob carga estão fora do escopo.
- Alguns valores da Tabela I para A12 a A14 foram preenchidos a partir de como as verificações funcionam, e não medidos.
- 🔎 Contexto adicional: o número de destaque de 84% é uma média sobre cinco modelos e cinco formulações, e os autores mostram que o sucesso dos ataques varia muito conforme o modelo, então uma implantação em um modelo específico pode ficar bem distante dessa média.
- 🔎 Contexto adicional: o número de 174 µs cobre apenas as verificações do ATI, medidas em um único laptop; uma implantação real também transportaria assinaturas e certificados pela rede em cada salto, o que a simulação ignora.

## 🏗️ O que isso significa para construir sistemas
_Interpretação, não afirmação dos autores:_ o ATI ficaria como um proxy de segurança na frente de cada serviço de agente, com o próprio agente inalterado, da mesma forma que um API gateway ou um service mesh fica na frente de serviços comuns. O custo seriam as verificações de assinatura por requisição, que os autores medem bem abaixo de um milissegundo, mais a emissão de certificados por organização, um processo de revogação e uma chave de assinatura protegida para o log. Para confiar nele, você precisaria de uma atestação de host em que pudesse se apoiar, já que as medições dos autores vêm do host, e de uma revisão de privacidade dos logs e certificados, que, segundo os autores, expõem relações entre organizações.

## 🔑 Principais conclusões
- As verificações de identidade na entrada só barram atacantes externos sem credenciais (A1 subiu de 20% para 100%); elas não comprovam que o modelo ou o prompt são os aprovados.
- A atestação do modelo, das ferramentas e do prompt de sistema barra a substituição de modelo (A7, de 0% para 100%) e a adulteração do prompt de sistema (A14, de 27% para 100%), que as verificações de identidade deixavam passar.
- Com os quatro planos de bloqueio ativados, a contenção geral subiu de 43% para 84% em 14 casos de ataque, enquanto o sucesso em tarefas benignas foi de 91% para 92%.
- As regras de política impostas em código só barram parcialmente o replay e as escritas entre organizações (A5 subiu de 15% para 75%, A9 de 16% para 71%), porque cada regra só dispara na chamada que ela corresponde.
- A segurança no nível do modelo varia muito: o mesmo ataque pode ser recusado por um modelo e executado por outro, então avaliações com um único modelo podem superestimar a segurança.
- A verificação completa acrescenta cerca de 174 µs por requisição na simulação, gastos principalmente na verificação de assinaturas nos planos de Identidade e Confiança.

## 📚 Plano de leitura
### A partir do artigo
- [repositório do ATI](https://github.com/CalPolyPomonaCS/Agent-Trust-Infrastructure-ATI-Official) — o pacote open-source ati e seu README com os passos de configuração e os formatos de token.
- [Microsoft Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit) — a ferramenta de governança mais próxima com a qual os autores comparam, que reporta taxas de detecção de injeção de prompt.
- [Model Context Protocol](https://modelcontextprotocol.io) — o protocolo do qual os autores dizem que o ATI não depende, e aquele cuja falha no MCP Inspector eles citam.
- [CVE-2025-49596](https://nvd.nist.gov/vuln/detail/CVE-2025-49596) — a falha de execução remota de código em um componente do MCP que motiva as verificações em nível de ferramenta.
- [SPIFFE](https://spiffe.io) — o modelo de identidade de workloads que o plano de identidade segue.

### Para ir além
- Zero Trust Architecture, NIST SP 800-207 — a regra de nunca confiar por padrão que o ATI aplica aos agentes.
- Binding Agent ID (arXiv:2512.17538) — uma abordagem de responsabilização que comprova, com provas de conhecimento zero, qual programa um agente executa.
- Authenticated Delegation and Authorized AI Agents (arXiv:2501.09674) — tokens de delegação com escopo e expiração, que os autores optaram por não simular.
- O Entity Attestation Token (RFC 9711) e as credenciais de workload do grupo IETF WIMSE — os formatos de token que o ATI usa para identidade e atestação.
- Atestação de computação confidencial com hardware TPM e TEE — o que, segundo os autores, fortaleceria a lacuna de confiança no host.

## Conceitos
[Orquestração multiagente](/pt-br/concepts/multi-agent-orchestration.md) · [Model Context Protocol (MCP)](/pt-br/concepts/model-context-protocol.md) · [OAuth 2.0](/pt-br/concepts/oauth2.md) · [OpenID Connect](/pt-br/concepts/oidc.md) · [Modelagem de ameaças](/pt-br/concepts/threat-modeling.md) · [Injeção indireta de prompt](/pt-br/concepts/indirect-prompt-injection.md) · [Arquitetura zero trust](/pt-br/concepts/zero-trust-architecture.md)
