---
type: Article Summary
title: Combatendo a destilação adversarial
description: A destilação adversarial pode copiar capacidades de modelos de fronteira sem as salvaguardas, por isso a OpenAI agora protege o raciocínio oculto dos modelos em requisições, APIs e campanhas.
resource: https://openai.com/index/combatting-adversarial-distillation
tags:
- security
- distillation
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://openai.com/index/combatting-adversarial-distillation
  title: Combatting adversarial distillation
  author: OpenAI News
status: stable
source_url: https://openai.com/index/combatting-adversarial-distillation
source: OpenAI News
source_type: blog
published: '2026-11-20'
domain: security
concepts:
- distillation
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81dcaa04d2672affa431
lang: pt-br
translation_of: /notes/security/2026-11-21-combatting-adversarial-distillation.md
---

## 🎯 Tese central
A destilação adversarial pode copiar as capacidades de um modelo de fronteira sem as suas salvaguardas, por isso proteger o raciocínio intermediário do modelo nos níveis de requisição, API e campanha é uma parte central da defesa contra ela.

## 🧭 Por que o autor escreveu
O artigo parte de um risco específico: um agente pode usar as saídas de um modelo de fronteira sem autorização para treinar um modelo semelhante e implantá-lo sem proteções comparáveis, por exemplo para realizar ciberataques que o modelo original recusaria. Ele explica como a OpenAI detecta e interrompe esse processo, o que mudou no GPT-6.2 Astra e no GPT-6.2 Sol, e o que outros desenvolvedores podem aproveitar disso. Se a tese estiver correta, proteger apenas a resposta visível não basta, porque o raciocínio por trás dessa resposta pode ser o dado de treinamento mais valioso para uma cópia.

## Introdução
### Destilação adversarial
O artigo começa dizendo que, à medida que os modelos de IA de fronteira se tornam mais capazes, as salvaguardas importam mais para usá-los de forma responsável. A destilação adversarial é o caminho que contorna essas salvaguardas: um agente obtém as saídas de um modelo de fronteira sem autorização, usa-as para treinar outro modelo com capacidades semelhantes e implanta esse modelo sem proteções comparáveis. O exemplo do artigo é um adversário que destila as capacidades cibernéticas de um modelo e depois usa o resultado para ajudar em ciberataques que o modelo original recusaria, ou que suas salvaguardas bloqueariam.

A introdução também antecipa três pontos: como a OpenAI detecta e interrompe a destilação adversarial, como ela reforçou as proteções do raciocínio protegido dos seus modelos e o que aprendeu que pode ajudar a indústria como um todo a fortalecer suas defesas.

### Raciocínio protegido
O raciocínio protegido é a cadeia de raciocínio (chain-of-thought): as etapas intermediárias que um modelo gera internamente enquanto chega a uma resposta. O artigo dá ênfase especial a impedir que atacantes o extraiam, porque esses rastros podem conter informações que a resposta final omite, o que os torna dados de treinamento especialmente valiosos para outro modelo. A maior parte das defesas descritas no artigo tem como alvo esse único ativo.

## Nossa abordagem para a destilação adversarial
### Salvaguardas combinadas
A abordagem da OpenAI combina testes adversariais, treinamento de modelos, classificadores e proteções no nível da API com investigações sobre tentativas coordenadas de extrair capacidades dos modelos. O artigo afirma que essas salvaguardas atualizadas estão mais alinhadas às que a OpenAI usa para biologia e segurança cibernética.

### Salvaguardas atualizadas no GPT-6.2 Astra e no GPT-6.2 Sol
A OpenAI diz ter reforçado suas defesas contra a extração de raciocínio protegido no GPT-6.2 Astra e no GPT-6.2 Sol. Em uma avaliação adaptativa interna, usando red-teaming automatizado intenso, os testadores verificaram se atacantes conseguiriam recuperar o raciocínio protegido que o modelo gerara ao responder a um prompt anterior. A taxa média de sucesso dos ataques caiu de 54.0% contra o GPT-6 Astra com as salvaguardas originais para 0.2% contra o GPT-6.2 Astra com as salvaguardas atualizadas.

> 🔎 Contexto adicionado: Red-teaming significa testar um sistema tentando ativamente quebrá-lo. O red-teaming automatizado usa sistemas de IA para gerar esses ataques em escala.

## Por que a destilação é um problema de segurança
### Destilação
Destilação é uma técnica comum de aprendizado de máquina em que um modelo aprende com dados produzidos por outro. Um modelo pode aprender com as respostas de outro, com o raciocínio dele ou com os seus julgamentos sobre quais respostas são melhores e por quê. O artigo afirma que, além das preocupações com propriedade intelectual, a destilação não autorizada representa sérios riscos à segurança, porque um segundo modelo pode aprender capacidades avançadas de fronteira sem herdar as salvaguardas que regulam como o modelo original pode usá-las.

### Domínios de uso dual
A preocupação é especialmente clara em domínios de uso dual, como segurança cibernética e biologia. Um modelo que ajuda defensores a encontrar vulnerabilidades de software também poderia ajudar um atacante a explorá-las, e um modelo que auxilia pesquisas biológicas poderia ajudar alguém a desenvolver um patógeno nocivo. Se tais capacidades fossem transferidas para um modelo com salvaguardas mais fracas, esse modelo poderia fornecer ajuda que o original recusaria, ou que as salvaguardas originais bloqueariam.

## A importância de proteger os rastros exatos de raciocínio para a segurança cibernética
### Modelos professor e aluno
Os modelos da OpenAI ajudam usuários em tarefas de programação e de segurança cibernética defensiva. Um segundo modelo treinado com muitas dessas saídas benignas pode aprender habilidades que também se aplicam a tarefas de segurança cibernética ofensiva. O modelo que produz os dados de treinamento é chamado de professor, e o que aprende com ele, de aluno. O estudo interno do artigo pergunta como a destilação, especialmente a que envolve rastros de raciocínio, pode transferir capacidades cibernéticas arriscadas.

### Rastros exatos de raciocínio versus resumos de raciocínio
O estudo usou como base de destilação o conteúdo normalmente visível aos usuários (respostas finais, resumos de raciocínio e interações com ferramentas). Em seguida, testou quanta capacidade cibernética adicional é transferida quando o aluno tem acesso aos rastros exatos de raciocínio do professor. Cada execução de treinamento partiu do mesmo modelo aluno pré-treinado. O conjunto de dados de ajuste fino supervisionado consistiu principalmente em trajetórias de modelos professores de nível Astra trabalhando em tarefas de programação e de segurança cibernética defensiva, e cada trajetória incluía cada etapa do modelo, inclusive interações com ferramentas e a resposta final. Cada aluno foi avaliado após o ajuste fino e novamente após uma rodada leve de aprendizado por reforço em ambientes de segurança cibernética e programação.

> 🔎 Contexto adicionado: O ajuste fino supervisionado treina um modelo com mais exemplos de entradas e saídas desejadas. O aprendizado por reforço treina um modelo recompensando os resultados que ele deve produzir, e "leve" aqui significa uma rodada pequena desse processo.

Após o ajuste fino supervisionado, o aluno treinado com rastros exatos de raciocínio geralmente superou o aluno treinado com resumos de raciocínio. Após o aprendizado por reforço, o aluno com rastros exatos ainda pontuou mais alto no maior orçamento de inferência testado, nos dois benchmarks mostrados no artigo, enquanto o aluno treinado com resumos pontuou mais alto em orçamentos menores. O texto extraído não nomeia os benchmarks nem define as unidades do orçamento de inferência.

O artigo conclui que proteger os rastros de raciocínio pode limitar a transferência de capacidades cibernéticas, mesmo quando a tarefa subjacente é permitida pelas políticas de uso. Em muitas interações desse tipo, rastros exatos de raciocínio podem ensinar a um aluno habilidades que se generalizam para além das tarefas que o professor executou originalmente. A OpenAI afirma que continuará estudando quanta capacidade adicional é transferida entre diferentes modelos alunos e receitas de treinamento por destilação.

A nota de rodapé 1 registra que a Anthropic relatou uma vantagem semelhante ao treinar com transcrições completas de raciocínio em vez de resumos, no SWE-bench Verified, um benchmark de engenharia de software. A OpenAI afirma que o seu resultado mostra que essa vantagem se estende à segurança cibernética de fronteira.

## Defesas que se reforçam mutuamente
### Defesa no nível da requisição e no nível da campanha
O artigo descreve dois níveis complementares em sua pilha de salvaguardas. No nível da requisição, o comportamento do modelo, as verificações de classificadores e os controles de API limitam a extração de rastros de raciocínio. No nível da campanha, a OpenAI procura padrões em sinais de requisições e de contas que possam revelar tentativas coordenadas de extração. Ataques descobertos nas investigações tornam-se exemplos para treinar e testar as salvaguardas, de modo que os dois níveis se alimentam mutuamente. Em resumo, as defesas combinam salvaguardas para requisições individuais com detecção e aplicação de medidas no nível de campanha, e os achados das investigações e do red-teaming automatizado fortalecem ambos.

## Proteção de requisições e respostas individuais
### Treinamento defensivo para o comportamento do modelo
O treinamento adversarial ensina os modelos a reconhecer tentativas de extração de raciocínio e a se recusar a revelar o raciocínio protegido, continuando a responder a pedidos legítimos de explicação e de ajuda. Seus exemplos de treinamento vêm de ataques bem-sucedidos identificados por meio de red-teaming automatizado interno, investigações e divulgações responsáveis de possíveis falhas de segurança feitas por pesquisadores externos ou parceiros do setor.

### Salvaguardas de classificadores
A OpenAI usa classificadores de entrada para detectar tentativas de extrair raciocínio protegido, e classificadores de saída para detectar vazamentos de raciocínio protegido em mensagens do assistente e nos argumentos de chamadas de ferramentas. Essas salvaguardas se aplicam tanto a respostas em streaming quanto a respostas não streaming. A cobertura das chamadas de ferramentas importa porque um modelo pode expor raciocínio protegido nos argumentos de uma ferramenta sem incluí-lo na resposta final. Classificadores de saída podem bloquear vazamentos suspeitos antes que esse conteúdo chegue ao chamador.

> ❌ **Antipadrão — verificar apenas a resposta final:** Analisar somente a mensagem final do assistente em busca de raciocínio vazado e ignorar os argumentos das chamadas de ferramentas. **Por que as pessoas fazem isso:** O artigo não diz; a mensagem final é a saída visível e o lugar mais óbvio para verificar. **Consequência:** O raciocínio pode vazar pelos argumentos de uma ferramenta e chegar ao chamador sem ser notado. **Correção:** Verificar também os argumentos das chamadas de ferramentas e bloquear vazamentos suspeitos com classificadores de saída antes que o conteúdo chegue ao chamador.

### Proteções no nível da API
As proteções no nível da API restringem como o raciocínio protegido pode ser reutilizado entre interações e modelos. O comunicado de setembro da OpenAI descreveu ataques que moviam raciocínio criptografado de uma conversa para outra e pediam ao modelo que o revelasse em forma legível pelo usuário. A OpenAI reforçou as proteções nas fronteiras entre usuário, workspace, organização e família de modelos, incluindo o fechamento de um caminho que poderia expor o raciocínio criptografado de outro usuário.

Com o GPT-6.2 Astra e o GPT-6.2 Sol, a OpenAI vincula o raciocínio protegido de um modelo ao histórico de conversa que o produziu. À medida que novas mensagens são adicionadas, o modelo pode se apoiar no raciocínio anterior. Se uma mensagem anterior for reescrita, a API descarta o raciocínio que veio depois dela. Famílias de modelos mais antigas, com salvaguardas mais fracas, também são impedidas de ler o raciocínio protegido do GPT-6.2 Astra e do Sol.

> 🔎 Contexto adicionado: Raciocínio criptografado é raciocínio armazenado em uma forma que os usuários não conseguem ler diretamente. O artigo não descreve o esquema de criptografia.

## Investigação e interrupção de campanhas coordenadas
Quando os sistemas da OpenAI detectam campanhas não autorizadas de destilação adversarial, que frequentemente incluem tentativas coordenadas de extrair raciocínio protegido, ela investiga as contas responsáveis. Quando identifica violações, aplica medidas contra a rede de contas, de acordo com suas políticas e termos de serviço. Cada investigação de campanha gera informações que permitem à OpenAI aprimorar suas defesas para detectar atividades semelhantes.

### Compartilhamento de indicadores de comprometimento
A OpenAI compartilha indicadores de comprometimento com outros provedores de modelos para que possam investigar abusos relacionados em seus próprios serviços. O artigo afirma que esse trabalho vai além da plataforma da própria OpenAI.

> 🔎 Contexto adicionado: Um indicador de comprometimento é um sinal rastreável de atividade maliciosa suspeita que outra parte pode buscar. O artigo não diz quais indicadores específicos a OpenAI compartilha.

### Política de padrões consistentes
A política de padrões consistentes da OpenAI estabelece padrões compartilhados para salvaguardas e aplicação coordenada de medidas entre os provedores que oferecem seus modelos. A OpenAI trabalha de perto com parceiros, quando apropriado, para sustentar esses padrões e fortalecer as proteções nas implantações.

## Medindo o progresso
### Resistência à extração de raciocínio
Para medir o funcionamento conjunto das salvaguardas no nível da requisição, a OpenAI testa se atacantes ainda conseguem extrair raciocínio protegido em uma configuração que espelha de perto as implantações de produção. Em avaliações estáticas, os testadores repetem um conjunto fixo de ataques previamente bem-sucedidos para ver quais ainda expõem o raciocínio protegido. Em avaliações adaptativas, red-teamers automatizados de IA ajustam sua abordagem conforme a resposta do sistema, o que pode revelar fraquezas que os testes estáticos não detectam.

A visibilidade dos testadores também varia. O teste de caixa-preta lhes dá a mesma visão que um usuário externo teria, enquanto o teste de caixa-branca lhes fornece informações adicionais sobre o sistema para ajudar a construir ataques mais fortes. Na avaliação adaptativa de caixa-preta, os testadores tentaram recuperar o raciocínio protegido gerado ao responder tanto a uma requisição anterior quanto à atual. No GPT-6.2 Astra com as salvaguardas atualizadas, a taxa média de sucesso dos ataques ficou abaixo de 1% para cada tipo de ataque, substancialmente menor do que no GPT-6 Astra com as salvaguardas originais. A OpenAI está agora aplicando melhorias selecionadas de salvaguardas também ao GPT-6 Astra.

| Tipo de ataque | GPT-6 Astra, salvaguardas originais: ASR de extração adaptativa (%) | GPT-6.2 Astra, salvaguardas atualizadas: ASR de extração adaptativa (%) |
|---|---|---|
| Extração de raciocínio protegido de uma requisição anterior | 54.0 | 0.2 |
| Extração de raciocínio protegido da requisição atual | 22.9 | 0.9 |

A tabela informa a taxa de sucesso de ataque (ASR) em red-teaming adaptativo de caixa-preta contra o GPT-6 Astra e o GPT-6.2 Astra, agregada entre seis modelos atacantes. Quanto menor o ASR, melhor.

> ❌ **Antipadrão — confiar apenas no replay estático:** Usar um conjunto fixo de ataques previamente bem-sucedidos como único teste de uma defesa. **Por que as pessoas fazem isso:** O artigo não diz; um conjunto fixo é repetível e fácil de executar novamente. **Consequência:** Testes estáticos podem deixar passar fraquezas que um red-teamer adaptativo encontraria. **Correção:** Adicionar avaliações adaptativas, nas quais red-teamers automatizados ajustam sua abordagem às respostas do sistema.

### Detecção e aplicação de medidas contra campanhas
A OpenAI identificou várias campanhas não autorizadas de destilação adversarial em sua plataforma neste ano e compartilhou achados iniciais em seu relatório de setembro. Suas ações semanais de aplicação de medidas contra essas campanhas variaram de milhares a dezenas de milhares de contas, e o ritmo aumentou significativamente no último verão. A OpenAI acompanha a prevalência do abuso de destilação e o tempo entre a detecção e a aplicação de medidas. Seus objetivos são limitar o volume e o valor dos dados que os destiladores podem obter, aumentar o custo das campanhas de destilação e reduzir o tempo necessário para responder a novos ataques. O artigo diz que ela continua aprimorando essas defesas por meio de testes adversariais e investigações.

## As implicações de segurança nacional da destilação adversarial
A destilação adversarial também representa um risco à segurança nacional. Um ator estrangeiro que reproduz capacidades avançadas de um modelo dos EUA pode implantá-las sem as proteções que o desenvolvedor colocou. Em fevereiro, a OpenAI informou o Congresso sobre atividades que indicavam que a DeepSeek tentava destilar modelos de fronteira dos EUA. Em setembro, relatou ter interrompido uma campanha para extrair o raciocínio protegido de seus modelos, e atribuiu uma parte central dessa atividade a indivíduos associados à Moonshot AI, desenvolvedora do Kimi. A OpenAI diz que planeja compartilhar em breve mais informações sobre campanhas recentes que interrompeu.

### Legislação e controles de exportação
A OpenAI apoia legislação que daria ao governo dos EUA mais instrumentos para identificar e sancionar atores estrangeiros que extraem ilicitamente capacidades de modelos de IA americanos. Ela também informou ao Congresso que apoia a Chip Security Act, a AI OVERWATCH Act e a MATCH Act. O argumento do artigo é que controles de exportação mais rígidos limitam o acesso dos adversários ao hardware necessário para treinar modelos de fronteira, enquanto as defesas contra destilação dificultam a aquisição de algumas dessas mesmas capacidades a partir de modelos que já foram treinados. Juntas, as duas frentes ajudam a manter a liderança dos EUA em IA de fronteira e dão aos desenvolvedores mais margem para desacelerar um lançamento quando precisam de tempo para testar salvaguardas.

## Como outros podem se apoiar neste trabalho
Apoiar a proliferação segura de capacidades de fronteira é um objetivo importante do trabalho antidestilação da OpenAI. Sua experiência até agora sugere vários passos úteis para outros desenvolvedores de modelos e plataformas:

> 📋 **Checklist — Passos para outros desenvolvedores de modelos e plataformas**
> - [ ] Proteja o raciocínio privado em toda a interação. Verifique as mensagens do assistente e os argumentos das chamadas de ferramentas em busca de vazamentos, e restrinja como o raciocínio criptografado pode ser reutilizado entre conversas e modelos.
> - [ ] Mantenha como testes de regressão os registros de ataques de extração bem-sucedidos encontrados por meio de red-teaming, monitoramento de produção ou divulgação responsável. Quando apropriado, e de acordo com as políticas de privacidade aplicáveis, use esses exemplos para melhorar o treinamento de modelos e os classificadores. Ao avaliar defesas atualizadas, repita os ataques originais e teste se um red-teamer adaptativo ainda consegue encontrar uma brecha nessas defesas.
> - [ ] Combine mitigações no nível da requisição com investigação e aplicação de medidas no nível da campanha.
> - [ ] Coordene requisitos de salvaguardas e aplicação de medidas com parceiros de implantação, e compartilhe conhecimento com outros desenvolvedores de modelos de IA, para garantir um piso elevado de salvaguardas contra destilação em todo o setor.

A OpenAI também compartilhou achados por meio do Frontier Model Forum e de canais governamentais apropriados de compartilhamento de informações, para ajudar outros desenvolvedores a fortalecer suas defesas, mantendo privados os detalhes que atacantes poderiam explorar. Ela afirma que continuará trabalhando com pesquisadores, desenvolvedores e parceiros para compartilhar o que aprende sobre ataques emergentes e defesas eficazes. O artigo termina dizendo que, à medida que a IA de fronteira se torna amplamente disponível, essas defesas visam reduzir o risco de que suas capacidades sejam reproduzidas sem salvaguardas adequadas, em apoio à missão da OpenAI de garantir que a inteligência artificial geral seja implantada com segurança e beneficie toda a humanidade.

## 🧠 Modelos mentais
### Dois níveis complementares
**O modelo:** Salvaguardas no nível da requisição verificam cada interação, enquanto a detecção no nível da campanha procura padrões entre requisições e contas que revelem extração coordenada. Cada nível fortalece o outro.
**Como aplicar:** Combine verificações por requisição com detecção por conta e por padrão, e realimente o treinamento e os testes com o que as investigações descobrirem.

### Ataques viram testes de regressão
**O modelo:** Todo ataque de extração bem-sucedido é um caso de teste a ser mantido, não um incidente pontual.
**Como aplicar:** Registre ataques bem-sucedidos vindos de red-teaming, monitoramento de produção ou divulgação responsável, e repita-os sempre que uma defesa mudar.

### Verifique todo canal de saída
**O modelo:** Vazamentos podem aparecer em qualquer lugar onde o modelo escreve, inclusive nos argumentos de chamadas de ferramentas, não apenas na mensagem final.
**Como aplicar:** Analise todo canal de saída, em streaming e não streaming, antes que o conteúdo chegue ao chamador.

## 🔑 Principais conclusões
- A destilação adversarial pode transferir as capacidades de um modelo de fronteira para um modelo sem as salvaguardas, e o principal exemplo do artigo é a capacidade cibernética.
- No estudo interno, um aluno treinado com rastros exatos de raciocínio geralmente superou um treinado com resumos após o ajuste fino supervisionado, e manteve a vantagem no maior orçamento de inferência após o aprendizado por reforço.
- A proteção precisa cobrir toda a interação: classificadores de saída analisam os argumentos das chamadas de ferramentas, e os controles de API vinculam o raciocínio ao histórico de conversa e bloqueiam o reúso entre usuários, workspaces, organizações e famílias de modelos.
- No red-teaming adaptativo de caixa-preta, a taxa de sucesso de ataque do GPT-6.2 Astra caiu de 54.0% para 0.2% na extração de raciocínio de uma requisição anterior, e de 22.9% para 0.9% na requisição atual.
- A defesa atua em dois níveis, salvaguardas por requisição e detecção e aplicação de medidas por campanha, e as descobertas das investigações alimentam tanto o treinamento quanto os testes.
- Replays estáticos podem não detectar fraquezas que red-teamers adaptativos encontram, então as avaliações devem variar a abordagem e a visibilidade do atacante.
- Indicadores de comprometimento compartilhados e padrões consistentes de salvaguardas permitem que outros provedores investiguem abusos relacionados em suas próprias plataformas.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um arquiteto que trabalha com governança de IA, o artigo sugere que os controles mais importantes ficam nas fronteiras do uso de modelos: inspecionar requisições, analisar saídas em todos os canais e definir regras de reúso entre usuários, workspaces e famílias de modelos. Essas fronteiras correspondem aos pontos em que você colocaria um gateway de IA ou um ponto de aplicação de políticas em um panorama corporativo. A pergunta que ele levanta para o seu trabalho de arquitetura: se a sua organização faz ajuste fino ou destilação a partir de modelos de fornecedores, quais regras de proveniência e reúso governam os rastros de raciocínio e as saídas que você mantém, e quem as verifica?

## 📚 Plano de leitura
### Do artigo
- [Combatendo a destilação adversarial](https://openai.com/index/combatting-adversarial-distillation) — o artigo-fonte que esta nota expande, incluindo suas figuras e a ordem completa das seções.
- O texto extraído não contém URLs no corpo para as outras obras que cita, por isso elas aparecem em "Para saber mais".

### Para saber mais
- Relatório de setembro sobre a campanha para extrair raciocínio protegido — os achados anteriores em que o artigo se baseia, citados pela OpenAI, mas não vinculados no texto extraído.
- Comunicado de setembro sobre raciocínio criptografado transferido entre conversas — o padrão de ataque que motivou as proteções de API entre fronteiras.
- Frontier Model Forum — onde a OpenAI compartilhou achados com outros desenvolvedores para fortalecer suas defesas.
- Política de padrões consistentes — os padrões compartilhados de salvaguardas e aplicação de medidas que a OpenAI aplica entre os provedores de seus modelos.
- Chip Security Act, AI OVERWATCH Act e MATCH Act — as medidas de hardware e de controle de exportação que a OpenAI informou ao Congresso que apoia.
- Resultado da Anthropic no SWE-bench Verified sobre treinamento com transcrições completas de raciocínio versus resumos (nota de rodapé 1) — um achado semelhante em engenharia de software.

## Conceitos
- [distillation](/pt-br/concepts/ai-ml/distillation.md)
