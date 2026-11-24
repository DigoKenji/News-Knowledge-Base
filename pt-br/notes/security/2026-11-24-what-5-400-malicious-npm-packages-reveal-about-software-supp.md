---
type: Article Summary
title: O que 5,400 pacotes npm maliciosos revelam sobre o risco na cadeia de suprimento de software em 2026
description: Quando contada por campanha, a publicação de pacotes npm maliciosos é estável; comprometimentos raros de pacotes legítimos concentram quase todos os downloads.
resource: https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026
tags:
- security
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026
  title: What 5,400 Malicious npm Packages Reveal about Software Supply Chain Risk in 2026
  author: Databricks Threat Research
status: stable
source_url: https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026
source: Databricks
source_type: blog
published: '2026-11-23'
domain: security
concepts:
- oidc
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81f5a2b6c31fddec9a6a
lang: pt-br
translation_of: /notes/security/2026-11-24-what-5-400-malicious-npm-packages-reveal-about-software-supp.md
---

## 🎯 Tese central
Quando as contagens de pacotes npm maliciosos são normalizadas por campanha, a atividade de publicação é estável, enquanto os raros comprometimentos de pacotes legítimos concentram quase toda a exposição de downloads, e as remoções feitas pelo registro ficam atrás dos avisos públicos.

## 🧭 Por que o autor escreveu
O artigo é o primeiro de uma série sobre ameaças à cadeia de suprimento. Ele parte de cerca de 5,400 pacotes npm maliciosos confirmados, publicados entre dezembro de 2025 e outubro de 2026, o primeiro ano depois que o npm revogou os tokens de autenticação de longa duração. Os totais mensais brutos fazem a ameaça parecer volátil, mas o autor argumenta que a volatilidade vem de poucas campanhas; se isso estiver certo, os defensores devem acompanhar campanhas, comprometimentos e o atraso de remoção, e não as contagens de pacotes.

## Escopo e método
O dataset vem de um scanner que analisa eventos de publicação no npm. Só são mantidos pacotes com pelo menos uma fonte independente: um aviso público, um relatório de campanha de fornecedor, uma remoção de segurança do npm ou uma análise de um analista. O tempo é medido pela data de publicação no npm. A unidade de registro é a versão do pacote, e cada pacote recebe a data de sua primeira versão maliciosa. Os downloads são capturas mensais feitas no momento da varredura, e o status no registro foi verificado em 6 de novembro de 2026.

> 🔎 Contexto adicional: as figuras são referenciadas por legenda, mas não estão incluídas no texto extraído, então esta nota se baseia na prosa. O artigo não tem blocos de código nem checklists, então nenhum deles é reproduzido.

### Pacote malicioso verificado
Um pacote entra no dataset somente depois que uma fonte independente o confirma, o que filtra alertas brutos de scanner. O filtro privilegia a precisão sobre a cobertura, então um pacote suspeito sem nenhuma corroboração externa fica de fora. Por isso, as contagens devem ser lidas como casos confirmados, e não como toda publicação suspeita.

### Unidade de registro
As contagens são por versão de pacote, mas cada pacote assume a data de sua primeira versão maliciosa. Um pacote com várias versões ruins, portanto, soma várias vezes nos números de volume, mantendo uma única data. Ter isso em mente evita que um único pacote de longa duração seja confundido com uma nova onda.

## Atividade ao longo do tempo, contada por campanha
As contagens mensais de pacotes maliciosos confirmados variam em um fator de quinze ao longo do período. O autor atribui isso a um pequeno número de campanhas: em agosto de 2026, uma única campanha de confusão de dependências (dependency confusion) responde por 78% das versões confirmadas. Contando cada campanha como um único ator, a atividade é estável: várias centenas de novas contas de publicação por mês, uma taxa constante de pacotes maliciosos em relação ao total de pacotes publicados e picos que correspondem a campanhas nomeadas. Depois dessa normalização, restam três mudanças. O comprometimento de pacotes legítimos é episódico. O reconhecimento de confusão de dependências é contínuo, em um nível constante. A ofuscação aumentou: a parcela de atores cujos pacotes acionam uma regra de ofuscação mais que dobrou entre maio e agosto de 2026.

### Normalização por campanha
Normalizar significa agrupar pacotes pelo ator ou campanha por trás deles antes de interpretar uma tendência. Sem isso, uma explosão de centenas de versões parece uma ameaça crescente. Com isso, a taxa de publicação subjacente pode ser vista separadamente das explosões. O artigo usa esse passo para mostrar que uma atividade constante existe por baixo dos picos de manchete.

### Confusão de dependências
A confusão de dependências depende de atacantes publicando pacotes com nomes que uma organização usa internamente, na esperança de que os builds busquem esses pacotes no registro público. O artigo relata reconhecimento desse ataque em todos os meses, e ele impulsiona o pico de agosto. Sua entrega é descrita como builds mal configurados, e não como usuários escolhendo o pacote.

> 🔎 Contexto adicional: ferramentas de build podem preferir uma versão do registro público a um nome privado, e essa é a brecha que o ataque explora. O artigo não explica o mecanismo em si.

> ❌ **Anti-padrão — Nomes internos não protegidos:** uma configuração de build que permite que um nome de pacote interno sem escopo seja resolvido no registro público do npm. **Por que as pessoas fazem isso:** o artigo não diz; a resolução padrão funciona sem configuração. **Consequência:** um pacote com o mesmo nome, registrado por um atacante, é instalado automaticamente, o que o artigo diz ser o caminho pelo qual a maioria dos pacotes criados por atacantes chega às vítimas. **Correção:** veja o contexto adicional abaixo.

> 🔎 Contexto adicional: o artigo não dá nenhuma correção. Uma mitigação comum é fixar nomes internos em um registro privado ou em um escopo que a organização controla.

## Pacotes legítimos comprometidos
Pacotes legítimos comprometidos são o outro lado do quadro. Três ondas de uma mesma família de worms ocorreram em maio, junho e agosto de 2026. As ondas de maio e agosto comprometeram cada uma mais de cem pacotes, e a onda de junho republicou o escopo inteiro de um fornecedor em um único dia. Os detalhes da família aparecem em uma seção própria, mais abaixo.

### Pacote legítimo comprometido
Um pacote comprometido é um pacote real e popular cuja conta de publicação foi tomada, então a versão maliciosa é publicada sob um nome confiável. Usuários que já dependem dele instalam a versão maliciosa sem nenhuma escolha de sua parte. O ponto do artigo é que esses eventos são raros, mas alcançam muito mais instalações do que pacotes maliciosos recém-criados.

## Alvos de credenciais
O alvo de credenciais de desenvolvedores de IA ficou entre 1% e 4% dos pacotes durante todo o ano, embora as contagens absolutas tenham subido junto com o volume geral. O roubo de tokens de autenticação do npm atingiu o pico de 22% dos pacotes em março de 2026 e caiu abaixo de 5% a partir de maio. Os tokens clássicos de longa duração foram revogados em 9 de dezembro de 2025. O autor raciocina que os tokens visados depois disso eram plausivelmente tokens de acesso granulares e segredos de CI.

### Token de autenticação do npm
Um token do npm permite que uma ferramenta publique pacotes sem login interativo, então um token roubado pode publicar em nome do mantenedor. Os dados do artigo mostram o roubo de tokens atingindo o pico em março de 2026 e permanecendo abaixo de 5% depois de maio. Essa queda segue a revogação de dezembro de 2025, embora o artigo não afirme que os dois fatos estejam ligados por causalidade.

> 🔎 Contexto adicional: um token de acesso granular é um token mais restrito, que pode ser limitado a pacotes e permissões específicos, diferentemente dos tokens clássicos de longa duração que foram revogados.

## Contas de publicação
99% dos pacotes maliciosos são publicados por contas de mantenedor único, e 55% dessas contas usam um endereço de e-mail gratuito (free-mail). Dois terços das contas publicam exatamente um pacote malicioso. Entre as contas que publicam mais de um, a mediana do intervalo entre a primeira e a última publicação maliciosa é inferior a um dia. As campanhas ligadas à Coreia do Norte (DPRK-nexus) são a exceção: várias centenas de contas, cada uma com um ou dois pacotes, 83% em endereços de e-mail gratuitos. Outras campanhas nomeadas usam e-mail gratuito em 15% dos casos, o que o autor interpreta como compatível com pools de contas adquiridas.

### Conta de mantenedor único
Uma conta de mantenedor único é a única identidade com direitos de publicação de um pacote, então quem controla a conta controla cada lançamento. O artigo constata que 99% dos pacotes maliciosos vêm dessas contas. A maioria delas é usada uma única vez, já que dois terços publicam um único pacote malicioso, o que aponta para identidades descartáveis, e não para mantenedores de longo prazo.

### Pool de contas adquiridas
Um pool de contas adquiridas é um conjunto de contas obtidas em massa e distribuídas entre campanhas. O artigo descreve a taxa de 15% de e-mail gratuito das outras campanhas nomeadas como compatível com isso. O padrão DPRK-nexus, de centenas de contas, cada uma usada para um ou dois pacotes, se encaixa na mesma imagem.

> 🔎 Contexto adicional: DPRK-nexus se refere a campanhas ligadas a atores de Estado da Coreia do Norte. O artigo usa o rótulo sem elaborar.

## Metadados dos pacotes
88% dos pacotes tinham menos de trinta dias de idade no momento da publicação maliciosa, e 75% não declaravam repositório. 66% tinham uma única versão. 65% mantinham os valores de template do npm init nos campos de autor, descrição ou repositório, e 54% declaravam um script de ciclo de vida. 17% levavam um nome com aparência interna, como core, utils, sdk ou config. O typosquatting é uma tática minoritária: 13% incorporam um nome popular, e menos de 5% estão a até duas edições de caractere de um nome popular.

### Script de ciclo de vida
Um script de ciclo de vida é um comando no package.json que o npm executa automaticamente em certos momentos, como durante a instalação. Como roda sem que ninguém o invoque, é um ponto inicial para a execução de código malicioso. O artigo constata que 54% dos pacotes maliciosos declaram um, e isso se torna um sinal-chave de comprometimento: 78% das versões comprometidas adicionam ou modificam um.

> 🔎 Contexto adicional: o arquivo package.json é o manifesto que o npm lê para descrever e instalar um pacote.

### Typosquatting
O typosquatting publica um pacote cujo nome imita um pacote popular, apostando em um erro de digitação para capturar instalações. O artigo constata que ele é uma tática minoritária aqui, com 13% dos pacotes incorporando um nome popular e menos de 5% a até duas edições de caractere. Por isso, a maior parte do conjunto não depende de errar a grafia de um nome conhecido.

## Indicadores de comprometimento
Pacotes legítimos comprometidos se diferenciam dos criados por atacantes pelos sinais de mudança no nível da versão. 78% das versões comprometidas adicionam ou modificam um script de ciclo de vida, contra 4% dos pacotes criados por atacantes. 23% apresentam mudança de mantenedor ou de publicador. A maioria dos comprometimentos passa pela conta legítima e deixa os registros de propriedade inalterados.

### Registro de propriedade
Um registro de propriedade mostra quem mantém um pacote e quem publicou cada versão. Uma mudança nele é um alarme natural para os defensores. A constatação do artigo é que a maioria dos comprometimentos o deixa inalterado, porque o atacante publica pela conta real do mantenedor, então um histórico de propriedade limpo não mostra que uma versão é segura.

## Comportamento
72% dos pacotes carregam um marcador de ofuscação, e 30% buscam um segundo estágio. O roubo de credenciais e dados é o comportamento predominante em todos os meses, exceto agosto. Em agosto, alguns pacotes roubaram credenciais, mas tentaram entregar um implante de comando e controle Sliver à máquina da vítima, então sua classificação mudou para Trojan / Downloader.

### Stealer de credenciais
Um stealer de credenciais coleta segredos da máquina onde o pacote é instalado. O artigo relata que 45% dos stealers visam uma única classe de credencial, e 36% visam três ou mais. Os stealers de classe única se distribuem igualmente entre credenciais de nuvem, chaves SSH, credenciais de desenvolvedores de IA, tokens do npm e arquivos .env. Nos stealers de múltiplas classes, credenciais de nuvem, chaves SSH e arquivos .env aparecem juntos, enquanto o roubo de carteiras e de dados de navegador ocorre separadamente. As credenciais de desenvolvedores de IA são o alvo mais especializado: 80% dos pacotes que as visam não visam nenhuma outra classe.

### Payload de segundo estágio
Um payload de segundo estágio é código que um primeiro pacote busca depois de executar, de modo que o pacote instalado pode permanecer pequeno. O artigo relata que 30% dos pacotes buscam um. Para uma revisão, isso significa que inspecionar apenas o pacote instalado pode deixar passar o que ele busca depois.

### Implante Sliver
Um implante de comando e controle dá a um atacante um canal persistente com uma máquina comprometida. Em agosto, alguns stealers foram além e tentaram entregar um. A mudança de classificação para Trojan / Downloader reflete esse passo além do roubo.

> 🔎 Contexto adicional: Sliver é um framework open-source de comando e controle. O artigo o menciona apenas como o implante que os pacotes de agosto tentaram entregar.

## Plataforma
55% dos pacotes não contêm código específico de sistema operacional. Entre os demais, os droppers visam Windows por meio de PowerShell, tarefas agendadas e executáveis. Stealers de credenciais e backdoors visam Linux por meio de systemd, cron e arquivos de configuração do shell. Código específico de macOS aparece em cerca de um em cada oito stealers.

### Dropper
Um dropper é um pacote cuja função é entregar e iniciar outro payload, em vez de roubar algo por conta própria. A divisão apresentada no artigo mostra que os droppers tendem a mecanismos do Windows, enquanto stealers e backdoors tendem à persistência no Linux. Código específico de plataforma é, portanto, uma pista do papel que um pacote desempenha em um ataque.

## Iscas
24% dos pacotes têm escopo, espalhados por mais de trezentos escopos distintos. A maioria desses escopos contém um único pacote malicioso, o que o artigo descreve como um namespace interno de organização registrado uma vez. 36% declaram um autor e 24% um repositório, e cerca de 80% de cada grupo se referem a uma identidade diferente da conta de publicação. 62% incluem um README, e 16% desses contêm um comando de instalação de outro pacote, em metade dos casos um popular. A maioria dos pacotes criados por atacantes não usa isca, o que é coerente com a constatação de que eles são obtidos por builds mal configurados, e não escolhidos pelos usuários.

### Pacote com escopo
Um pacote com escopo tem um nome da forma @scope/name, em que o escopo é um namespace pertencente a um usuário ou organização. Os escopos normalmente mantêm os nomes internos de uma organização separados. Aqui, atacantes registraram escopos parecidos com o namespace interno de uma organização, o que pode capturar builds que resolvem esses nomes.

> 🔎 Contexto adicional: a forma @scope/name é a convenção de nomenclatura do npm para pacotes com escopo.

### Isca
Uma isca é qualquer coisa que faça um pacote parecer legítimo ou tentador, como um autor, um link de repositório ou um README. O artigo encontra iscas em apenas alguns pacotes, e a maioria dos pacotes criados por atacantes não tem nenhuma. Isso sugere que a entrega depende mais da configuração do build do que de convencer um desenvolvedor a escolher o pacote.

## A família de worms e a publicação confiável
Pacotes legítimos comprometidos são 7% do conjunto e mais de 99% da exposição de downloads. Todas as ondas de comprometimento do período pertencem a uma única família: primeiro o kit de um único ator de ameaça, depois disponibilizado gratuitamente como código aberto em maio. As ondas de maio e agosto comprometeram cada uma mais de cem pacotes. Em junho, o mesmo kit republicou o escopo inteiro de um único fornecedor em um dia, por meio de publicação confiável, seguido em poucos dias por alguns pacotes sem relação. Fora dessas ondas, o comprometimento ocorre em alguns pacotes por mês.

As versões comprometidas por worms podem ser distinguidas de pacotes criados por atacantes. Todas são executadas por meio de um script de ciclo de vida, um terço lê credenciais de nuvem ou o token de conta de serviço do Kubernetes, e 44% acionam regras de ofuscação. Recebem avisos públicos em poucas horas após a publicação. As vítimas de maior exposição da onda de agosto foram republicadas por meio de publicação confiável OIDC a partir de ambientes de CI comprometidos, e não por tokens roubados. Ao longo do período, 85% dos pacotes do conjunto malicioso publicados por meio de publicação confiável são comprometimentos. A revogação dos tokens de dezembro de 2025 levou os mantenedores à publicação confiável, e a atividade de comprometimento posterior passou a visar diretamente o pipeline de publicação.

### Publicação confiável
A publicação confiável permite que um workflow de CI publique no npm usando uma identidade OIDC, em vez de um token de longa duração armazenado. Os dados do artigo mostram que a migração para ela não eliminou a superfície de ataque; apenas deslocou o alvo para o ambiente de CI que publica. A revogação de dezembro de 2025 apontou os mantenedores para esse caminho, o que torna o pipeline a coisa a ser protegida.

> ❌ **Anti-padrão — Tratar a publicação confiável como solução:** migrar para a publicação confiável e presumir que o pipeline está seguro, sem proteger o ambiente de CI que tem direitos de publicação. **Por que as pessoas fazem isso:** a revogação empurrou os mantenedores para essa opção, então ela parece ser o estado final. **Consequência:** ambientes de CI comprometidos republicaram os pacotes de maior exposição na onda de agosto, e 85% dos pacotes de publicação confiável do conjunto são comprometimentos. **Correção:** veja o contexto adicional abaixo.

> 🔎 Contexto adicional: o artigo não dá nenhuma correção. Uma mitigação comum é reforçar o ambiente de CI e limitar quais workflows e branches podem publicar.

## Serviços legítimos compartilhados
38% dos pacotes, e 42% dos que têm um indicador de rede, se comunicam com um serviço legítimo de terceiros. A hospedagem serverless é a mais comum, e apenas os Cloudflare Workers atendem várias centenas de pacotes a partir de um número comparável de contas. Em seguida vêm sites de hospedagem de código e de colagem de texto, depois webhooks do Telegram, do Discord e do Slack. Uma pequena população busca o endereço do payload em um smart contract por meio de um endpoint RPC de blockchain pública; dois terços deles são fortemente ofuscados, e 42% distribuem um binário compilado. 8% dos droppers não têm um host resolvível, porque a URL do segundo estágio é montada em tempo de execução.

### Hospedagem serverless
A hospedagem serverless executa código na plataforma de um provedor de nuvem sem que o atacante gerencie um servidor. O artigo a apresenta como o serviço legítimo mais usado, e só os Cloudflare Workers atendem várias centenas de pacotes. Tráfego para um provedor bem conhecido é mais difícil de bloquear apenas por reputação do que tráfego para um host desconhecido.

> 🔎 Contexto adicional: hospedagem serverless, aqui, significa funções executadas por um provedor, de modo que o atacante não precisa de servidor próprio.

### Ponteiro de payload hospedado em blockchain
Alguns pacotes leem o endereço do payload em um smart contract por meio de um endpoint RPC de blockchain pública. O artigo não diz por que os atacantes escolhem isso, mas significa que o ponteiro fica em uma cadeia pública, e não em um servidor que o atacante controla. Dois terços desses pacotes são fortemente ofuscados, e 42% distribuem um binário compilado.

## Exposição
81% dos pacotes criados por atacantes não tinham downloads no momento da varredura. Os dez pacotes mais baixados do conjunto respondem por mais de 99% dos downloads registrados, e os dez são pacotes legítimos comprometidos. Os pacotes criados por atacantes são a maioria em número, mas é nos pacotes comprometidos que a exposição se concentra.

### Exposição de downloads
A exposição de downloads é o número de instalações que um pacote recebeu, ou poderia receber. Neste conjunto, ela é altamente concentrada: alguns pacotes populares comprometidos concentram mais de 99% dos downloads registrados. Ordenar por downloads, portanto, mostra onde está o risco, enquanto contar pacotes mostra sobretudo volume.

## Tempo até o aviso público
87% do conjunto tem um aviso OSV. Campanhas nomeadas de worms e de confusão de dependências recebem avisos no próprio dia da publicação. Pacotes não atribuídos recebem avisos após uma mediana de 1,7 dia, com 10% esperando mais de um mês. Pacotes DPRK-nexus levam uma mediana de 4,5 dias, com 10% esperando mais de dois meses, a população mais lenta entre todas as voltadas a desenvolvedores.

### Aviso OSV
Um aviso OSV é um registro público de um pacote malicioso ou vulnerável no formato OSV. É o sinal que informa a consumidores e ferramentas que um pacote é conhecido por ser ruim.

> 🔎 Contexto adicional: OSV é um formato aberto de banco de dados de vulnerabilidades que agrega avisos de vários ecossistemas.

### Atraso entre publicação e aviso
O atraso é o tempo entre uma publicação maliciosa e o primeiro aviso público. É a janela em que um pacote pode ser instalado antes que alguém alerte sobre ele. Campanhas nomeadas fecham essa janela em menos de um dia, enquanto pacotes não atribuídos podem ficar expostos por um mês ou mais.

## Resultado no registro
Cada pacote criado por atacantes foi verificado no registro no momento da análise. 40% tinham sido substituídos pelo placeholder de segurança do npm, com mediana de 7,1 dias após a publicação, e 10% depois de mais de 59 dias. 37% tinham sido despublicados pelo próprio publicador, com mediana de 0,3 dia. Atacantes retiram pacotes mais rápido do que o registro os remove, e a parcela de autorremoções cresceu ao longo do ano. 7% foram excluídos sem um carimbo de data recuperável, e 16% continuavam instaláveis.

A remoção varia conforme o ator. 64% dos pacotes DPRK-nexus foram removidos pelo npm, com atraso mediano de 19,5 dias, e 5% continuam instaláveis; campanhas DPRK-nexus individuais variam de 16,4 a 65,9 dias. Dos pacotes não atribuídos, 21% continuam instaláveis. A partir de março de 2026, a mediana dos avisos precedeu a mediana das remoções em todos os meses, então o registro é o controle mais lento.

### Placeholder de segurança do npm
O placeholder de segurança é o substituto do npm para um pacote removido, que o artigo conta entre os resultados do registro. 40% dos pacotes criados por atacantes terminaram assim, tipicamente após cerca de uma semana, com uma cauda além de dois meses.

> 🔎 Contexto adicional: o placeholder é uma substituição do lado do registro, distinta de o publicador retirar o pacote.

### Atraso de remoção
O atraso de remoção é o tempo entre a publicação e a remoção pelo registro. O artigo constata que ele costuma ser maior do que o atraso do aviso, então a divulgação pública tende a vir primeiro. Sua conclusão é que a remoção é um controle mais lento do que a divulgação pública, então os defensores não podem contar com o registro para eliminar malware antes que os alertas sejam emitidos.

## Resultados por campanha
Os resultados diferem muito por campanha. Campanhas com avisos no mesmo dia tiveram atrasos medianos de remoção de poucos dias ou menos. Campanhas DPRK-nexus receberam avisos em poucos dias, mas continuaram instaláveis por semanas ou meses; cada tag DPRK-nexus do dataset tem um atraso mediano de remoção superior a duas semanas. Um grupo adicional, que inclui cinco famílias de stealers de carteiras de criptomoedas, nunca foi removido pelo registro. Seus publicadores retiraram todos os pacotes por conta própria, tipicamente em um dia, e o registro não registrou nenhuma ação de segurança. O artigo termina dizendo que o próximo relatório tratará de ameaças emergentes que ampliam o papel do registro npm além da distribuição tradicional de malware.

## 🧠 Modelos mentais
### Conte campanhas, não pacotes
**O modelo:** a maior parte da variação mensal vem de algumas campanhas, então a explosão de um ator parece uma tendência, a menos que seja contada uma única vez.
**Como aplicar:** agrupe as detecções por campanha antes de interpretar uma tendência, e verifique se um pico vem de um ator ou de muitos.

### Comprometimento raro, exposição desproporcional
**O modelo:** comprometimentos de pacotes confiáveis são episódicos, mas alcançam quase todos os downloads, então o risco se concentra em poucos nomes populares.
**Como aplicar:** ordene os pacotes por downloads e acompanhe primeiro, nos mais usados, as mudanças no nível da versão, como novos scripts de ciclo de vida.

## 🔑 Principais conclusões
- As contagens mensais de pacotes maliciosos variam quinze vezes, e uma única campanha de confusão de dependências responde por 78% das versões confirmadas em agosto de 2026.
- Pacotes legítimos comprometidos são 7% do conjunto, mas mais de 99% da sua exposição de downloads.
- 78% das versões comprometidas adicionam ou modificam um script de ciclo de vida, contra 4% dos pacotes criados por atacantes.
- 99% dos pacotes maliciosos vêm de contas de mantenedor único, e 85% dos pacotes de publicação confiável do conjunto são comprometimentos.
- 81% dos pacotes criados por atacantes não tinham downloads quando foram verificados, e 37% foram despublicados pelos próprios publicadores, com mediana de 0,3 dia após a publicação.
- A partir de março de 2026, a mediana dos avisos precedeu a mediana das remoções em todos os meses, então a divulgação pública superou o registro.
- 16% dos pacotes criados por atacantes continuavam instaláveis no momento da análise, e 64% dos pacotes DPRK-nexus foram removidos pelo npm, com atraso mediano de 19,5 dias.

## 🏛️ Para o Kenji
No trabalho de um arquiteto, isso se aplica à governança de dependências: quais registros um build pode usar, se nomes internos de pacotes estão protegidos contra resolução pública, e como os direitos de publicação em CI são delimitados e monitorados. O artigo sugere que pacotes confiáveis comprometidos e pipelines de publicação carregam mais risco do que pacotes maliciosos recém-criados. Uma pergunta que ele levanta: quais dos nossos pipelines de build e publicação ainda publicariam com segurança se um atacante controlasse o ambiente de um único job? (Esta é a sua interpretação, não a do autor.)

## 📚 Plano de leitura
### Do artigo
- [Artigo original](https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026) — o dataset completo, suas figuras e a série que este primeiro texto inaugura.
- GitHub (2025), “npm classic tokens revoked, session-based auth and CLI token management now available” — a revogação de dezembro de 2025 que inicia o período do estudo. O artigo não dá link para ela.
### Para ir além
- Documentação de publicação confiável do npm (OIDC) — como a identidade do CI substitui os tokens de publicação armazenados.
- Formato de avisos OSV — como os avisos públicos são estruturados e agregados entre ecossistemas.
- Escopos de pacotes do npm e resolução de registros — como as ferramentas de build escolhem entre registros privados e públicos.

## 🔗 Conceitos
- [oidc](/pt-br/concepts/security/oidc.md)
