---
type: Article Summary
title: O melhor desenvolvedor já não é quem escreve o melhor código
description: O desenvolvimento orientado a especificações desloca o valor do desenvolvedor da velocidade de programação para a intenção precisa e a verificação rigorosa, elevando a barra.
resource: https://medium.com/gitconnected/the-best-developer-is-no-longer-the-one-who-writes-the-best-code-996e8ed0869b
tags:
- software-engineering
- spec-driven-development
- context-engineering
- ai-agent
- multi-agent-orchestration
- verification-tax
- technical-debt
- version-control-code-review
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- resource: https://medium.com/gitconnected/the-best-developer-is-no-longer-the-one-who-writes-the-best-code-996e8ed0869b
  title: The Best Developer Is No Longer the One Who Writes the Best Code
  author: unknown
status: stable
source_url: https://medium.com/gitconnected/the-best-developer-is-no-longer-the-one-who-writes-the-best-code-996e8ed0869b
source: Level Up Coding
source_type: medium
domain: software-engineering
concepts:
- spec-driven-development
- context-engineering
- ai-agent
- multi-agent-orchestration
- verification-tax
- technical-debt
- version-control-code-review
- software-engineering
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/425baf74172d81d48210dfe13bd4b286
lang: pt-br
translation_of: /notes/software-engineering/2026-11-27-the-best-developer-is-no-longer-the-one-who-writes-the-best.md
---

## 🎯 Tese central
No Spec-Driven Development, o valor do desenvolvedor passa de quão rápido ele transforma ideias em código para quão precisamente declara a intenção e quão rigorosamente verifica o resultado, e isso eleva a barra.

## 🧭 Por que o autor escreveu
O autor parte de uma mudança percebida: assistentes de IA produzem código em minutos, mas a verificação, a integração e a responsabilização consomem o resto do dia. Se a tese estiver certa, a habilidade escassa já não é digitar código. É definir o que significa "correto" e provar isso, e equipes que continuarem medindo desenvolvedores pela velocidade de programação vão errar tanto na avaliação das pessoas quanto na avaliação do risco.

## Introdução
O artigo abre com uma comparação entre dois dias de trabalho. Em 2019, um desenvolvedor passava oito horas no editor, depurando linha a linha através de quilômetros de boilerplate. Hoje, o mesmo desenvolvedor passa uma hora escrevendo uma especificação, roda três instâncias de agentes em segundo plano e gasta o resto do dia lendo e aceitando o que foi gerado. O ponto do autor é que isso é "não menos trabalho, mas um trabalho diferente". A abertura também cita dados da Faros AI, com mais de 10.000 desenvolvedores em 1.255 equipes: em equipes com alta adoção de IA, o tempo de revisão de pull requests cresceu quase 91%.

> 🔎 Contexto adicional: a Faros AI é fornecedora de análises de produtividade em engenharia, então seus números medem a base de clientes dela, e não a indústria como um todo.

## A conclusão que você pode levar agora
No Spec-Driven Development (SDD), o gargalo deixou de ser escrever código e passou a ser definir a intenção e verificar o resultado. Programar não está morrendo; o valor do desenvolvedor migra da velocidade de digitação para a precisão com que ele diz o que quer e o rigor com que confere o resultado.

A analogia do autor: você deixa de ser pedreiro e passa a ser arquiteto supervisionando a obra. Você produz a planta (a especificação), encomenda a construção a uma equipe de agentes de IA e assina o aceite técnico. O código é uma realização possível do projeto, não o projeto em si.

A armadilha crítica está em uma frase: "todos os testes passaram" não é o mesmo que "corresponde à minha intenção". Fechar essa lacuna é o trabalho novo e mais difícil. A consequência prática é incômoda. Se você economiza uma hora gerando código, mas não consegue verificar se ele faz exatamente o que você pretendia, não ganhou tempo. Adiou o trabalho para uma dívida invisível que volta com juros.

> ❌ **Anti-padrão — Verificação apenas por testes verdes:** aceitar código gerado porque a suíte de testes passa. **Por que as pessoas fazem isso:** testes passando parecem prova, e o código parece plausível. **Consequência:** o código pode divergir da intenção sem que ninguém perceba, e o custo volta depois como dívida. **Correção:** confira a saída contra a especificação que você escreveu, e não apenas contra os testes.

## Papel tradicional vs. novo papel
O centro de gravidade saiu de traduzir lógica em código e foi para formular intenção. O autor apresenta isso como uma mudança observável, não como um slogan de fornecedor. Até pouco tempo atrás, o principal custo da engenharia era converter lógica de negócio em sintaxe. Modelos de fronteira tornaram baratos os primeiros rascunhos de código, então o valor migrou para outro lugar.

No novo arranjo, você se torna um **autor de intenção**, cuja principal entrega é uma especificação precisa e interpretável por máquina, e não código. A IA gera e implementa o código. A versão mais radical se chama spec-as-source: você edita apenas a especificação, e o código gerado é marcado com "não edite manualmente". O autor observa que a maioria das equipes fica entre o spec-first e esse extremo.

Por baixo disso há uma inversão filosófica. Durante anos, a suposição era que o código é a fonte da verdade, a única descrição confiável do que um sistema faz. O SDD torna a intenção a fonte da verdade e o código, seu artefato gerado. Uma boa especificação funciona então como um contrato que define o "correto" antes de qualquer coisa ser construída.

O autor também reporta a escala. A parcela de código coautorado por IA cresceu de alguns por cento em 2023 para dezenas de por cento hoje, e mais da metade dos engenheiros profissionais usa ferramentas de IA diariamente. Sean Grove, da OpenAI, argumentou que escrever código representa de 10 a 20 por cento do valor de um engenheiro, e os 80 a 90 por cento restantes são "comunicação estruturada", isto é, articulação organizada da intenção. O autor diz que as proporções exatas podem ser discutidas, mas a direção é difícil de contestar: menos código não significa menos trabalho.

*Figura (legenda presente apenas na fonte):* o centro de gravidade do trabalho se desloca de escrever código para definir a intenção e aceitar o resultado.

## Competências que acabaram de ganhar valor
O autor descreve uma polarização de competências, não "o fim da programação". Kent Beck disse que noventa por cento de suas habilidades perderam valor e dez por cento ganharam mil vezes mais. O autor chama isso de exagero deliberado que, ainda assim, capta a assimetria.

Ganhando valor:
- **Pensamento arquitetural:** projetar fronteiras de sistemas e tomar decisões que o agente não tomará por você.
- **Precisão de requisitos:** transformar um vago "queremos que funcione" em uma especificação inequívoca e testável.
- **Verificação e julgamento** (a palavra de Beck: "gosto"): avaliar se um resultado é realmente bom, e não apenas tecnicamente correto.
- **Context engineering:** dar ao agente o contexto certo no momento certo.
- **Orquestração de agentes:** coordenar várias instâncias paralelas trabalhando em partes diferentes de uma tarefa.

### Context engineering
O autor considera esta a habilidade menos óbvia. Um agente é tão bom quanto o contexto que você lhe dá. Mecanicamente, você fornece a especificação, as restrições, exemplos e fragmentos relevantes do sistema existente, na ordem e na quantidade certas, para que o modelo raciocine a partir do material correto. Sem isso, o agente cumpre a letra do prompt e não a sua intenção, e essa é a origem da maioria dos bugs encontrados na revisão.

> 🔎 Contexto adicional: "contexto", aqui, significa tudo o que o modelo lê em uma chamada, incluindo instruções, exemplos e código recuperado, e não apenas a pergunta do usuário.

Perdendo valor: boilerplate, tradução manual de requisitos em código, depuração linha a linha e memorização de sintaxe e de APIs. Esse conhecimento não se torna inútil, pois você não consegue julgar a saída do agente sem ele. Mas a fluência bruta em digitá-lo deixou de ser um diferencial.

A regra da equipe do autor é: não faça commit de código que você não consegue explicar.

> ❌ **Anti-padrão — Aceitar o que parece bom:** fazer merge de algumas centenas de linhas que parecem razoáveis e passam nos testes. **Por que as pessoas fazem isso:** o volume é grande e a revisão parece uma formalidade. **Consequência:** código que ninguém na equipe entende acaba em produção. **Correção:** a regra do autor, "não faça commit de código que você não consegue explicar".

*Figura (legenda presente apenas na fonte):* mapa de competências: onde investir e o que já não é diferencial competitivo.

## O que fica, o que sai
O gargalo não desapareceu; ele se deslocou para a verificação. Quando a geração de código sobe de centenas para milhares de linhas por dia, escrever deixa de ser a restrição, e a revisão, a integração e a responsabilização assumem esse papel. Simon Willison observou que todo o ciclo de vida do software foi projetado com a suposição de que produzir várias centenas de linhas leva um dia, e que essa suposição não se sustenta mais. O trabalho do desenvolvedor continua sendo entregar código que ele provou que funciona. A IA colocou a prova no centro da profissão.

O autor enuncia uma regra dura: quem clica em "merge" é responsável. Uma máquina não pode ser responsabilizada por uma queda de serviço, então quanto mais código a IA gera, mais valioso se torna o ato humano de assumir as consequências.

### A IA realmente acelera?
O autor argumenta que os dados parecem contraditórios apenas até você verificar as condições. A IA acelera tarefas simples e *greenfield*, e pode desacelerar engenheiros experientes em sistemas grandes já existentes (*brownfield*). São condições diferentes com resultados diferentes, não as mesmas condições com resultados opostos.

> 🔎 Contexto adicional: *greenfield* significa construir do zero, sem uma base de código existente a respeitar; *brownfield* significa alterar um sistema grande e já existente.

As evidências que o artigo apresenta:
- Em um experimento controlado com um servidor HTTP simples, desenvolvedores com um assistente terminaram mais rápido (Peng et al., n=95, greenfield, arXiv:2302.06590).
- Em um experimento de campo controlado com quase 5.000 desenvolvedores, os pull requests mesclados cresceram cerca de 26%.
- A METR estudou 16 desenvolvedores experientes de código aberto em seus próprios repositórios grandes (brownfield, com milhões de linhas). Eles foram 19% mais lentos com IA, embora acreditassem estar 20% mais rápidos. A METR depois revisou esse estudo por causa de um erro de seleção da amostra, então o autor trata o resultado como um sinal, não como prova definitiva.

Depois vem o paradoxo da confiança: a IA coautora uma parcela significativa do código de produção, mas 76% dos desenvolvedores relatam alucinações frequentes e baixa confiança no código gerado por IA (Qodo, State of AI Code Quality 2025). A distância entre a escala da delegação e o nível de confiança é onde nasce a dívida de verificação. O tempo economizado na escrita volta como uma auditoria tediosa do código de outra pessoa.

O autor também corrige uma crença anterior: a de que a especificação é simplesmente o novo código, de modo que escrever boas specs dissolveria o problema. Até Guy Podjarny, um defensor proeminente do trabalho centrado em especificações, deslocou sua narrativa de "spec" para "contexto" (podcast da Tessl, episódio 101). A linguagem natural é ambígua demais para substituir totalmente a precisão formal, e modelos melhores não resolverão isso, porque a ambiguidade é uma propriedade da própria linguagem.

> ❌ **Anti-padrão — Spec como solução para tudo:** acreditar que uma especificação suficientemente boa elimina a necessidade de verificação. **Por que as pessoas fazem isso:** a spec parece o contrato que resolve a correção. **Consequência:** a ambiguidade da linguagem natural sobrevive no código e a carga de revisão continua. **Correção:** planejar a verificação como etapa permanente, mesmo com boas specs.

O fracasso cujo custo ninguém orçou costuma ter sempre o mesmo formato: código gerado por IA vai para produção sem revisão de um engenheiro experiente, e horas depois alguém está contabilizando prejuízos. A conclusão do autor não é "contra a IA", mas "planejar o processo com o tempo de verificação embutido desde o início", porque economizar na escrita sem verificar é uma ilusão.

*Figura (legenda presente apenas na fonte):* as mesmas ferramentas, em condições diferentes, e por isso os dados de produtividade parecem contraditórios até você olhar o contexto.

## Como se preparar você e sua equipe
O sucesso depende menos de implantar a ferramenta do que de os membros da equipe se tornarem **integradores ativos**, e não delegadores passivos. Isso é trabalho de liderança, não uma tarefa de configuração. Você pode obrigar o uso da IA; não pode obrigar confiança ou julgamento. A diferença decide se a IA desenvolve as competências da equipe ou as erode.

No nível individual, a decisão é: você trata o agente como parceiro de pensamento ou como um gerador que você aprova? O autor cita um estudo piloto de Shen e Tamkin (Anthropic, 2026) com cerca de cinquenta participantes aprendendo uma nova biblioteca. Desenvolvedores que usaram IA para investigar o "porquê" tiveram pontuação significativamente maior em testes de compreensão do que os que delegaram a geração e clicaram em "aceitar". O uso ativo constrói entendimento; a delegação passiva o erode.

> ❌ **Anti-padrão — Delegação passiva:** pedir código ao agente e clicar em "aceitar". **Por que as pessoas fazem isso:** é o caminho mais rápido para um diff pronto. **Consequência:** a compreensão cai, e o desenvolvedor não consegue avaliar a próxima saída. **Correção:** usar o agente para explicar o "porquê" por trás do código antes do merge.

No nível da equipe, o autor recomenda cinco alavancas deliberadas:
- **Política explícita de uso de IA:** onde a delegação autônoma é permitida e onde a revisão humana é obrigatória.
- **Revisão obrigatória de código crítico e relacionado à segurança:** não é lugar para aceite automático.
- **Lotes pequenos e ciclos rápidos de feedback:** pegar o erro do agente antes que ele se propague.
- **Avaliar a qualidade da especificação, não a contagem de linhas:** medir o que agora é o gargalo real.
- **"Dias sem IA" regulares:** para que o conjunto de habilidades centrais não enferruje.

O autor alerta para uma armadilha em que até equipes bem geridas caem: uma exigência top-down de "use IA" sem apoio cultural pode fazer mais mal do que bem. A pesquisa de Cat Hicks sobre o que ela chama de AI Skill Threat, isto é, estresse e incerteza sobre a identidade profissional, atinge de 43% a 45% dos engenheiros (um estudo com mais de 3.200 desenvolvedores). Uma cultura de aprendizado e pertencimento reduz essa ameaça, enquanto pressão baseada em competição e em "talento inato" a amplifica. Um líder que constrói uma cultura de aprendizado reduz a resistência; um que apenas aplica pressão a aumenta.

O plano concreto para a segunda-feira que o autor propõe: para você, a regra "entendo antes de fazer merge"; para a equipe, as cinco alavancas acima.

> ❌ **Anti-padrão — Mandato sem apoio:** ordenar "use IA" sem treinamento, normas ou um espaço seguro para aprender. **Por que as pessoas fazem isso:** a adoção é fácil de medir e de exigir. **Consequência:** sobem o estresse e a ameaça à identidade, e em seguida vêm a resistência ou o uso passivo. **Correção:** combinar o mandato com uma cultura de aprendizado e regras claras de revisão.

*Figura (legenda presente apenas na fonte):* duas trilhas para a mudança de papel; só uma constrói integradores ativos.

## O que isso significa para sua carreira
O artigo abre com uma frase de Eric Hoffer: em tempos de mudança, os que aprendem herdam a terra, enquanto os já instruídos estão admiravelmente equipados para um mundo que não existe mais (Reflections on the Human Condition, 1973).

O mercado está se polarizando e a barra está subindo, não caindo. O número de vagas júnior simples em empresas com forte adoção de IA está diminuindo, e o prêmio salarial para habilidades de IA e arquitetura está crescendo. Algumas projeções de mercado ainda mostram crescimento líquido do emprego no setor. Os dois sinais podem ser verdadeiros, dependendo da camada do mercado que você observa.

O autor nomeia diretamente o custo psicológico. Passar de criador que "molda o código como argila" para revisor do trabalho de outra pessoa atinge uma identidade construída sobre o ofício. A ansiedade sobre o próprio papel é uma resposta normal a uma mudança real, não sinal de fraqueza, e o silêncio sobre o tema a aprofunda.

A história oferece uma checagem de realidade. Cada camada de abstração chegou com a profecia de que eliminaria o papel do desenvolvedor, e em todas as vezes a profecia estava errada. Os compiladores deveriam permitir que analistas de negócio escrevessem programas sem especialistas. Linguagens de quarta geração e ferramentas CASE prometiam gerar aplicações a partir de especificações. O DevOps deveria eliminar o administrador de banco de dados, que virou engenheiro de plataforma. O padrão: uma barreira de entrada menor aumenta o volume de software, o que aumenta a demanda por engenheiros mais avançados. O SDD, na visão do autor, é a próxima camada, não o primeiro "fim".

Os vencedores não são os que dominaram melhor o ofício antigo, mas os que tratam o novo papel como aprendizado do zero. Apegar-se a "eu sou quem escreve o código mais rápido" é uma identidade ruim para o que vem a seguir. Adotar a identidade de autor de intenção e verificador é herdar o futuro.

*Figura (legenda presente apenas na fonte):* cada camada de abstração anunciou o fim dos desenvolvedores, e em cada vez elevou a barra.

## Resumo
A diferença entre o desenvolvedor de 2019 e o de hoje não é que um trabalhe menos. O valor deles migrou da velocidade de programação para a precisão da intenção e o rigor da verificação. O autor reduz toda a tese a essa frase.

As cinco ações concretas:
1. Ganhando valor: arquitetura, precisão de requisitos, julgamento e orquestração. Perdendo valor: boilerplate, depuração linha a linha e memorização de sintaxe.
2. O gargalo mudou, não desapareceu. "Testes passaram" não é "corresponde à intenção", e economia sem verificação é dívida, não ganho.
3. Inclua o tempo de verificação nas estimativas desde o início, em vez de tratar a geração de código como o fim do trabalho.
4. Como líder, construa uma cultura de aprendizado, não apenas um mandato. O autor afirma que integradores ativos superam delegadores passivos em todas as métricas que importam.
5. A profissão não está acabando. A barra está subindo, e o precedente histórico sustenta isso.

O autor encerra lembrando que o trabalho de um autor de intenção é mais difícil que o de um executor, mas também mais interessante. Menos tempo vai para digitar o que você já sabia escrever, e mais para o que exige pensamento: o que queremos construir e como provamos que construímos.

## Nota final
O autor pede que os leitores compartilhem uma mudança que notaram no próprio dia a dia de trabalho e que enviem o texto a um colega ansioso com a mudança. A mensagem se repete: a barra está subindo, e isso é uma boa notícia para quem continua aprendendo.

## 🧠 Modelos mentais
### Autor de intenção e supervisão da construção
**O modelo:** você é o arquiteto que escreve a planta, não o pedreiro. Sua entrega é uma especificação, e agentes de IA constroem o código sob sua supervisão.
**Como aplicar:** antes de gerar código, escreva o que significa "correto" como um contrato testável. No aceite, confira a construção contra esse contrato, e não apenas contra a suíte de testes.

### Dívida de verificação
**O modelo:** tempo economizado escrevendo código que não foi verificado não é economia; é adiamento para uma dívida invisível que volta com juros.
**Como aplicar:** estime o tempo de verificação como item de linha desde o início. Se você não consegue verificar uma mudança gerada, considere-a inacabada.

### Quem clica em merge é responsável
**O modelo:** a responsabilidade não pode ser delegada a uma máquina. Quem faz o merge responde pelo resultado.
**Como aplicar:** exija um revisor humano que consiga explicar mudanças críticas ou relacionadas à segurança antes do merge.

### Contexto, não apenas specs
**O modelo:** a saída do agente é tão boa quanto o contexto que ele recebe: especificação, restrições, exemplos e fragmentos relevantes do sistema, na ordem e na quantidade certas.
**Como aplicar:** quando um agente não atinge a intenção, primeiro verifique qual contexto ele recebeu, antes de reescrever o prompt.

## 🔑 Principais conclusões
- O gargalo no SDD passou de escrever código para definir a intenção e verificar os resultados. "Todos os testes passaram" não prova que o código corresponde à intenção.
- Equipes com alta adoção de IA viram o tempo de revisão de pull requests crescer quase 91% (Faros AI, mais de 10.000 desenvolvedores e 1.255 equipes), então a capacidade de revisão se torna a restrição.
- As evidências de produtividade dependem do contexto: a IA acelerou tarefas greenfield em experimentos controlados, enquanto a METR constatou que desenvolvedores experientes foram 19% mais lentos em repositórios brownfield grandes, embora acreditassem estar mais rápidos.
- 76% dos desenvolvedores relatam alucinações frequentes e baixa confiança no código de IA (Qodo, 2025), e essa lacuna de confiança é onde se forma a dívida de verificação.
- O uso ativo da IA para entender o código constrói compreensão; a aceitação passiva a erode (estudo piloto de Shen e Tamkin, cerca de 50 participantes).
- Uma cultura de aprendizado reduz a AI Skill Threat, que afeta de 43% a 45% dos engenheiros segundo a pesquisa de Cat Hicks, enquanto a pressão competitiva a aumenta.

## 🏛️ Para o Kenji
*Esta é a sua interpretação, não a do autor.* Para um caminho de Staff Architect, o artigo aponta para um papel em que você define fronteiras de sistemas, contratos de requisitos e critérios de aceite que os agentes então realizam, e em que sua formação em governança importa, porque "testes passaram" não é evidência de intenção. A pergunta que ele levanta para o seu trabalho: quais decisões de arquitetura no seu domínio precisam de um contrato de intenção escrito e verificável antes que qualquer agente toque nelas, e quem é o dono do aceite quando vários agentes constroem em paralelo?

## 📚 Plano de leitura
### Do artigo
- O texto do artigo não contém hiperlinks. Suas fontes são citadas na lista "Para se aprofundar" abaixo, e o único identificador informado é arXiv:2302.06590.

### Para se aprofundar
- Peng et al., experimento controlado com servidor HTTP (arXiv:2302.06590) — a evidência de produtividade em greenfield na qual o artigo se apoia.
- Estudo da METR sobre desenvolvedores experientes de código aberto em repositórios grandes — a contraevidência brownfield, lida com a nota do autor sobre a revisão posterior por erro de seleção da amostra.
- Shen e Tamkin, piloto de pesquisa da Anthropic sobre aprender uma nova biblioteca com IA — o resultado de compreensão entre "investigar o porquê" e "aceitar".
- Qodo, State of AI Code Quality 2025 — os 76% de alucinações e a baixa confiança.
- Dados de produtividade da Faros AI (mais de 10.000 desenvolvedores) — o crescimento de 91% no tempo de revisão.
- Cat Hicks, pesquisa sobre AI Skill Threat — o argumento de identidade e cultura de aprendizado para líderes.
- Guy Podjarny, podcast da Tessl, episódio 101 — a mudança de "spec" para "contexto".
- Sean Grove (OpenAI), sobre comunicação estruturada como a maior parte do valor de um engenheiro — o argumento da intenção como fonte.
- Eric Hoffer, Reflections on the Human Condition (1973) — a citação final sobre quem aprende e a mudança.

## Conceitos
- [spec-driven-development](/pt-br/concepts/software-engineering/spec-driven-development.md)
- [context-engineering](/pt-br/concepts/ai-ml/context-engineering.md)
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [multi-agent-orchestration](/pt-br/concepts/ai-ml/multi-agent-orchestration.md)
- [verification-tax](/pt-br/concepts/software-engineering/verification-tax.md)
- [technical-debt](/pt-br/concepts/software-engineering/technical-debt.md)
- [version-control-code-review](/pt-br/concepts/software-engineering/version-control-code-review.md)
- [software-engineering](/pt-br/concepts/software-engineering/software-engineering.md)
