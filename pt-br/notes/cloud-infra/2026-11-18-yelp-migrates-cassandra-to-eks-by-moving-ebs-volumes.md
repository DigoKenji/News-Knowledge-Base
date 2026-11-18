---
type: Article Summary
title: A Yelp Migra o Cassandra para o EKS Movendo Volumes EBS
description: A Yelp migrou 90 clusters Cassandra para o EKS reanexando volumes EBS existentes a novos pods, evitando cópias de dados e mantendo o rollback trivial.
resource: https://www.infoq.com/news/2026/11/kubernetes-cassandra-eks/
tags:
- cloud-infra
- cloud-platforms
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/kubernetes-cassandra-eks/
  title: Yelp Migrates Cassandra to EKS by Moving EBS Volumes
  author: InfoQ
status: stable
source_url: https://www.infoq.com/news/2026/11/kubernetes-cassandra-eks/
source: InfoQ
source_type: blog
published: '2026-11-18'
domain: cloud-infra
subdomain: cloud-platforms
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8172897feaf63fd0dc32
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-18-yelp-migrates-cassandra-to-eks-by-moving-ebs-volumes.md
---

## 🎯 Tese central
A Yelp moveu sua frota Cassandra para o EKS reapontando volumes EBS existentes para novos pods, em vez de copiar dados entre nós. Assim, o tempo de indisponibilidade e a perda de dados ficaram em zero, e o rollback passou a ser a mesma operação executada na ordem inversa.

## 🧭 Por que o autor escreveu
A Yelp já havia migrado suas cargas sem estado (stateless) para o EKS, mas o Cassandra permaneceu em clusters legados autogerenciados, porque o Kubernetes não tem uma forma nativa de migrar StatefulSets e seus volumes persistentes entre clusters. Se a tese se sustentar, uma frota com estado pode mudar de cluster sem o custo de transferência de dados, sem a duplicação de capacidade e sem a transmissão de dados que leva semanas, exigidos pelas abordagens usuais.

## Escala e resultado
A migração abrangeu cerca de 90 clusters de produção, com zero indisponibilidade para os clientes e nenhuma perda de dados. O maior cluster, um armazenamento de dados de usuários com 54 pods, levou cerca de 12 horas de ponta a ponta.

## Por que as abordagens usuais foram descartadas
A abordagem padrão é adicionar nós ao novo cluster e desligar os antigos. Isso duplica temporariamente a capacidade computacional e gera custos de transferência de dados. Para os maiores clusters, somente a transmissão de dados poderia levar semanas. A Yelp também considerou um failover regional com reconstrução e um novo datacenter Cassandra no EKS, mas classificou ambas as opções como menos transparentes para os clientes.

### StatefulSet
Um StatefulSet é o tipo de carga de trabalho do Kubernetes para software com estado, como bancos de dados. Cada pod mantém uma identidade estável e normalmente está associado ao seu próprio volume persistente. O Kubernetes consegue gerenciar essa associação dentro de um único cluster, mas o artigo observa que ele não tem uma forma nativa de levar StatefulSets e seus volumes para outro cluster. Essa lacuna é o que obriga a desenhar à mão a estratégia de migração.

> 🔎 Contexto adicionado: um PersistentVolume (PV) é o registro, no cluster, de um volume de armazenamento, e um PersistentVolumeClaim (PVC) é a solicitação de um pod por um desses volumes. Pré-criar um PV que aponta para o ID de um volume existente é como a técnica do artigo vincula um volume antigo a um novo cluster.

## O mecanismo Rolling PV Transplant
O "Rolling PV Transplant" da Yelp depende de duas propriedades. Primeiro, um volume EBS está vinculado a uma Availability Zone, e não a um cluster Kubernetes. Segundo, um PersistentVolume é, na prática, um ponteiro para um ID de volume. A equipe pré-cria PVs e PVCs no EKS que referenciam os IDs dos volumes existentes e, em seguida, cria ali um StatefulSet idêntico, cujos pods ficam aguardando em ContainerCreating.

A automação então remove um pod por vez no cluster legado, espera o volume ser desanexado e deixa o pod no EKS reivindicá-lo. O pod volta ao anel com os mesmos dados e o mesmo intervalo de tokens, e o processo se repete para o próximo pod. A replicação do Cassandra tolera os poucos minutos em que cada nó fica offline.

### Volumes vinculados a AZ
Um volume EBS existe em uma Availability Zone e só pode ser anexado a nós dessa zona. Isso costuma ser tratado como uma limitação, porque impede que um volume acompanhe uma carga de trabalho entre zonas. Aqui, porém, é o que torna a técnica segura: a localização do volume não depende de qual cluster o está executando, então o mesmo volume pode ser desanexado de um cluster e anexado a outro na mesma zona.

> 🔎 Contexto adicionado: um anel Cassandra é o conjunto de nós que compartilham o espaço de tokens, e cada nó é dono de um intervalo de tokens. Manter o mesmo intervalo de tokens no pod substituto é o que mantém os dados consistentes com o restante do anel.

## A flag skipReconciliation
A Yelp definiu a flag `skipReconciliation: true` nos recursos customizados dos dois clusters. Dois operadores estavam gerenciando o mesmo anel lógico durante a migração e, sem a flag, podiam entrar em conflito sobre o estado dos pods. A flag foi removida do lado do EKS ao final da migração.

> ❌ **Anti-padrão: dois controladores sem coordenação no mesmo anel:** dois operadores reconciliando o mesmo anel lógico durante uma mudança. **Por que as pessoas fazem isso:** o artigo não diz por que os operadores antigo e novo rodaram juntos; é uma consequência de mover a carga de trabalho no lugar. **Consequência:** os operadores podem disputar o estado dos pods. **Correção:** a solução do artigo é definir `skipReconciliation: true` nos recursos dos dois clusters durante toda a migração e removê-la do lado do EKS no final.

## Rollback e ritmo de implantação
Em cerca de 1.000 pods, aproximadamente 1.000 TiB foram movidos por ponteiro, e não pela rede. Nenhum nó foi desativado do anel, então o rollback é a mesma operação executada na ordem inversa. A Yelp usou o rollback algumas vezes, por causa de uma definição de subnet ausente e de uma divergência de certificado, e restaurou os pods em minutos.

A implantação seguiu uma semana de execuções manuais em desenvolvimento e uma semana de automação. Em seguida, um engenheiro executou a implantação em produção em cerca de três semanas, migrando de três a seis clusters em um dia típico.

## Script de orquestração gerado pelo Claude
A equipe pediu ao Claude que gerasse o script de orquestração, com cerca de 800 linhas de bash, a partir do runbook manual já validado. O procedimento em si não foi projetado pelo Claude. O script precisou de pequenos ajustes após dois dias de testes.

> 🔎 Contexto adicionado: um runbook é um procedimento escrito, passo a passo. Aqui, a equipe primeiro comprovou cada etapa manualmente e depois pediu ao modelo que transformasse esse procedimento comprovado em código.

## Trabalho anterior com CSI e reuso além do Cassandra
A abordagem depende de uma migração anterior de Container Storage Interface (CSI) feita pela equipe de Compute Infrastructure da Yelp. Esse trabalho deu à equipe de Data Infrastructure confiança no processo de desanexar e anexar volumes. A Yelp também usou a técnica em clusters Kafka e afirma que ela se aplica a qualquer carga de trabalho com estado, fixada em uma AZ, no Kubernetes. Os clusters legados já foram desativados.

## Modos de falha e descobertas não relacionadas
A única falha específica dessa técnica foi um objeto VolumeAttachment obsoleto, deixado pelo controlador CSI da AWS depois da remoção do pod. O pod do EKS ficou em Pending sem nenhum erro. Excluir o objeto no cluster legado resolveu o problema. A Yelp recomenda incluir timeouts explícitos para operações de desanexação e anexação travadas em qualquer automação.

Duas outras descobertas não estavam relacionadas ao mecanismo. Primeiro, os backups falharam com erros de IAM AccessDenied por 48 horas, porque um novo papel do EKS ainda não estava sendo assumido pelos jobs agendados, e o alerta apenas abriu um chamado em vez de acionar uma página. Segundo, ao pausar os reparos para a migração, foi revelado que um cluster tinha os reparos silenciosamente desativados havia três meses, o que deixou inconsistências de dados acumuladas.

> ❌ **Anti-padrão: alertas que abrem chamados em vez de acionar uma página:** uma falha de backup foi registrada como chamado. **Por que as pessoas fazem isso:** o artigo não diz; um chamado é o padrão de menor esforço para problemas não urgentes. **Consequência:** a falha de backup durou 48 horas antes que alguém agisse. **Correção:** tratar falhas de backup como alertas de severidade de acionamento (paging) e verificar a severidade dos alertas antes de começar uma migração.

> ❌ **Anti-padrão: reparos desativados sem que ninguém percebesse:** os reparos de um cluster estavam desligados havia três meses. **Por que as pessoas fazem isso:** o artigo não diz como os reparos foram desativados. **Consequência:** inconsistências de dados se acumularam sem ser vistas. **Correção:** auditar o estado dos reparos antes de iniciar uma migração e tratá-lo como parte do plano.

## 🧠 Modelos mentais
### Ponteiro, não carga
**O modelo:** um PersistentVolume que referencia um ID de volume existente é um ponteiro. Mover o ponteiro move os dados da carga de trabalho sem mover os bytes.
**Como aplicar:** quando um sistema com estado precisa mudar de infraestrutura, pergunte primeiro se o armazenamento pode ser reanexado em vez de copiado, e se as restrições de localização do armazenamento permitem isso.

### Rollback como a mesma operação na ordem inversa
**O modelo:** se nenhum nó sai do anel durante a mudança, reverter é a mesma troca de ponteiro executada para trás, e não um procedimento de recuperação separado.
**Como aplicar:** escolha etapas de migração que mantenham o ambiente antigo válido até o fim, de modo que cada etapa possa ser revertida com as mesmas ferramentas.

## 🔑 Principais takeaways
- A Yelp migrou cerca de 90 clusters Cassandra para o EKS sem indisponibilidade e sem perda de dados, reapontando volumes EBS existentes para novos pods.
- A técnica funciona porque um volume EBS está vinculado a uma Availability Zone, e não a um cluster, e um PersistentVolume é, na prática, um ponteiro para um ID de volume.
- Nenhum nó foi desativado do anel, então o rollback foi a mesma operação na ordem inversa, usado algumas vezes para restaurar pods em minutos.
- Dois operadores sem coordenação no mesmo anel exigiram a flag skipReconciliation para evitar conflitos sobre o estado dos pods.
- Um VolumeAttachment obsoleto deixado pelo controlador CSI da AWS foi a única falha específica da técnica, por isso a automação precisa de timeouts explícitos para desanexação e anexação travadas.
- A Monday.com chegou ao mesmo objetivo com um modelo de transmissão de dados que mantém o ambiente antigo intacto até o fim, ao custo de uma migração mais longa e de executar os dois lados ao mesmo tempo.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. A lição mais útil do artigo para um arquiteto é auditar o estado operacional ao redor antes de uma mudança: cronogramas de reparo, papéis IAM de backup e severidade dos alertas causaram os maiores danos aqui, mais do que a mecânica dos volumes. Uma pergunta que isso levanta para o seu próprio trabalho de plataforma é quais dos seus sistemas com estado precisariam de um caminho de rollback comprovável, e se as restrições de localização de armazenamento que tornam segura uma troca de ponteiro estão documentadas em algum lugar.

## 📚 Plano de leitura
### Do artigo
- Nenhum link para citar: o texto extraído do artigo não contém URLs para o post do blog da Yelp nem para o post da Monday.com. O artigo completo está na URL dos metadados da fonte, que é o melhor lugar para encontrar as referências originais.
### Para ir além
- Post do blog da Yelp, escrito pela equipe de confiabilidade de sites (SRE), sobre a migração do Cassandra para o EKS — leia para ver os detalhes do runbook e as etapas exatas da automação, que o artigo resume.
- Post de engenharia da Monday.com sobre migração do Cassandra com zero indisponibilidade entre clusters EKS — leia para comparar o modelo de transmissão de dados com a abordagem baseada em ponteiros.
- Documentação do Kubernetes sobre StatefulSets e PersistentVolumes — leia para entender os objetos de que a técnica depende, antes de tentar uma mudança semelhante.
- Container Storage Interface (CSI) e objetos VolumeAttachment — leia para depurar operações de desanexação e anexação travadas, como a descrita no artigo.
