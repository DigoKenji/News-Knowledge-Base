---
type: Article Summary
title: 8 coisas legais que as pessoas estão fazendo com o Claude Opus 5.5
description: O Opus 5.5 transforma prompts únicos em animações, aplicativos, cenas 3D, apresentações e jogos funcionando, com pouca supervisão.
resource: https://medium.com/generative-ai/8-cool-things-people-are-using-claude-opus-5-5-for-0dec7df4da4e
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/generative-ai/8-cool-things-people-are-using-claude-opus-5-5-for-0dec7df4da4e
  title: 8 Cool Things People Are Using Claude Opus 5.5 For
  author: Generative AI
status: stable
source_url: https://medium.com/generative-ai/8-cool-things-people-are-using-claude-opus-5-5-for-0dec7df4da4e
source: Medium (Generative AI)
source_type: medium
published: '2026-09-27'
domain: ai-ml
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81e2af56f5f5fafcf5d6
lang: pt-br
translation_of: /notes/ai-ml/2026-11-20-8-cool-things-people-are-using-claude-opus-5-5-for.md
---

## 🎯 Tese central
O Opus 5.5 consegue transformar um único prompt em resultados criativos e técnicos completos e bem acabados (animações, aplicativos interativos, cenas 3D, sites, cenas no Blender, apresentações, interfaces e jogos), com pouca orientação do usuário.

## 🧭 Por que o autor escreveu
O autor diz que o Opus 5.5 está "finalmente de volta" e que conclui os pedidos sem precisar de acompanhamento constante. Ele reúne oito exemplos do seu feed em redes sociais para mostrar o que as pessoas estão construindo nos primeiros dias após o lançamento, para que os leitores possam tentar projetos parecidos.

## 1. Animando histórias com JavaScript

### Animação quadro a quadro gerada em código
Kevin Ngo pediu ao Opus 5.5 uma história animada de 28 segundos em que cada quadro é produzido por JavaScript. O resultado tem estilo desenhado à mão: uma cidade noturna com estrelas, aviões de papel, uma criatura sorridente semelhante a uma estrela, uma menina escrevendo na janela e respostas representadas por cães e chá. O autor descreve a obra como uma mistura da estética de papel recortado com movimento gerado por código. A ideia é que a animação existe apenas como saída de programa, e não como arquivos de imagem ou vídeo.

### Animação com pinceladas e sem modelo de imagem ou vídeo
Um segundo criador pediu ao Claude que animasse a própria vida desde o dia zero até o presente, usando pinceladas feitas em JavaScript. O autor observa que o modelo não gera imagens nem vídeos, mas consegue produzir esse tipo de movimento. A explicação dele é que não é preciso nenhuma imagem de entrada, então o resultado depende de quão criativo for o prompt.

## 2. Construindo aplicativos interativos que explicam um conceito

### Explicadores interativos como formato de ensino
Ryan Sael pediu um laboratório interativo de lentes que explica o foco da câmera. Girar o anel de foco move os elementos de vidro, e o plano de foco nítido se desloca pela cena. O padrão é um aplicativo cujos controles correspondem diretamente ao mecanismo físico que está sendo ensinado, de modo que o aprendiz vê causa e efeito enquanto ajusta.

### Custo de construir esse tipo de aplicativo
O aplicativo inteiro levou 1 hora e 26 minutos em uma única execução e custou US$ 25,66 pela API. O autor estima que construir um aplicativo do mesmo nível com o Fable 5.1 ou com o GPT 6 Astra custaria centenas de dólares. Essa é uma estimativa dele, não uma comparação medida. Ele gostaria de ver mais aplicativos assim, como um app de teoria musical em que arrastar notas muda o acorde, ou um visualizador de ordenação que pode ser pausado e embaralhado de novo.

## 3. Gerando e animando objetos 3D

### Three.js e TSL em uma cena 3D gerada
A Techartist criou o "Architect's Model", uma experiência web interativa que transforma um esboço 2D rascunhado em uma casa 3D acabada. Ela foi construída inteiramente com Three.js e TSL, e o Opus 5.5 escreveu o código.

> 🔎 Contexto adicional: o Three.js é uma biblioteca JavaScript para gráficos 3D no navegador. O TSL é uma linguagem de sombreamento baseada em nós usada com o Three.js.

### Revelação em etapas como visão do histórico de design
O espectador pode orbitar, rolar e navegar por quatro estágios: traçado inicial, volumetria básica, detalhamento adicional e o edifício acabado. O autor observa que voltar no tempo é a parte mais útil, porque mostra quais decisões produziram a forma final.

### Camada de apresentação, não CAD
O autor vê isso como uma forma de arquitetos e engenheiros apresentarem conceitos a clientes, que muitas vezes têm dificuldade com plantas, mas conseguem usar um controle deslizante. O alerta dele: "Só não confunda isso com CAD." O modelo não tem dimensões reais, dados estruturais e nada que possa ser construído. É uma camada de apresentação sobre o trabalho de projeto de verdade.

> ❌ **Anti-padrão — confundir um modelo de apresentação com um modelo de projeto:** uma casa 3D visualmente convincente sem dimensões nem estrutura. **Por que as pessoas fazem isso:** o resultado visual parece acabado e persuasivo. **Consequência:** as partes interessadas podem ler o visual como um projeto construível. **Correção:** mantenha o modelo de apresentação separado dos artefatos reais de projeto (CAD, dados estruturais) e diga isso ao apresentá-lo.

### Comparação de modelos com uma fatia de geleia
Outro criador fez uma fatia de geleia elástica e comparou o resultado do Opus com uma versão Astra. O criador preferiu o Opus pela cor, pela superfície brilhante e pela forma como a fatia se dobra e treme depois de solta.

### Fixar dependências quando a biblioteca está mudando
A dica do autor: o TSL é novo e ainda está mudando, então fixe a versão do Three.js e cole a documentação no contexto. Sem isso, o modelo vai recorrer a APIs que não existem mais.

> ❌ **Anti-padrão — gerar código contra uma API sem versão fixada e em movimento:** deixar o modelo escrever com base na versão que ele lembra. **Por que as pessoas fazem isso:** funciona na primeira tentativa e o resultado parece atual. **Consequência:** código que chama APIs removidas ou renomeadas. **Correção:** fixe a versão da biblioteca e forneça a documentação atual no contexto, como o autor sugere.

## 4. Projetando sites com elementos 3D

### Construindo experiências web 3D sofisticadas a partir de um tutorial
O vídeo tutorial de 12 minutos de Viktor Oddy mostra como construir, passo a passo, sites 3D interativos de alto padrão. Os visuais e os efeitos de movimento são do tipo que, segundo o criador, costumam custar cerca de US$ 50 mil para produzir. O autor trata isso como um sinal de pressão sobre plataformas como a Lovable, que criam designs de sites com IA. O ponto dele: o usuário pode pedir ao Claude que construa o aplicativo diretamente dentro da sua IDE.

> 🔎 Contexto adicional: uma IDE é um ambiente de desenvolvimento integrado, o editor onde engenheiros escrevem e executam código.

## 5. Trabalhando dentro do Blender

### Animação 3D agêntica no Blender
Alex Albert, da Anthropic, destacou a capacidade do Opus 5.5 de gerar vídeos de claymation em stop-motion de qualidade profissional no Blender a partir de um único prompt no [claude.ai](http://claude.ai). O modelo planeja as cenas, renderiza os quadros, verifica o resultado e monta uma animação de cerca de 20 segundos. O autor toma isso como evidência de quão agêntico o Claude se tornou: ele executa sozinho um pipeline criativo de várias etapas.

> 🔎 Contexto adicional: o Blender é uma suíte de criação 3D de código aberto. Ele é programável por meio de uma API Python.

### Espere desvio de versão
O autor espera alguns erros quando as pessoas tentarem isso, porque a API Python do Blender muda entre versões. Ainda assim, ele pretende testar.

## 6. Criando apresentações de marca

### Apresentações completas de marca a partir de um prompt
O autor já recomendou ferramentas de apresentação com IA, como Gamma, Plus AI e NotebookLM. Ele sugere que o Claude com Opus 5.5 talvez já esteja no topo dessa lista. A apresentação que a Yahyavision compartilhou tem um visual futurista coeso: um logotipo próprio com elementos entrelaçados, o slogan "Every AI Agent, One Index", maquetes de sites, ícones de aplicativos e uma paleta de verde-azulado e ciano com Electric Lime e Cerulean Blue. Ele planeja uma comparação com o Gemini Notebook nos próximos dias.

## 7. Construindo estruturas de frontend e painéis

### Substituindo kits de UI comprados por kits gerados
O autor não é designer de frontend, e é justamente no design que ele gasta a maior parte dos tokens, porque faz centenas de ajustes até acertar. Ele ficou impressionado com os painéis, bibliotecas de UI, componentes e ícones de movimento gerados. Ele se pergunta se ainda precisa comprar bibliotecas e templates premium.

### Produtividade de designers
A Tanjim compartilhou uma UI que um designer de produto sênior construiu em uma hora, mais outra hora de acabamento. Ela diz que o mesmo trabalho levaria cerca de uma semana de outra forma. A Tanjim, que fundou um estúdio de design de produtos com IA, argumenta que designers que usarem ferramentas como o Opus 5.5 vão se tornar "imparáveis".

## 8. Criando jogos jogáveis

### Jogo de corrida 3D construído com IA
O autor diz que esta é provavelmente a principal razão para se animar com o Opus 5.5: ele consegue construir jogos complexos e de alta qualidade. A simulação de direção em Three.js atualizada por Chetan Ankola permite que o jogador dirija por estradas sinuosas e surreais, desenhadas à mão, passando por ilhas flutuantes e cenas como Lemon Spiral e Möbius Arcade. O vídeo mostra ângulos de câmera dinâmicos e controles de estúdio em tempo real para efeitos de tinta, iluminação e trilhas sonoras sincronizadas com faixas como "Very Slow Waves". O autor destaca como a interface, a animação, os efeitos e a jogabilidade funcionam juntos.

## Considerações finais

### Uso real em produtos
O autor usou o Opus 5.5 para aprimorar partes de seus próprios produtos, incluindo o sistema de automação da Zeniteq. Ele relata que até agora funcionou sem falhas para ele e que consome muito menos tokens do que outros modelos de fronteira que testou para esse trabalho. Ele tem mais curiosidade sobre a integração com o After Effects e o Blender. Não é animador, mas o exemplo de claymation fez com que ele quisesse criar algo para seus sites ou artigos. Ele convida os leitores a experimentarem uma versão pequena de um dos exemplos e compartilharem o resultado.

## 🧠 Modelos mentais
### Prompt como única entrada
**O modelo:** o ponto do autor na seção 1 é que um modelo sem geração de imagem ou vídeo ainda pode produzir movimento convincente, porque a saída é código e a qualidade depende de como a tarefa é descrita.
**Como aplicar:** quando o alvo é visual, mas o modelo não consegue renderizá-lo diretamente, peça o artefato como código (JavaScript, Three.js, Python para Blender) e julgue o resultado executando-o.

### Camada de apresentação versus fonte da verdade
**O modelo:** um artefato 3D ou de slides visualmente persuasivo é uma ferramenta de comunicação. Ele não carrega os dados estruturados de que o projeto real precisa.
**Como aplicar:** rotule os visuais gerados como apresentações e mantenha separados os artefatos que são fonte da verdade (CAD, especificações, dados).

### Fixe as partes móveis
**O modelo:** quando uma biblioteca ou API é nova e ainda está mudando, o código gerado tende a derivar para o que o modelo lembra.
**Como aplicar:** fixe a versão da biblioteca e forneça a documentação atual no prompt antes de pedir código.

## 🔑 Principais conclusões
- O Opus 5.5 produziu projetos criativos completos e executáveis só a partir de prompts, incluindo animação construída como saída JavaScript quadro a quadro.
- Um explicador interativo levou 1 hora e 26 minutos em uma única execução e custou US$ 25,66 pela API, segundo o artigo.
- Cenas 3D geradas são camadas de apresentação: não têm dimensões nem estrutura reais, então não são CAD.
- Com o TSL ainda mudando, fixe a versão do Three.js e cole a documentação atual no contexto, senão o modelo usa APIs que não existem mais.
- O modelo consegue planejar, renderizar, verificar e montar uma animação de claymation de cerca de 20 segundos no Blender a partir de um único prompt, segundo a leitura do autor sobre a publicação da Anthropic.
- O autor relata que usa o Opus 5.5 na automação dos próprios produtos e que ele consome menos tokens que outros modelos de fronteira nesse trabalho.

## 🏛️ Para o Kenji
Interpretação sua, não do autor: para um arquiteto, a parte útil é a separação entre um artefato de apresentação convincente e o registro real de projeto, já que o próprio autor alerta que a casa 3D não tem dimensões nem dados estruturais. A pergunta que isso levanta é quais partes do seu próprio fluxo de projeto poderiam usar visuais ou protótipos gerados com segurança para comunicação com clientes, e quais precisam continuar vinculadas a artefatos de projeto governados, com versões de ferramentas fixadas.

## 📚 Plano de leitura
### A partir do artigo
- [claude.ai](http://claude.ai): a plataforma de onde o exemplo de claymation no Blender foi gerado a partir de um único prompt, citada na seção 5.

### Para ir além
- Documentação de anúncio e de modelo da Anthropic para o Opus 5.5: para confirmar as capacidades e os preços que o autor descreve.
- Documentação do Three.js e do TSL: necessária se você fixar versões, como o autor recomenda na seção 3.
- Notas de versão da API Python do Blender: para verificar o desvio de versão que o autor espera na seção 5.
