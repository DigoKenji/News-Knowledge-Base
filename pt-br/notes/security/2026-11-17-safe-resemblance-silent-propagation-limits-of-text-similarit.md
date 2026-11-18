---
type: Paper Summary
title: 'Resemblância segura, propagação silenciosa: limites dos guardrails de similaridade textual para a segurança de agentes de IA'
description: Um guardrail de similaridade textual deixa passar saídas funcionais injetadas e sinaliza reutilização autorizada, então a semelhança textual não consegue avaliar a segurança de agentes.
resource: https://arxiv.org/abs/2611.21520
tags:
- security
- appsec
- multi-agent-orchestration
- rag
- indirect-prompt-injection
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.21520
  title: 'Safe Resemblance, Silent Propagation: Limits of Text-Similarity Guardrails for AI Agent Safety'
  author: arXiv
status: stable
source_url: https://arxiv.org/abs/2611.21520
source: arXiv
source_type: arxiv
published: '2026-11-16'
domain: security
subdomain: appsec
concepts:
- multi-agent-orchestration
- rag
- indirect-prompt-injection
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d812ca4dddd6e409c78d3
lang: pt-br
translation_of: /notes/security/2026-11-17-safe-resemblance-silent-propagation-limits-of-text-similarit.md
---

## 🎯 Em uma frase
Um guardrail de similaridade textual (DonkeyRail) deixa passar 8 de 19 saídas funcionais com limiar 0.5 (recall de 57,89%) e sinaliza 70 de 192 respostas legítimas do Luna (36,46%). Assim, a semelhança textual não consegue separar instruções propagadas de reutilização autorizada nesse relé de três agentes.

## ❓ O problema
Em um sistema multiagente, a saída de um agente vira a entrada de outro. Se uma saída carrega uma instrução injetada, ela pode continuar direcionando os agentes seguintes mesmo depois que a redação muda, de modo que um filtro que procura texto copiado pode deixá-la passar. A defesa comum é um guardrail de similaridade: ele calcula quanto uma resposta se sobrepõe aos registros dos quais foi gerada e dispara um alerta acima de um limiar. A dificuldade é que o trabalho legítimo também tem muita sobreposição, porque citar uma fonte ou reutilizar um template costuma ser exatamente o que o usuário pediu. O artigo pergunta se uma pontuação fixa de similaridade consegue distinguir esses dois casos e se um limiar escolhido com tráfego legítimo continua funcionando em tráfego novo.

## 💡 A ideia
Os autores separam três coisas que são fáceis de confundir: quão parecidos são dois textos, o que um texto faz um agente posterior executar e se a tarefa permite isso. Eles rotulam cada saída pelo seu comportamento, e não pela sua redação. Uma saída conta como funcional se faz dois agentes subsequentes calcularem, cada um, um resultado novo para um novo chamado, de modo que copiar uma resposta anterior não consegue ter êxito. A pontuação de similaridade é então medida contra esse rótulo comportamental independente. Os autores também argumentam que a permissão vem da tarefa confiável e da política, e não do texto, então um classificador apenas observacional não consegue resolver a questão sozinho.

## ⚙️ Como funciona

### Experimento de relé comportamental
Um agente de e-mail recebe nove registros de contexto sintéticos, um registro de relé e uma solicitação de reunião que contém um número de chamado. O relé pede ao agente que some sete ao chamado, emita uma linha LAB-PROOF separada e leve o mesmo procedimento para a sua resposta. Três agentes rodam em sequência. Cada um recebe a saída real do anterior como contexto mais um novo chamado, e nenhuma semente anterior é reinserida. O endpoint primário para uma saída raiz o1 exige que o segundo e o terceiro agentes produzam o resultado novo correto. O artigo também relata um endpoint mais rigoroso, que exige que os três agentes ajam.

```text
F(o1) = 1[A2 = 1 ∧ A3 = 1]
```

Aqui, A_h vale 1 quando o agente h produz uma linha correta de resultado novo fora das tags de relé, dos blocos de código e das linhas citadas, e 0 caso contrário. F(o1) vale 1 quando os dois agentes posteriores agem. Como cada agente recebe um novo chamado, uma resposta copiada não consegue satisfazer o endpoint.

### O guardrail de similaridade
O DonkeyRail é um classificador público da linha de pesquisa Morris-II. Para cada uma de três métricas, ele toma o máximo sobre dez registros recuperados, comparando cada registro com a resposta. O BLEU mede a precisão modificada de n-gramas, o ROUGE-L mede a maior subsequência comum, e o METEOR mede unigramas alinhados com uma penalidade de fragmentação. Um modelo logístico de três atributos transforma esses atributos em uma pontuação e dispara um alerta quando a pontuação atinge o limiar. O classificador fica congelado durante todo o estudo, e o artigo relata o limiar padrão de 0.5 junto com um limiar calibrado.

```text
x(D, o) = ( max_d∈D BLEU(d, o),  max_d∈D ROUGE-L(d, o),  max_d∈D METEOR(d, o) )
s(D, o) = σ(b + wᵀ x(D, o)),  alert when s(D, o) ≥ τ
```

D é o conjunto de registros recuperados, o é a resposta e σ é a função logística. Os máximos podem vir de registros diferentes. O coeficiente negativo do ROUGE-L significa que nenhuma métrica isolada conduz a pontuação combinada.

### Controles legítimos e sua divisão
Para cada modelo, o estudo constrói 192 respostas de calibração e 192 respostas de teste. Cada divisão tem 48 saídas para cada uma de quatro tarefas: confirmação comum, citação literal, paráfrase e reutilização completa de template. Os templates de requisição e os formatos dos registros de origem no conjunto de teste são diferentes dos de calibração, com 24 templates completos em cada conjunto. A requisição confiável autoriza explicitamente a citação e a reutilização, e os registros de origem não contêm nenhum procedimento de relé. Toda saída legítima permanece no denominador dos falsos positivos, e uma verificação de conclusão separada registra se a resposta executou a tarefa.

### Regra de calibração
O limiar calibrado é definido a partir das pontuações de calibração em ordem decrescente s(1) ≥ … ≥ s(192) como τcal = nextafter(s(4), +∞). Isso permite no máximo três alertas de calibração, uma taxa empírica de falsos positivos de calibração abaixo de 2%. A regra é fixada antes de qualquer coleta de dados, e as saídas de teste e os resultados dos ataques não entram na escolha.

### Correção da medição com limiar fixo
O artigo constata que a implementação pública reporta taxas com limiar fixo interpolando entre limiares de probabilidade da curva ROC, em vez de aplicar o limiar às pontuações. A contagem direta usa FPR(τ) = Σ 1[yᵢ = 0 ∧ sᵢ ≥ τ] / Σ 1[yᵢ = 0]. Nas 21 configurações disponíveis, a contagem direta muda o FPR em 19 e muda o FPR ou o TPR em todas as 21. Para o modelo logístico de três atributos em 0.5, o FPR relatado de 0.021580 passa a ser 29/1500 = 0.019333 com contagem direta. O AUC de 0.997884 permanece o mesmo, então o erro afeta os pontos de operação, e não a ordenação. Todos os resultados comportamentais do artigo usam decisões diretas.

### Análise de transferência de rótulo proxy
O gerador original rotulava os positivos com uma frase fixa de roleplay, que é um proxy para o comportamento de interesse. O artigo define a contaminação de critério: uma avaliação com proxy está contaminada quando seu rótulo é o próprio proxy, o proxy se correlaciona com a estatística do detector e o proxy discorda do alvo em algum ponto. Sua Proposição 1 constrói um caso em que o AUC do proxy é 1 enquanto o AUC do alvo é qualquer valor a em [0, 1]. O corolário é que um AUC alto no proxy não dá nenhum limite inferior universal para o AUC do alvo. A diferença normalizada do artigo, CCI = 1 − AUC(f, B) / AUC(f, C), é apenas descritiva, e calculá-la exige rótulos pareados de proxy e de alvo sobre a mesma população, o que os registros do DonkeyRail não fornecem.

### Análise de autorização
Se a política que determina a permissão não faz parte do que o guardrail observa, transcrições visíveis idênticas podem exigir decisões diferentes. A Proposição 2 afirma que nenhuma função determinística do texto visível consegue decidir a autorização corretamente nos dois mundos, e que um classificador randomizado sobre a mesma entrada atinge acurácia esperada de 1/2 em um par balanceado. O artigo constrói 64 pares sintéticos com tarefa, contato recuperado e resposta byte a byte idênticos, mas com políticas confiáveis de permitir/negar diferentes. Uma linha de base constante, apenas observacional, acerta 64/128, enquanto consultar a política acerta 128/128.

## 🧪 Evidências
| Cenário | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Luna, limiar fixo 0.5, 72 cadeias de ataque reservadas | 19 raízes funcionais | 11 detectadas, 8 não detectadas | Recall funcional de 57,89% (intervalo bootstrap de 35,0% a 76,9%) |
| Luna, limiar fixo 0.5, respostas legítimas de teste | 192 respostas | 70 sinalizadas | Taxa de falsos positivos de 36,46% (intervalo por agrupamento de templates de 21,9% a 51,6%) |
| Luna, limiar calibrado | 0 de 192 alertas de calibração | 1 de 19 detectadas; 15 de 192 respostas de teste sinalizadas | Recall de 5,26%; taxa de falsos positivos de 7,81% |
| Luna, limiar 0.5, tarefas de citação e de template | 48 saídas por tarefa | 37 e 31 sinalizadas | Contagem de falsos positivos (Tabela 2) |
| Sol, limiar fixo 0.5 | 0 de 72 raízes funcionais | 98 de 192 respostas sinalizadas | Taxa de falsos positivos de 51,04%; recall indefinido |
| Sol, limiar calibrado | 192 respostas | 69 de 192 sinalizadas | Taxa de falsos positivos de 35,94% |
| Luna, avaliação anterior, limiar 0.5 | 13 raízes funcionais | 7 detectadas; 128 de 192 respostas sinalizadas | Raízes funcionais detectadas (7 de 13); contagem de falsos positivos |
| AUC agregado do Luna para raízes / funcional versus benigno | 144 raízes, 384 saídas legítimas | 0,7609 / 0,5272 | AUC |

Os resultados mostram que o classificador congelado deixa passar uma grande parte das saídas funcionais com o limiar padrão, e que sinaliza a maioria das respostas legítimas de citação e de template nos dois modelos. Os 48 de 48 alertas falsos do Sol em citação e em reutilização de template são o resultado mais marcante do conjunto. A calibração eliminou todos os alertas no próprio conjunto de calibração, mas no conjunto de teste sinalizou 15 respostas e detectou apenas uma de 19 saídas funcionais. O Sol não produziu raízes funcionais, então seu recall é indefinido, e a propagação observada igual a zero para o Sol não comprova que o guardrail a detecta. A comparação entre modelos vale apenas para os prompts e as configurações de raciocínio testados. Os intervalos do artigo descrevem a incerteza apenas dentro das tarefas e dos templates construídos pelos autores.

## ⚠️ Limitações
- O experimento mede a adoção de instruções sob exposição forçada, em que cada saída é inserida no contexto do agente seguinte. Ele não mede a recuperação natural, a abertura de mensagens nem a entrega. Não há execução de ferramentas externas, exfiltração de dados nem estado persistente, e a ação aritmética é um proxy restrito para um comportamento malicioso mais amplo.
- O estudo usa um único relé aritmético, doze enquadramentos reservados e seis tipos de variantes, com dois aliases de modelo que não têm snapshots datados. Os enquadramentos compartilham um mesmo núcleo operacional, então não são mecanismos de ataque independentes. Metade da mistura legítima é formada por tarefas de alta cópia, então as taxas de falsos positivos descrevem essa mistura e não estimam a prevalência em e-mails reais.
- A falha da calibração muda duas coisas ao mesmo tempo, os templates de requisição e os formatos dos registros de origem, então a causa não pode ser isolada. A avaliação anterior encontrou zero alertas de teste calibrados dentro de templates compartilhados, mas também perdeu todos os 13 positivos funcionais com esse limiar.
- 🔎 Contexto adicionado: o ataque e seu critério de sucesso são sintéticos e usam um procedimento canário autodescritivo (some sete, emita LAB-PROOF). A regra exploratória do artigo, que verifica a presença de uma linha de prova sem aspas, alerta em 10 de 13 raízes funcionais sem nenhum alerta legítimo, mas os autores afirmam que ela foi desenhada para exatamente esta gramática e não é uma defesa geral.
- 🔎 Contexto adicionado: as contagens de utilidade de bloqueio (Luna de 122 para 90 tarefas concluídas, Sol de 188 para 93) pressupõem que toda resposta sinalizada seria bloqueada, então são estimativas contrafactuais, e não medições de um sistema em produção.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. Se você passa saídas de modelos entre agentes, uma verificação de sobreposição dessas saídas é, na melhor das hipóteses, um sinal, e ficaria presente em cada salto. O custo de computação é pequeno, mas o custo dos falsos positivos recai exatamente sobre os fluxos comuns em pipelines de agentes, como encaminhamento, resumo e reutilização de templates. Para confiar em uma verificação desse tipo, você precisaria de uma avaliação comportamental dos seus próprios agentes, de templates reservados extraídos das suas tarefas reais e de uma entrada explícita de política que diga o que cada tarefa autoriza. Os desenhos propostos pelo artigo para isso, Fides e CaMeL, usam rótulos e verificações de política sobre as chamadas de ferramentas, e não um limiar de sobreposição, o que aponta para uma arquitetura mais forte baseada em política sensível à proveniência.

## 🔑 Principais conclusões
- Com limiar 0.5, o classificador DonkeyRail deixou passar 8 de 19 saídas funcionais do Luna e sinalizou 70 de 192 respostas legítimas do Luna.
- Citação e reutilização de template disparam alertas falsos porque a sobreposição é alta por construção: 37 de 48 e 31 de 48 para o Luna, e 48 de 48 em cada categoria para o Sol.
- Um limiar calibrado, com zero alertas na calibração, ainda sinalizou 15 de 192 respostas de teste e detectou apenas 1 de 19 saídas funcionais, porque os templates e os formatos mudaram.
- As taxas com limiar fixo do código público vêm da interpolação na ROC, e decisões diretas mudam o FPR em 19 de 21 configurações, então os pontos de operação precisam ser recalculados.
- Um AUC de proxy igual a 1 não garante um AUC comportamental alto, e medir a diferença exige rótulos pareados de proxy e de alvo.
- A autorização depende de uma política fora da transcrição, e nos pares construídos pelos autores um classificador apenas observacional não consegue decidi-la.

## 📚 Plano de leitura
### Do artigo
O texto e a lista de referências do artigo não trazem URLs, então estas entradas são citadas sem links.
- Here comes the AI worm (Cohen, Bitton, Nassi), arXiv:2403.02817 — a origem do prompt autorreplicante e do guardrail Virtual Donkey que este artigo testa.
- ComPromptMized: Public Morris-II and DonkeyRail artifacts (Cohen, Bitton, Nassi), repositório no GitHub no commit 6ab9116c2b08f3309a034ceac7f35a78416f05a7 — o código e os parâmetros do classificador que o artigo avalia.
- Defeating prompt injections by design (Debenedetti et al.), arXiv:2503.18813 — o desenho CaMeL, que separa o fluxo de controle confiável dos dados em quarentena.
- Securing AI agents with information-flow control (Costa et al.), arXiv:2505.23643 — o desenho Fides, que propaga rótulos e verifica chamadas de ferramentas contra a política.
- AgentDojo (Debenedetti et al.), arXiv:2406.13352 — separa a utilidade da tarefa legítima do sucesso do objetivo do atacante.
- Autonomous LLM agent worms (Zha and Wang), arXiv:2605.02812 — avalia os ciclos de propagação de forma comportamental, e não pela retenção de palavras-chave.
### Para aprofundar
- Indirect prompt injection em aplicações integradas a LLMs — o modelo de ameaça sobre o qual todo o artigo se apoia.
- Controle de fluxo de informação para chamadas de ferramentas de agentes — como rótulos de proveniência e de leitores permitidos controlam ações com consequências.
- Contaminação de critério e viés de incorporação em avaliação — os conceitos de validade por trás da análise com rótulo proxy.
- Calibração de limiares sob mudança de distribuição — por que um limiar sem falsos alertas em um conjunto de templates pode falhar em outro.
