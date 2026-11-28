---
type: Paper Summary
title: 'Modelos de pesos, eclusas de ar e sandboxes: um chamado à validação técnica de testes de re-identificação sob as Diretrizes de Anonimização do EDPB'
description: Um teste de re-identificação que falha pode ser apenas fraco demais, então esses testes precisam de limites de detecção, viés e incerteza validados antes de contar como evidência de anonimato.
resource: https://arxiv.org/abs/2611.34859
tags:
- security
- governance-risk
- overfitting
- regularization
- model-evaluation
- ai-security
- security-controls
- threat-modeling
- security-risk-management
- large-language-model
- data-governance-quality
- differential-privacy
- membership-inference
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-28T06:47:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.34859
  title: 'Model Weights, Airlocks and Sandboxes: A Call for Technical Validation of Re-identification Tests under the EDPB
    Anonymisation Guidelines'
  author: Rickard Brännvall, Henrik Forsgren, Erik Ylipää, Aslak Felin, Michael Popoff, Susanne Stenberg
status: stable
source_url: https://arxiv.org/abs/2611.34859
source: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
source_type: arxiv
published: '2026-11-25'
domain: security
subdomain: governance-risk
concepts:
- overfitting
- regularization
- model-evaluation
- ai-security
- security-controls
- threat-modeling
- security-risk-management
- large-language-model
- data-governance-quality
- differential-privacy
- membership-inference
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/426baf74172d81028cd2ec0e7513337b
lang: pt-br
translation_of: /notes/security/2026-11-28-model-weights-airlocks-and-sandboxes-a-call-for-technical-va.md
---

## 🎯 Em uma frase
Um ataque de re-identificação que falha não prova que os dados ou o modelo são anônimos, porque o teste pode simplesmente ser fraco demais; os autores defendem que esses testes devem ser validados como métodos de medição, e seu exemplo trabalhado mostra que um teste com 1,000 registros e taxa de falso alarme de 0.1% só consegue detectar de forma confiável vazamentos que afetem cerca de 0.67% dos registros (cerca de sete registros), e somente se o ataque capturar todos os registros que vazam.

## ❓ O problema
Pela GDPR, os dados são anônimos apenas se nenhum indivíduo puder ser identificado por meios razoavelmente prováveis de serem usados. As Diretrizes 02/2026 do EDPB, publicadas para consulta em 7 de julho de 2026, aplicam esse teste também a modelos de IA, e pedem testes sem dizer como um teste de re-identificação deve ser desenhado, o que seu resultado prova ou como sua incerteza deve ser reportada. Essa lacuna importa porque um ataque que falha é ambíguo: pode significar que a publicação está bem protegida, ou que o teste era fraco demais para notar um vazamento. As duas ferramentas em que o artigo se concentra são a inferência de pertencimento (membership inference), um ataque que tenta adivinhar se um determinado registro estava nos dados de treinamento de um modelo a partir de como o modelo se comporta com ele, e a privacidade diferencial, um limite matemático para o quanto qualquer saída pode mudar quando os dados de uma pessoa são adicionados ou removidos, geralmente obtido pela adição de ruído aleatório.

## 💡 A ideia
A intuição central é que um ataque que falha é um resultado de medição, e como qualquer medição ele tem limites. Outros campos regulados, como diagnóstico in vitro, análise de resíduos de alimentos, água potável e validação de modelos bancários, aceitam um teste somente depois que ele foi verificado em casos cuja resposta verdadeira é conhecida, e somente com seu limite de detecção, viés e incerteza declarados. Os autores propõem o mesmo para os testes de re-identificação: validar o método antecipadamente em publicações de referência com vazamento conhecido (por exemplo, inserindo registros "canário" nos dados de treinamento), e então reportar o que o teste poderia ter detectado junto com cada resultado. Eles separam o método, o procedimento validado com seu ataque e premissas, da medição, o resultado de aplicá-lo a uma publicação específica.

## ⚙️ Como funciona
### O arcabouço: ambiente seguro, eclusa de ar e tipos de evidência
Os dados permanecem dentro de um ambiente seguro de processamento (SPE, secure processing environment), onde os usuários têm papéis definidos, seu processamento é registrado em log, eles não podem importar dados externos e o poder de computação é definido pelo operador. Toda saída passa por uma eclusa de ar (airlock), uma revisão de liberação: se passar, é liberada com seu log de auditoria, e se falhar, volta para revisão. A revisão avalia os destinatários, não os usuários internos; um destinatário conhecido sob controles comparáveis pode ser avaliado pela abordagem contextual, enquanto qualquer coisa publicada deve ser avaliada para qualquer entidade pela abordagem simplificada, que os autores consideram a escolha natural para publicação aberta. A evidência vem em três tipos. Evidência formal é uma garantia, como a privacidade diferencial, que vale para qualquer atacante; evidência empírica é um teste de ataque, em que o sucesso indica vazamento, mas a falha mostra apenas que esse ataque, sob suas premissas, não encontrou nada; evidência indireta é documentação e indicadores de risco, que informam um julgamento sem medi-lo.

### As sete questões em aberto
As questões 1 e 2 tratam da evidência empírica: como o limite de detecção, o viés e a incerteza de um teste devem ser estabelecidos e documentados (Q1), e o que um teste médio sobre muitos registros diz sobre um indivíduo (Q2, ilustrado por um pedido de exclusão após a publicação). As questões 3 a 5 tratam do cenário e da forma de uma publicação: como a abordagem contextual se aplica a usuários internos em um SPE (Q3), como a comparação se aplica a pesos publicados em comparação com um serviço de consulta (Q4), e como ataques previsíveis devem ser considerados quando uma publicação não pode ser retirada (Q5). A Questão 6 pergunta quando uma garantia formal pode sustentar a abordagem simplificada. A ilustração dos autores é que, com epsilon = 1, uma crença de 1% pode subir para no máximo 2.67% e uma crença de 50% para cerca de 73%, mas se tal mudança é significativa não é algo resolvido pela medição. A Questão 7 pergunta para quais publicações a evidência indireta sozinha basta, por exemplo o hospital do Cenário 7, com uma regressão logística pequena e fortemente regularizada.

### Pesos, eclusas de ar e sandboxes
Para pesos publicados, os ataques de inferência de pertencimento mais bem estabelecidos usam apenas as saídas de um modelo, então testes semelhantes são usados para pesos e para um serviço de consulta. Os pesos acrescentam meios ao atacante: consultas ilimitadas e inspeção direta dos parâmetros. Um serviço pode limitar consultas e restringir respostas a rótulos, mas ataques que usam apenas rótulos ainda podem ter sucesso, e uma campanha descrita no relatório da Anthropic usou cerca de 24,000 contas fraudulentas para mais de 16 milhões de trocas, então limites por conta podem ser contornados. Pesos publicados não podem ser retirados, e desaprender é caro e difícil de verificar, então o artigo favorece a proteção antes da publicação, como ruído, regularização ou privacidade diferencial no treinamento, mais um teste antes da publicação. Para um modelo treinado em uma eclusa de ar, o anonimato só pode se transferir se o destinatário não tiver informação ou meios além dos que os usuários internos têm; caso contrário, o modelo precisa de avaliação própria. Em um sandbox regulatório, a autoridade supervisiona, e os autores perguntam se um protocolo de ataque, métrica e limiar poderia ser fixado no plano do sandbox antecipadamente, observando que um provedor que escolhe os testes tem um conflito de interesses.

### Validação do método: limite de detecção, viés e incerteza
O exemplo trabalhado do artigo (Apêndice B) usa N = 1,000 registros de treinamento e uma taxa de falso positivo f = 0.001. Sem vazamento, o número de registros marcados X é em média cerca de 1, então são necessários quatro acertos antes que um resultado difira do acaso no nível de 5%, com um erro real de 1.9%. O limite de detecção é a menor fração de registros vazados que o teste marca com pelo menos 95% de probabilidade. O modelo de vazamento é:

```text
q = πr + (1 − π)f
```

Aqui q é a fração dos registros de treinamento que o ataque marca, π é a fração de registros que vazam, r é a sensibilidade (a probabilidade de o ataque marcar um registro que vaza) e f é a taxa de falso positivo. Com r = 1 o limite é π = 0.67%, cerca de sete registros; o mesmo teste tem um limite de 1.35% com r = 0.5 e de 6.8% com r = 0.1. Os autores dizem que o limite não pode ser lido como uma afirmação sobre vazamento enquanto r não for estabelecido por validação em casos com vazamento conhecido, que é o cerne do chamado deles por métodos de teste validados.

## 🧪 Evidência
| Cenário | Base de comparação | Este artigo | Métrica |
|---|---|---|---|
| Exemplo trabalhado, 1,000 registros de treinamento, f = 0.001, r = 1 | Sem vazamento: cerca de 1 acerto esperado; são necessários 4 acertos (erro real 1.9%) | Limite de detecção q = 0.77% (7.7 acertos esperados); π = 0.67%, cerca de sete registros | Fração dos registros de treinamento marcados |
| Mesmo teste com sensibilidade menor | Limite de detecção com r = 1 é 0.67% | r = 0.5: 1.35% (13.5 registros); r = 0.1: 6.8% (67 registros) | Fração dos registros vazados detectáveis |
| Teste maior, f = 0.001 | 1,000 registros: 0.67% (6.7 registros) | 10,000 registros: 0.13% (13.1 registros); 100,000 registros: 0.036% (36.4 registros) | Limite de detecção (Tabela 3) |
| Limite superior após um teste com f conhecido e 1 acerto | Acertos esperados por acaso: 1 | Limite superior de 95%: 4.7 acertos, 3.7 após subtrair o acaso, 0.37% dos registros (r = 1); sem acertos e f = 0: 0.30% | Limite superior do vazamento do ataque testado |
| Dados de referência para inferência de pertencimento, modelo de linguagem ajustado, FPR de 1% | Modelo de referência treinado com dados da mesma origem: 18.9% dos registros de treinamento identificados | Dados de referência de origem parecida, mas diferente: 4.9% (Mattern et al.) | Fração dos registros de treinamento identificados |
| Número de modelos de referência, um benchmark de imagens, FPR de 1% (estudo dos autores) | 64 modelos de referência: 12.9% identificados | 8 modelos de referência: 9.2% (uma variância para todos os registros) ou 2.7% (variância por registro) | Fração dos registros de treinamento identificados |
| Registro mais exposto vs. conjunto de dados inteiro, FPR de 0.1% (Aerni et al.) | Conjunto inteiro: 4% identificados | Registro mais exposto: 99.9% identificados | Fração dos registros identificados |

Os números são ilustrações e resultados citados de outros trabalhos; o artigo é uma posição e não apresenta novos experimentos. O exemplo trabalhado assume registros independentes, uma única taxa de falso positivo para todos os registros e um único ataque, e os autores observam que escolher entre ataques ou limiares depois de ver os resultados aumenta a taxa de erro. Os resultados citados mostram que o número de modelos de referência, os dados de referência e as premissas sobre o atacante podem alterar o resultado de um teste em várias vezes, que é o principal motivo que os autores dão para a validação.

## ⚠️ Limitações
- Os autores chamam o artigo de uma posição: seus cenários são ilustrativos e não cobrem todas as formas de publicação, e os métodos de teste não foram avaliados empiricamente neste trabalho.
- As leituras jurídicas são dos próprios autores e não foram verificadas contra a jurisprudência; eles deixam em aberto se o teste de anonimato se enquadra no Artigo 59 da AI Act, o que a documentação do teste deve conter, quais valores de parâmetros de privacidade são pequenos o suficiente, quão pequeno deve ser o risco individual e se a abordagem simplificada cobre ataques ainda não conhecidos.
- O escopo é o critério No Inference (não inferência), e a inferência de pertencimento é o principal exemplo; isolamento, vinculação, registros e agregados são em grande parte deixados de lado.
- A análise se refere às diretrizes como publicadas para consulta, que, segundo os autores, podem mudar.
- 🔎 Contexto adicionado: Ataques com modelos de referência multiplicam o custo de treinamento, já que cada modelo de referência é treinado como o modelo publicado, e o artigo não estima esse custo.
- 🔎 Contexto adicionado: O artigo é uma resposta à consulta, então os números de parágrafo que cita (por exemplo, § 71b) precisariam ser conferidos com o texto final do EDPB.

## 🏗️ O que isso significa para construir sistemas
Isto é interpretação, não afirmação dos autores. Em uma equipe que treina modelos com dados pessoais ou sensíveis, a publicação de um modelo se tornaria uma etapa controlada do pipeline: pesos ou uma API só saem depois de um teste de ataque pré-publicação, executado com um protocolo fixo, um limite de detecção registrado e um responsável independente da equipe do modelo. Para um serviço de consulta, as preocupações do artigo se mapeiam em controles de gateway, como autenticação, limites de taxa e respostas apenas com rótulos, e o orçamento de testes deve presumir um atacante com mais contas do que um único usuário. Os custos são computação para modelos de referência, esforço para validar o método em casos de vazamento conhecido e documentação retida para reavaliação posterior. Para confiar em um resultado, seria preciso a validação em casos de referência, os resultados para os registros mais expostos e uma condição declarada para novo teste, como um novo ataque publicado.

## 🔑 Principais conclusões
- Um ataque de re-identificação que falha mostra apenas que o ataque não encontrou nada sob suas premissas; não mostra que a publicação é anônima.
- O exemplo trabalhado (1,000 registros, taxa de falso positivo de 0.1%) detecta apenas vazamentos que afetam cerca de 0.67% dos registros, cerca de sete registros, e somente se o ataque capturar todos os registros que vazam.
- Um método de teste deve ser validado antecipadamente em casos de referência com vazamento conhecido, e cada resultado deve ser reportado com seu limite de detecção, viés e incerteza.
- Pesos publicados não podem ser retirados, então mitigações como privacidade diferencial, ruído ou regularização devem ser aplicadas antes da publicação; já um serviço de consulta pode ser retirado, embora as respostas já dadas permaneçam com seus destinatários.
- Uma grande diferença entre o desempenho de treinamento e de teste é evidência de vazamento, mas uma diferença pequena indica apenas risco menor e não descarta o vazamento; a evidência indireta pode triar publicações, mas não deve ser lida como prova.
- Em um sandbox regulatório, o protocolo de teste deve ser acordado com a autoridade antes de o teste ser executado, porque o provedor que seleciona os testes tem um conflito de interesses.

## 📚 Plano de leitura
### A partir do artigo
- [LeakPro](https://github.com/aidotse/LeakPro) — biblioteca de código aberto para testes de privacidade; os autores observam que ela informa estimativas de incerteza para algumas métricas, um ponto de partida prático para testes de inferência de pertencimento.
- [Formulário de anonimato da Findata para modelos de IA e ML](https://findata.fi/en/services-and-instructions/producing-anonymous-results/) — um exemplo real de evidência indireta: um formulário de 28 perguntas preenchido antes de um modelo poder sair de um ambiente seguro.
- [Orientações da ICO sobre anonimização](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/anonymisation/) — a visão do Reino Unido sobre anonimato dependente de contexto e o teste do intruso motivado.
- [Testbeds de PETs do NIST](https://www.nist.gov/itl/applied-cybersecurity/privacy-engineering/collaboration-space/pets-testbeds) — onde métricas empíricas de avaliação de privacidade estão sendo desenvolvidas.
- [Anthropic: detectando e prevenindo ataques de destilação](https://www.anthropic.com/news/detecting-and-preventing-distillation-attacks) — a fonte do número de contas para extração de modelos baseada em consultas.
- [Resposta da RISE à consulta do EDPB sobre anonimização](https://www.edpb.europa.eu/system/files/consultation_feedback/1801/feedback_1801_edpb-anonymisation-consultation-rise-reply-letter.pdf) — a própria resposta dos autores à consulta, que este artigo desenvolve.
### Para ir além
- Carlini et al., "Membership inference attacks from first principles" (IEEE S&P 2022) — o ataque LiRA que o artigo trata como o estado da arte em inferência de pertencimento.
- Steinke, Nasr e Jagielski, "Privacy auditing with one (1) training run" (NeurIPS 2023) — como auditar um modelo com privacidade diferencial inserindo canários.
- Wood et al., "Differential privacy: a primer for a non-technical audience" (2018) — uma porta de entrada para o parâmetro de privacidade epsilon, do qual a Questão 6 depende.
- NIST SP 800-226, "Guidelines for evaluating differential privacy guarantees" (2025) — como ler epsilon, delta e a unidade de privacidade.
- Yeom et al., "Privacy risk in machine learning: analyzing the connection to overfitting" (CSF 2018) — por que a diferença entre treino e teste é um indicador de vazamento.
- Giomi et al., "A unified framework for quantifying privacy risk in synthetic data" (a ferramenta Anonymeter) — validar uma medida de risco com vazamento conhecido inserido.

## Conceitos
- [Overfitting e generalização](/pt-br/concepts/ai-ml/overfitting.md)
- [Regularização e o trade-off entre viés e variância](/pt-br/concepts/ai-ml/regularization.md)
- [Avaliação de modelos e métricas](/pt-br/concepts/ai-ml/model-evaluation.md)
- [Segurança de IA e aprendizado de máquina adversarial](/pt-br/concepts/security/ai-security.md)
- [Controles de segurança](/pt-br/concepts/security/security-controls.md)
- [Modelagem de ameaças](/pt-br/concepts/security/threat-modeling.md)
- [Gestão de riscos de segurança e análise de impacto nos negócios](/pt-br/concepts/security/security-risk-management.md)
- [Modelo de linguagem de grande porte e modelo de fundação](/pt-br/concepts/ai-ml/large-language-model.md)
- [Governança de dados e qualidade de dados](/pt-br/concepts/data/data-governance-quality.md)
- [Privacidade diferencial](/pt-br/concepts/security/differential-privacy.md)
- [Inferência de pertencimento](/pt-br/concepts/security/membership-inference.md)
