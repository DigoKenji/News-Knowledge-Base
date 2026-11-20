---
type: Article Summary
title: Nova sugestão de Andrej Karpathy - pedir que a saída da IA seja em formato ASD-STE100, e não em texto
description: Para Karpathy, revisar a saída da IA exige redação restrita e ferramentas visuais descartáveis, e não prosa prolixa.
resource: https://medium.com/neuralnotions/andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100-format-and-not-in-text-format-b7b753f52f32
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/neuralnotions/andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100-format-and-not-in-text-format-b7b753f52f32
  title: Andrej Karpathy's New suggestion- Ask AI output in ASD-STE100 format and not in Text format
  author: Neural Notions
status: stable
source_url: https://medium.com/neuralnotions/andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100-format-and-not-in-text-format-b7b753f52f32
source: Medium (Neural Notions)
source_type: medium
published: '2026-10-03'
domain: ai-ml
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81dfad99ede5264acb83
lang: pt-br
translation_of: /notes/ai-ml/2026-11-20-andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100.md
---

## 🎯 Tese central
À medida que a IA faz mais trabalho autônomo, a principal função das pessoas passa a ser revisar, supervisionar e entender essa saída. Por isso, a saída deveria ser um texto restrito ou uma ferramenta visual rápida, e não uma prosa fluente.

## 🧭 Por que o autor escreveu
O autor acha cansativo ler texto bruto de IA: ele divaga, usa muitos sinônimos e enterra as conclusões principais sob enchimento polido. Se a tese se sustenta, a escolha do formato de saída passa a fazer parte da revisão do trabalho da IA, e deixa de ser um detalhe cosmético.

## Abertura: a premissa de Karpathy e as duas correções
O artigo parte da visão de Andrej Karpathy de que, à medida que os modelos assumem mais trabalho autônomo, o trabalho humano passa a ser revisar, supervisionar e compreender o que eles produzem. Karpathy propôs duas correções: um padrão de escrita restrito e a migração do texto para diagramas, páginas HTML interativas e animações explicativas descartáveis. O autor adotou as duas ao longo da última semana e relata o que aprendeu.

### ASD STE100
O artigo descreve o ASD STE100 como um padrão de manutenção aeronáutica dos anos 1970 e, em outro trecho, como o inglês técnico simplificado criado por fabricantes aeroespaciais europeus no fim dos anos 1970. Ele data o padrão de forma inconsistente entre essas duas passagens, então trate a década exata como afirmação do autor.

> 🔎 Contexto adicionado: o ASD STE100 é uma forma controlada de inglês para documentação técnica aeroespacial. Suas regras limitam o vocabulário e a estrutura das frases, para que as instruções sejam lidas sempre da mesma forma.

## O problema do FLUFF
O autor argumenta que, quando as pessoas dizem que a IA é ruim em trabalho técnico, geralmente querem dizer que a saída parece confusa. Os modelos são treinados para soar fluentes e expansivos, e isso cria ambiguidade. O autor chama o problema central de "FLUFF" e o relaciona ao setor aeroespacial, onde a ambiguidade em instruções técnicas é perigosa.

### Troca de sinônimos e ambiguidade
A ambiguidade aparece quando um modelo nomeia um mesmo objeto com várias palavras. O exemplo do artigo: um componente chamado de motor no primeiro parágrafo, de motor de propulsão no segundo e de unidade propulsora no terceiro. O leitor já não consegue saber se são uma peça ou três.

> ❌ **Antipadrão — Troca de sinônimos:** o mesmo objeto recebe um novo nome em cada parágrafo ("motor", "propulsor", "unidade propulsora"). **Por que as pessoas fazem isso:** a escrita fluente premia a variedade, e os modelos são treinados para soar expansivos. **Consequência:** o leitor não consegue saber se os termos se referem à mesma peça, então a revisão fica mais lenta e menos confiável. **Correção:** use um termo por objeto em todo o texto. Nas palavras do artigo, "Se um objeto é um parafuso, chame-o de parafuso para sempre."

### Regras de escrita do ASD STE100
O artigo apresenta três diretrizes centrais do padrão. Frases de procedimento devem ter menos de vinte palavras. Cada instrução descreve apenas uma ação. Nunca troque termos por variedade. O autor acrescenta aos prompts técnicos uma diretiva que pede ao modelo que siga o espírito do ASD STE100:

> Explique isto usando o espírito do ASD STE100. Mantenha estruturas de frase diretas, limite os passos de procedimento a ações únicas e evite sinônimos desnecessários.

O autor relata que as explicações dos modelos passaram a ler-se como manuais de oficina precisos, e não como um estagiário falante que enche linguiça. O artigo afirma que essa única restrição economiza muito esforço mental ao revisar bases de código, artigos de pesquisa ou propostas de arquitetura.

## Artefatos efêmeros
O autor considera a segunda parte da observação de Karpathy a mais transformadora. Escrever software sob medida costumava ser caro, então o código só valia a pena se muitas pessoas fossem usá-lo por meses ou anos. Agora o custo marginal de gerar código está próximo de zero, o que torna práticos artefatos grandes, personalizados e descartáveis.

### Artefatos efêmeros
Um artefato efêmero é uma ferramenta gerada cuja única função é transferir rapidamente um insight do modelo para a cabeça do leitor. O autor queria entender como a taxa de aprendizado afeta o gradiente descendente. Em vez de pedir uma explicação, que produziria quatro parágrafos sobre overshooting e convergência lenta, pediu um único arquivo HTML autocontido com um slider. O arquivo ficou pronto em cerca de trinta segundos. Depois que entendeu o mecanismo, o autor fechou a aba e apagou o arquivo.

> 🔎 Contexto adicionado: custo marginal é o custo extra de produzir mais uma unidade. Aqui significa o custo extra de gerar mais um trecho de código, que o artigo diz ser essencialmente zero.

> Gere um único arquivo HTML autocontido com Tailwind CSS e um script de canvas. Inclua um slider interativo para a taxa de aprendizado, para que eu possa ver uma bola rolando por uma superfície de erro em tempo real.

Com o slider em 0,001, a bola se arrastava. Em 1,5, a bola oscilava violentamente para fora da tela. Segundo o autor, a explicação inteira levou dez segundos de interação visual. Ele não precisava de software reutilizável. Precisava de uma ferramenta que existisse apenas para transferir um insight.

> ❌ **Antipadrão — Explicação em prosa por padrão:** pedir ao modelo que descreva um sistema dinâmico em parágrafos. **Por que as pessoas fazem isso:** é a forma usual de pedir uma explicação, e o artigo sugere que era o hábito do próprio autor. **Consequência:** o leitor precisa construir a imagem mental só com palavras, quando o comportamento é mais fácil de ver ao variar os parâmetros. **Correção:** para números ou sistemas, peça uma página interativa descartável que permita mudar os parâmetros.

## Nova hierarquia visual
A posição do autor é que o texto simples é o formato de menor largura de banda para entender algo rapidamente. Em seguida, o artigo apresenta a ordem que o autor usa agora ao pedir respostas. A ordem vai do formato mais eficaz ao menos eficaz para transferir entendimento.

### Animações procedurais e vídeo
O artigo diz que pedir a modelos roteiros de animações procedurais, no estilo do 3Blue1Brown, faz com que matemática abstrata e algoritmos fiquem claros de imediato. Combinar código visual gerado com síntese de voz transforma pesquisa densa em uma aula personalizada. Esse é o degrau mais alto.

### Micro apps interativos em HTML
Quando há números ou sistemas envolvidos, o autor pede uma página interativa com sliders, toggles ou entradas ao vivo. O artigo afirma que brincar com os parâmetros ensina dez vezes mais do que ler descrições estáticas. Esse é o degrau que o autor usou no exemplo da taxa de aprendizado.

### Diagramas estruturados
Quando uma página interativa é mais do que a tarefa exige, o autor pede diagramas Mermaid, layouts em SVG ou fluxos arquiteturais. O artigo diz que a organização espacial reduz a carga cognitiva rapidamente.

### Texto restrito
Quando o texto é necessário, o autor o restringe: frases curtas, verbos diretos e terminologia rigorosa. Esse é o último degrau antes do texto simples, que o artigo considera a escolha menos eficaz.

## 🧠 Modelos mentais
### Escrita procedural restrita
**O modelo:** mantenha as frases de procedimento com menos de vinte palavras, dê a cada instrução uma única ação e use um termo por objeto, como o ASD STE100 faz.
**Como aplicar:** inclua a diretiva do artigo nos prompts técnicos e confira se cada objeto na resposta mantém o mesmo nome.

### Ferramentas descartáveis
**O modelo:** quando gerar código custa quase nada, uma ferramenta feita para deixar um insight claro pode ser usada uma vez e descartada. Ela não precisa ser uma aplicação reutilizável.
**Como aplicar:** quando precisar entender um parâmetro, peça uma página interativa de arquivo único, altere as entradas e depois apague o arquivo.

### Ranking de largura de banda dos formatos
**O modelo:** formatos de saída diferem na velocidade com que transferem entendimento. O artigo ordena a animação procedural em primeiro lugar, depois as páginas interativas, depois os diagramas, depois o texto restrito, com o texto simples como o mais fraco.
**Como aplicar:** escolha o formato mais alto que se encaixe no conteúdo. Recorra ao texto restrito quando um visual seria exagero.

## 🔑 Principais conclusões
- O papel humano no trabalho assistido por IA passa a ser revisar e entender a saída do modelo, por isso o formato dessa saída importa.
- A prosa bruta do modelo é difícil de revisar porque divaga, varia o vocabulário e enterra os pontos principais.
- As regras do ASD STE100 (procedimentos com menos de vinte palavras, uma ação por instrução, um termo por objeto) reduzem a ambiguidade na saída da IA.
- Como gerar código é quase gratuito, uma página interativa descartável pode substituir uma explicação escrita com um propósito único.
- No teste do artigo, mover o slider da taxa de aprendizado entre 0,001 e 1,5 deixou o comportamento claro em cerca de dez segundos de interação.
- O artigo ordena os formatos pela velocidade com que transferem entendimento: animação procedural, páginas interativas, diagramas, texto restrito e, por último, texto simples.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. No trabalho de arquitetura, as regras de escrita restrita se aplicam a ADRs e propostas que usam um termo por componente, e os degraus visuais se aplicam a diagramas Mermaid em revisões de arquitetura. A pergunta que isso levanta: se um visual gerado é descartado após um uso, como você verificaria seus números e premissas antes de uma decisão depender dele?

## 📚 Plano de leitura
### No artigo
- O artigo não cita links.

### Para aprofundar
- ASD STE100 (inglês técnico simplificado) — o padrão aeroespacial cujas regras o autor aplica aos prompts.
- Diagramas Mermaid e layouts em SVG — o degrau de "diagramas estruturados" do artigo, útil para fluxos de arquitetura.
- Animação procedural estilo 3Blue1Brown — o degrau mais alto do artigo para explicar matemática abstrata e algoritmos.
- Observações originais de Andrej Karpathy sobre saída restrita e artefatos efêmeros — o artigo as resume sem linkar a fonte.
