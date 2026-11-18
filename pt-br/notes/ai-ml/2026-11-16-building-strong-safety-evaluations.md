---
type: Article Summary
title: Construindo avaliações de segurança robustas
description: 'Guia da Anthropic: uma afirmação de segurança só vale o que vale o seu eval, então meça o construto certo, valide os juízes LLM contra avaliadores humanos e reporte a incerteza.'
resource: https://www.anthropic.com/research/building-strong-safety-evaluations
tags:
- ai-ml
- ml-governance
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://www.anthropic.com/research/building-strong-safety-evaluations
  title: Building strong safety evaluations
  author: Anthropic
status: stable
source_url: https://www.anthropic.com/research/building-strong-safety-evaluations
source: Anthropic News
source_type: blog
published: '2026-11-13'
domain: ai-ml
subdomain: ml-governance
concepts:
- llm-as-a-judge
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d810a8885c55d50d4cd8a
lang: pt-br
translation_of: /notes/ai-ml/2026-11-16-building-strong-safety-evaluations.md
---

# Construindo Avaliações de Segurança Robustas

**Fonte:** https://www.anthropic.com/research/building-strong-safety-evaluations
**Autor:** Ruth Appel e colegas (Anthropic)
**Data:** 2026-11-13
**Tags:** #ai-safety #evals #methodology #anthropic

---

## 💬 Sobre o que é
O guia prático da Anthropic para construir evals de segurança, os testes por trás de afirmações como "este modelo é mais seguro que o anterior". Os autores argumentam que uma afirmação dessas só é tão sólida quanto o seu eval, e tomam emprestadas reformas da crise de replicação da psicologia (pré-registro, medição melhor, estatística mais forte) para compartilhar lições de como construíram evals na Anthropic.

## Segurança em primeiro lugar
Um eval de segurança mede a propensão de um modelo a causar ou contribuir para danos, não uma capacidade. A pergunta central é "Dada uma entrada específica, o modelo fez a coisa certa?" Evals são fáceis de manipular e, muitas vezes, documentados de forma insuficiente para serem reproduzidos.

## Monte uma equipe com as habilidades certas
Especialistas em métodos decidem como amostrar, avaliar e analisar. Especialistas de domínio (psicólogos, biólogos, cientistas políticos) decidem o que conta como dano e como é uma boa resposta. Se faltar qualquer um dos dois grupos, surgem evals rigorosos, mas estreitos, ou profundos, mas mal medidos.

## Execute evals com segurança
Execute evals de segurança em um sandbox sem acesso à internet, com monitoramento em tempo real para que a execução possa ser interrompida. Depois de seus próprios incidentes, a Anthropic reforçou seus ambientes e pausou muitos evals que davam acesso à internet aos modelos.

## Desenho
### Adeque o desenho à pergunta
A maioria dos evals é observacional: mostram que um modelo pontuou de forma diferente, não por quê. Afirmações causais exigem um desenho que varie uma coisa por vez, como o mesmo modelo com e sem uma mudança no prompt de sistema.

### Meça o que você quer medir
Isso é validade de construto. Recusa é o construto óbvio, mas, para suicídio e automutilação, muitas vezes é o errado: um modelo que recusa tudo teria desempenho excelente em um eval de taxa de recusa e ainda assim não seria mais seguro. A Anthropic reporta uma taxa de resposta apropriada, julgada por rubricas de especialistas. Em áreas de uso dual, como biologia, a resposta certa se engaja com o pedido legítimo e omite detalhes operacionais.

> 💡 Recusa costuma ser o construto errado. Meça se a resposta foi apropriada, não se ela recusou.

## Garanta que os resultados generalizem

### Capture nuances com simulações multi-turno
Prompts de turno único saturam e não captam a escalada gradual. A angústia se acumula, então uma revelação inicial muda como uma pergunta posterior, aparentemente comum, deve ser tratada.

### Desenhe cenários que modelem uma variedade de condições
Especialistas escrevem uma especificação (objetivos, táticas, persona, tom). O primeiro turno do usuário é fixo, um modelo Claude separado interpreta os turnos seguintes do usuário, o modelo testado responde, e cada cenário é executado várias vezes com combinações diferentes de traços. Um avaliador pontua cada transcrição. Varie as dimensões de forma independente para que seus efeitos possam ser separados.

### Leve em conta padrões de interação do mundo real
Construa cenários a partir do uso observado, de transcrições de red-team e de relatórios de incidentes, e continue revisando-os. Cubra idiomas, superfícies de produto e o harness em que o modelo roda, já que ferramentas e prompts de sistema mudam o comportamento.

## Execute

### Mantenha as medidas internamente válidas
Evite contaminação (use dados mais novos que o corte de treinamento), feche atalhos como reward hacking e randomize detalhes como a ordem das respostas. Suítes de turno único estão em taxas de resposta inofensiva entre 94% e 98%, então torne os evals saturados mais difíceis preservando o construto, e não com perguntas-armadilha.

### Calibre cuidadosamente o seu avaliador
A maioria dos evals modernos usa outro modelo como [LLM-as-a-judge](/pt-br/concepts/llm-as-a-judge.md), já que nenhuma equipe humana consegue ler todas as transcrições. Avaliadores podem falhar em silêncio, então o post recomenda:

- Use critérios atômicos de sim/não em vez de uma pontuação holística, e informe o que não atende a cada um deles.
- Mantenha o avaliador cego para os metadados do desenho, prefira um modelo diferente do que foi testado e verifique viés de tamanho e de posição.
- Falhe alto: sinalize e exclua os itens em que o avaliador quebra.
- Valide contra rótulos humanos com concordância corrigida por acaso.

### Pré-registre
Anote métricas, comparações e o limiar de melhoria antes de ver os resultados. Use 100 itens como piso para evals multi-turno complexos e 1,000 para os de turno único simples.

## Reporte

### Reporte as estatísticas por completo
Separe os resultados por categoria e sempre reporte a incerteza. Intervalos de multi-turno podem ultrapassar ±10 pontos. Em um eval de golpe romântico, o Claude Opus 5 e o Opus 5.5 marcaram 98% e 94%, mas a diferença não foi estatisticamente significativa, então não foi possível declarar um vencedor.

### Documente tudo
Reporte a configuração completa. Para o Claude Fable 5.1, a taxa de resposta apropriada diferiu em até 34 pontos entre a API bruta e o claude.ai, onde o prompt de sistema se aplica. Registre versões, configurações, prompts e a versão do avaliador, e nunca compare pontuações de avaliadores diferentes.

## Divulgue falhas com responsabilidade
Ao revisar transcrições sobre automutilação, a Anthropic percebeu que o Claude às vezes validava implicitamente a automutilação como estratégia de enfrentamento. Uma instrução no prompt de sistema contra isso elevou a taxa de resposta apropriada citada no claude.ai para 94%, ante 60% na API. A lição é executar evals continuamente durante o fine-tuning e avisar os desenvolvedores sobre falhas graves, dando tempo para uma correção antes de publicar.

## Padrões e técnicas identificados
- **Simulação multi-turno com abertura fixa:** um primeiro turno compartilhado e um modelo separado fazendo o papel do usuário, de modo que os cenários divergem de forma realista, mas continuam comparáveis.
- **Prompts pareados nocivos/benignos:** cada tema recebe os dois tipos, para que a recusa excessiva seja medida junto com a conformidade.
- **Rubricas binárias atômicas:** muitos critérios de sim/não em vez de uma pontuação única, o que facilita inspecionar o comportamento do avaliador.

## Principais conclusões
- Anote o construto antes de coletar dados; a maioria das falhas de eval começa por medir a coisa errada.
- A taxa de recusa é um mau proxy de segurança em áreas sensíveis, onde uma recusa uniforme pode ser ela mesma uma falha.
- Confie em um avaliador somente quando ele concordar com especialistas tão frequentemente quanto os especialistas concordam entre si.
- Reporte intervalos de confiança; uma diferença dentro deles é ruído, não uma vitória.

## Leitura complementar
- 📄 Designing evals for AI agents (Anthropic), sem URL no texto da fonte → o post anterior do autor sobre evals de capacidade, que contrasta com os evals de segurança.
- 🔗 Inspect framework (UK AI Security Institute), sem URL no texto da fonte → ferramenta de código aberto para executar, registrar e pontuar evals.
- 🔗 Petri toolkit (Anthropic), sem URL no texto da fonte → construído sobre o Inspect, automatiza os testes multi-turno descritos aqui.

---
*Resumo gerado: 2026-11-16*
