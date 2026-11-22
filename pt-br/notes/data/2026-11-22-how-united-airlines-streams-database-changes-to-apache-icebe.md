---
type: Article Summary
title: Como a United Airlines transmite alterações de banco de dados para o Apache Iceberg com o Amazon Data Firehose
description: Um pipeline CDC serverless do Aurora MySQL para o Iceberg fornece dados quase em tempo real que vários mecanismos de consulta podem ler.
resource: https://aws.amazon.com/blogs/architecture/how-united-airlines-streams-database-changes-to-apache-iceberg-with-amazon-data-firehose
tags:
- data
- dead-letter-queue
- change-data-capture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-united-airlines-streams-database-changes-to-apache-iceberg-with-amazon-data-firehose
  title: How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-united-airlines-streams-database-changes-to-apache-iceberg-with-amazon-data-firehose
source: AWS Architecture
source_type: blog
published: '2026-11-18'
domain: data
concepts:
- dead-letter-queue
- change-data-capture
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d8141800adc0b590c2faa
lang: pt-br
translation_of: /notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md
---

## 🎯 Tese central
Um pipeline serverless de captura de dados de alteração (CDC) do Aurora MySQL para tabelas Apache Iceberg no Amazon S3 pode substituir cargas em lote defasadas de dados de referência por dados quase em tempo real, que vários mecanismos de consulta conseguem ler, e o intervalo de buffer do Firehose permite trocar latência por custo.

## 🧭 Por que o autor escreveu
A United Airlines precisava que os dados de referência do seu banco de dados operacional Aurora MySQL chegassem aos analistas em poucos minutos enquanto migrava para uma nova plataforma analítica. O processo legado em lote não acompanhava esse ritmo, então os analistas trabalhavam com dados de mercado defasados e as decisões de negócio eram atrasadas. Se a tese se sustentar, um pipeline de streaming gerenciado fecha essa lacuna sem uma aplicação personalizada nem servidores para a etapa de entrega.

## Introdução e notas de escopo
A abertura não tem título no original. Ela apresenta a United Airlines, que o artigo descreve como a maior companhia aérea do mundo em receita por milha voada de passageiros, e o problema descrito acima. O pipeline da United envia as alterações do banco de dados do Aurora MySQL pelo Kinesis Data Streams, Lambda, Firehose e Iceberg no S3. Os resultados relatados são estes. Consultas que varrem mais de 10 milhões de linhas deixam de atingir os gargalos do SQL legado. Várias centenas de analistas de negócio conseguem executar consultas complexas sobre um domínio de dados mais amplo. A latência média das consultas melhorou 5x nos testes da United. Como essas consultas rodam dentro de jobs ETL do AWS Glue a jusante, o tempo total de processamento dos jobs caiu 2x, enquanto o tempo de execução cobrado do Glue também diminuiu. Depois de gravados, os dados podem ser consultados pelo AWS Glue, Amazon Redshift e Amazon Athena, e qualquer mecanismo compatível com Iceberg consegue lê-los.

### Captura de dados de alteração (CDC)
CDC é a prática de ler o log de alterações de um banco de dados e emitir cada inserção, atualização e exclusão como um evento, em vez de copiar tabelas inteiras periodicamente. Um consumidor aplica esses eventos a uma cópia dos dados, de modo que a cópia permaneça atualizada com muito menos trabalho do que uma recarga completa. Isso importa quando as alterações na origem precisam chegar aos analistas em poucos minutos. Nessa arquitetura, o mecanismo de CDC é o AWS DMS, que lê o binary log do MySQL.

### Amazon Kinesis Data Streams
O Kinesis Data Streams é um stream gerenciado e durável que fica entre o mecanismo de CDC e o serviço de entrega. Ele armazena em buffer os eventos de alteração, de modo que a replicação da origem e o processamento a jusante ficam desacoplados. Pode operar no modo sob demanda, que ajusta a capacidade automaticamente conforme o volume de alterações cresce. O template, em vez disso, usa quatro shards provisionados com retenção de 24 horas, que o artigo diz não estarem dimensionados para nenhuma vazão específica.

### Amazon Data Firehose
O Firehose lê os eventos de alteração do stream, agrupa-os em lotes, opcionalmente transforma cada lote com o Lambda, grava arquivos Parquet na tabela Iceberg, aplica cada inserção, atualização e exclusão e confirma um novo snapshot. O ponto do artigo é que você não constrói uma aplicação separada nem gerencia servidores para essa etapa de entrega. O agrupamento é governado por um tamanho e um intervalo de buffer, tratados na seção de processamento abaixo.

### Apache Iceberg
O Iceberg é um formato de tabela aberto cujos arquivos de dados e metadados ficam em armazenamento de objetos. O artigo atribui a ele transações ACID e viagem no tempo (time travel), que considera úteis para exigências regulatórias. Como as tabelas são abertas e catalogadas no AWS Glue Data Catalog, qualquer mecanismo compatível com Iceberg pode lê-las. O template exige format-version 2 e uma chave única.
> 🔎 Contexto adicional: uma tabela Iceberg registra cada commit como um snapshot, de modo que os leitores veem um estado consistente. A viagem no tempo lê um snapshot anterior, que é o que o artigo quer dizer com time travel.

### Amazon S3 e Parquet
O S3 armazena os arquivos de dados e de metadados do Iceberg. Os dados ficam no formato colunar Parquet, ao qual o artigo atribui durabilidade, escalabilidade e baixo custo de armazenamento. As tabelas da United já somam mais de 2 TB. O armazenamento não é o único custo: cada atualização também grava um arquivo de exclusão, o que adiciona cobranças de PUT no S3.

### AWS DMS Serverless
O DMS Serverless executa a replicação sem dimensionamento ou gerenciamento de instâncias de replicação, e o passo a passo o usa por esse motivo. A própria United usa o DMS provisionado padrão. O modo serverless tem limites: não oferece suporte a pontos de início de CDC personalizados, então uma replicação parada por mais tempo que a janela de retenção do binlog precisa de uma recarga completa. Ele também roda dentro da sua VPC, sem IP público.

**Nota sobre este passo a passo.** O artigo baseia o passo a passo na versão de pré-produção da United, mas o simplifica.
- Mecanismo de CDC: qualquer mecanismo que consiga gravar eventos de alteração no Kinesis e que suporte a sua origem funciona. Apenas a transformação Lambda é específica do mecanismo, então ela é reescrita quando você troca de mecanismo. O pipeline a jusante permanece o mesmo.
- Mecanismo de consulta: os analistas da United usam principalmente o Redshift Serverless. O passo a passo valida apenas com o Athena.
- Escopo: o template do CloudFormation é um ponto de partida representativo, não a solução de produção completa.
- Configurações operacionais: o template usa valores de demonstração. Ele registra logs em nível debug, usa quatro shards provisionados com retenção de 24 horas e define o buffer do Iceberg no Firehose como 1 MB ou 3 segundos, com duração de retentativa de 300 segundos. Um buffer curto confirma um snapshot e arquivos de exclusão a cada poucos segundos.
- Tabela única: o template replica uma tabela de origem em uma tabela Iceberg. Para mais tabelas, é preciso estendê-lo.
- Evolução de esquema: não é coberta pelo passo a passo (veja o conceito abaixo).

### Evolução de esquema
Evolução de esquema significa tratar alterações feitas na origem com instruções DDL ao longo do tempo. O artigo diz que o Firehose trata algumas delas sozinho. Se um registro de origem tiver um campo novo que não existe na tabela, mas todos os demais nomes de coluna e tipos de dados coincidirem, o Firehose ignora o campo novo. Se os nomes ou tipos de coluna não coincidirem, o Firehose retorna um erro e envia o registro para o bucket de erros no S3. Na United, a evolução de esquema é tratada fora desse padrão, e não na transformação Lambda.

> ❌ **Anti-padrão: tratar DDL na transformação.** Colocar as alterações de esquema dentro da função Lambda que reescreve cada registro. **Por que as pessoas fazem isso:** o artigo não diz; a transformação é o único código que o pipeline controla. **Consequência:** o artigo diz que a United mantém a evolução de esquema fora do padrão, para que a transformação permaneça específica do mecanismo. **Correção:** tratar as mudanças de esquema em um processo separado, como faz a United.

## Passo a passo da solução
O passo a passo implementa CDC quase em tempo real do Aurora MySQL para o Iceberg com serviços gerenciados. O CloudFormation automatiza o pipeline de replicação, enquanto você configura os endpoints de origem e de destino manualmente. O fluxo de dados tem sete etapas. O mecanismo de CDC lê o binary log do Aurora MySQL. O Kinesis Data Streams recebe e armazena em buffer os eventos. O Firehose lê o stream e agrupa os registros em lotes. O Firehose invoca uma função Lambda programada para o formato de registro do mecanismo de CDC. O Firehose grava arquivos Parquet na tabela Iceberg no S3. O Firehose confirma cada gravação como um novo snapshot Iceberg, atualizando o ponteiro de metadados da tabela no Glue Data Catalog. O Athena e o Redshift Serverless consultam os metadados no catálogo e leem os arquivos de dados do S3. O artigo se refere à Figura 1 para esse pipeline, mas a figura não está no texto extraído.

## Ingestão e streaming de dados
O mecanismo de CDC captura as alterações com impacto mínimo sobre o banco de dados de origem. As alterações são transmitidas continuamente para o Kinesis Data Streams, que oferece streaming em tempo real durável e escalável e faz o buffer entre a replicação da origem e o processamento a jusante. O Firehose consome o stream e agrupa os registros com base no tamanho e no intervalo de buffer configurados, o que reduz a sobrecarga de infraestrutura.

### Intervalo de buffer do Firehose
O intervalo de buffer define o equilíbrio entre latência e custo. Em um teste de baixo volume com cerca de 120 linhas, a United definiu um intervalo de 1 segundo e observou latência de ponta a ponta de até 10 segundos. Para uma carga inicial de cerca de 1,5 milhão de linhas, a United usou um intervalo de 60 segundos para equilibrar latência, custo e tamanho de arquivo. Essa carga terminou em cerca de 2 horas e 22 minutos, o que equivale a cerca de 176 linhas por segundo. O artigo diz que a United mediu essas latências manualmente em relação à vazão média de linhas, então elas devem ser tratadas como indicativas.

## Processamento e armazenamento
O Lambda e o Firehose processam o stream em tempo quase real. O Lambda analisa cada registro de CDC, extrai a operação (INSERT, UPDATE ou DELETE) e converte o registro para um formato compatível com Iceberg, com o campo de operação correto. Em seguida, o Firehose confirma as gravações como snapshots Iceberg. Ambos os serviços escalam com o stream. Os limites fixos são os quatro shards do Kinesis do template e a faixa de capacidade do DMS de 1 a 2 DCUs.

### Transformação com AWS Lambda
A função Lambda é a única peça do pipeline específica do mecanismo. Ela repassa cada linha e adiciona a tabela de destino e a operação que o Firehose precisa para roteá-la. O Firehose usa esses metadados para inserir, atualizar ou excluir a linha na tabela Iceberg correta. Como a função analisa o formato de registro de um único mecanismo, você a reescreve sempre que trocar de mecanismo de CDC. O template a executa no Python 3.14.

### Transações ACID e controle de concorrência otimista
O Firehose confirma cada gravação como uma transação ACID, ou seja, atômica, consistente, isolada e durável, usando controle de concorrência otimista. Os metadados da tabela são atualizados automaticamente a cada commit. É por isso que os leitores veem lotes inteiros ou nada. O artigo não descreve tratamento de conflitos além disso, então não presuma que gravadores concorrentes são seguros sem verificar.

### Merge-on-read
O Firehose aplica as operações de CDC com merge-on-read. Um UPDATE é gravado como um arquivo de exclusão mais um novo arquivo de dados, e um DELETE é gravado como um arquivo de exclusão. O mecanismo de consulta faz a mesclagem no momento da leitura para resolver o estado atual da linha. O modo exige que a tabela Iceberg seja format-version 2 e tenha uma chave única, o que o template define no delivery stream.

### Chave de partição do DMS
O DMS envia cada registro ao Kinesis com uma chave de partição. O template não define nenhuma regra de chave de partição para o DMS, então o DMS usa o esquema e o nome da tabela. Assim, todas as alterações da tabela caem em um único shard, e os outros três shards ficam ociosos. A vazão fica limitada ao que um shard suporta. A correção é definir a chave de partição como a chave primária da tabela em uma regra de mapeamento de objetos do DMS. Isso distribui a carga e mantém a ordem por linha.

> ❌ **Anti-padrão: chave de partição padrão.** Deixar o DMS com a chave de partição de esquema e tabela, de modo que o stream de alterações de uma tabela caia em um único shard. **Por que as pessoas fazem isso:** o artigo não diz por quê; o template é entregue sem a regra. **Consequência:** três dos quatro shards ficam ociosos e a vazão fica limitada a um shard. **Correção:** definir a chave de partição como a chave primária da tabela em uma regra de mapeamento de objetos.

A United pré-criou suas tabelas Iceberg com o Athena, e essas tabelas hoje somam mais de 2 TB de dados.

## Metadados e análise
O artigo descreve três superfícies de consulta sobre as mesmas tabelas. O AWS Glue Data Catalog é o repositório central de metadados. Ele armazena os metadados das tabelas Iceberg, as versões de esquema e as informações de partição, e oferece suporte à evolução de esquema e à governança em toda a organização. O Redshift Serverless consulta as mesmas tabelas pelo catálogo. O Athena oferece SQL com cobrança por consulta para análises pontuais e validação de dados.

### AWS Glue Data Catalog
O catálogo é a camada compartilhada de metadados. As tabelas Iceberg são registradas aqui, e o ponteiro para os metadados atuais da tabela avança a cada commit do Firehose. Os mecanismos consultam a tabela no catálogo e depois leem os arquivos de dados do S3. O artigo também sugere adicionar contexto de negócio ao catálogo para ajudar agentes de IA a descobrir dados.

### Amazon Redshift Serverless
O Redshift Serverless lê as tabelas Iceberg de duas formas. Você pode referenciar a tabela pelo banco awsdatacatalog montado automaticamente, ou criar um esquema externo sobre o banco do Glue. Os analistas da United usam principalmente o Redshift Serverless.

### Amazon Athena
O Athena consulta as mesmas tabelas pelo catálogo. Não exige infraestrutura para gerenciar, e você paga por consulta. No teste do passo a passo, um UPDATE e um DELETE na origem retornaram as linhas atuais corretas tanto no Athena quanto no Redshift Serverless.

## Criação da solução
As etapas de criação vêm em ordem: pré-requisitos e, depois, os Passos 1 a 10. Elas misturam trabalho no console, comandos da AWS CLI e uma implantação do CloudFormation.

## Pré-requisitos
> 📋 **Checklist: pré-requisitos** (itens resumidos até o requisito; a redação completa está no artigo)
> - [ ] Um cluster Aurora MySQL com binary logging habilitado.
> - [ ] Um segredo no Secrets Manager contendo os detalhes de conexão do Aurora, criado manualmente, com o ARN anotado.
> - [ ] Uma função de acesso do IAM que o DMS pode assumir para ler esse segredo.
> - [ ] Uma VPC com sub-redes privadas, regras de grupo de segurança permitindo a porta 3306, um endpoint de interface VPC do Kinesis e um endpoint de interface VPC do Secrets Manager.
> - [ ] Permissão para criar o endpoint e a replicação do DMS, o stream do Kinesis, a Lambda, o Firehose, grupos de logs, uma chave KMS, três funções do IAM e a stack, com CAPABILITY_NAMED_IAM.
> - [ ] AWS CLI instalada e configurada.

O artigo acrescenta que o segredo gerenciado pelo Aurora omite host e porta e não funciona com o DMS, então o segredo precisa ser criado manualmente. A política de confiança da função de acesso permite a entidade de serviço regional do DMS, por exemplo dms.us-east-1.amazonaws.com, e a política de permissões concede secretsmanager:GetSecretValue sobre o ARN do segredo. Como a stack cria funções do IAM, a identidade que faz a implantação também precisa de iam:CreateRole, iam:PutRolePolicy e iam:PassRole.

### Endpoints de interface VPC
O DMS Serverless roda dentro da sua VPC, sem IP público, e a replicação executa de forma privada, sem acesso à internet. Por isso, o passo a passo cria um endpoint de interface do Kinesis Data Streams e um endpoint de interface do Secrets Manager. Ambos precisam de DNS privado. O grupo de segurança do próprio cluster Aurora também precisa permitir MySQL de entrada na TCP 3306 a partir do grupo de segurança da replicação, porque o cluster fica fora da stack.

> ❌ **Anti-padrão: segredo gerenciado pelo Aurora para o DMS.** Apontar o DMS para o segredo que o Aurora cria. **Por que as pessoas fazem isso:** é o segredo padrão que o Aurora fornece. **Consequência:** o artigo diz que ele omite host e porta, então o DMS não funciona com ele. **Correção:** criar um segredo com usuário, senha, host e porta, e referenciar o seu ARN.

## Passo 1: Configurar o banco de dados de origem
O artigo pressupõe um cluster Aurora MySQL existente, testado na versão 3.08.0, compatível com o MySQL 8.0. Esta etapa não é automatizada.

A primeira parte configura o binary logging. Em um grupo de parâmetros de cluster de banco de dados personalizado, defina binlog_format como ROW, binlog_row_image como FULL e binlog_checksum como NONE. Esses valores vêm do grupo de parâmetros do cluster e não podem ser definidos com SET GLOBAL. Associe o grupo ao cluster e depois reinicie o writer, porque binlog_format é estático.

### Binary log e formato ROW
O binary log registra as alterações feitas no banco de dados, e o CDC o lê. O formato ROW registra as alterações reais das linhas, e não as instruções SQL. A imagem de linha FULL registra todas as colunas, então cada evento de alteração carrega a linha completa. Juntos, eles dão ao mecanismo de CDC dados suficientes para montar inserções, atualizações e exclusões corretas.

### Janela de retenção do binlog
A janela de retenção é o período em que o banco de dados mantém o binary log. O artigo a define como 24 horas com um procedimento armazenado e diz que ela deve ser maior que o maior tempo em que a replicação possa ficar parada. O Aurora MySQL versão 3 permite até 2.160 horas, ou seja, 90 dias. O DMS Serverless não oferece suporte a pontos de início de CDC personalizados, então uma replicação que fique parada além da janela precisa reiniciar com uma recarga completa.

Depois da reinicialização, verifique se log_bin está ON, binlog_format é ROW, binlog_row_image é FULL e binlog_checksum é NONE. A verificação de log_bin é a mais importante, porque confirma que o binary logging está ativo, e não apenas configurado. Se log_bin estiver OFF, confirme que o grupo de parâmetros está associado e reinicie o writer novamente.

O artigo observa que o passo a passo usa o usuário administrador do Aurora tanto para este SQL quanto para o endpoint de origem do DMS. Em produção, o artigo recomenda um usuário de replicação dedicado, com apenas REPLICATION CLIENT, REPLICATION SLAVE e SELECT na tabela de origem.

> ❌ **Anti-padrão: usuário administrador para CDC.** Usar a conta administradora do banco como usuário de origem do DMS. **Por que as pessoas fazem isso:** é a opção mais simples para um passo a passo, e o artigo diz que a usa por simplicidade. **Consequência:** a replicação tem muito mais privilégios do que precisa. **Correção:** criar um usuário de menor privilégio com somente os três privilégios listados.

## Passo 2: Criar o endpoint de origem do mecanismo de CDC
O endpoint de origem é criado manualmente no console do DMS, antes de a stack ser implantada. As configurações do artigo são: tipo de endpoint de origem, identificador mysql-source-endpoint, mecanismo de origem Amazon Aurora MySQL, o endpoint do writer como nome do servidor e a porta 3306. Para as credenciais, referencie o segredo do Secrets Manager e a função de acesso, o que é recomendado. Você também pode informar usuário e senha diretamente.

### Modo SSL
O passo a passo deixa o modo SSL como none, que é o padrão para endpoints do Aurora MySQL. O artigo diz que isso deixa a conexão entre o DMS e o Aurora sem criptografia. Para produção, recomenda verify-ca ou verify-full, com o certificado de CA da RDS anexado. No Aurora MySQL 8.4, require_secure_transport vem ativado por padrão e rejeita o modo SSL none, então o SSL precisa estar habilitado nesse caso.

> ❌ **Anti-padrão: configuração SSL de demonstração.** Deixar o modo SSL como none em produção. **Por que as pessoas fazem isso:** é o padrão, e o passo a passo o mantém. **Consequência:** o stream de alterações atravessa a rede sem criptografia e, na versão 8.4, a conexão é simplesmente rejeitada. **Correção:** definir verify-ca ou verify-full e anexar o certificado de CA da RDS.

Guarde o ARN do endpoint para o parâmetro de stack DmsSourceEndpointArn. O ARN do segredo vai em DmsSourceSecretArn.

## Passo 3: Configurar a infraestrutura de VPC
A VPC com sub-redes privadas é pressuposta e não é criada pelo CloudFormation. Usuários do DMS que estão começando precisam criar uma função de serviço com o nome exato dms-vpc-role, com uma política de confiança para dms.amazonaws.com e a política gerenciada pela AWS AmazonDMSVPCManagementRole. Sem ela, a criação do grupo de sub-redes falha com o erro "IAM Role dms-vpc-role is not configured properly". O console normalmente cria essa função automaticamente, mas a CLI e a API não.

### Grupo de sub-redes do DMS
O grupo de sub-redes precisa de pelo menos duas sub-redes privadas em zonas de disponibilidade diferentes. Seu ARN tem o formato arn:aws:dms:<region>:<account-id>:subgrp:<subnet-group-name>, e você o guarda para a stack.

### Grupo de segurança do DMS
O grupo de segurança da replicação permite HTTPS de entrada na porta 443 a partir do CIDR da VPC, para que a replicação consiga alcançar os endpoints de interface. A saída permite HTTPS na porta 443 para os serviços da AWS e MySQL na porta 3306 para o cluster Aurora. A saída padrão de todo o tráfego também funciona. Depois, adicione uma regra MySQL de entrada no grupo de segurança do cluster Aurora, a partir deste grupo.

Os dois endpoints de interface aparecem como Pending por um momento e depois como Available.

## Passo 4: Criar a infraestrutura de destino
Estes recursos precisam existir antes da implantação: um bucket do S3, um banco de dados no Glue Data Catalog e a tabela Iceberg. O Athena precisa de um bucket de resultados antes de tudo. Os comandos dos itens 1 e 2 são comandos da AWS CLI. O nome do bucket é um marcador, amzn-s3-demo-bucket, porque os nomes do S3 são globais. Use o seu próprio nome na localização da tabela do Glue e no parâmetro S3BucketName.

O banco do Glue, market_db, é um recurso diferente do banco do Aurora com o mesmo nome. Os nomes coincidem apenas por convenção. A tabela Iceberg é criada com DDL do Athena, que a registra no Glue Data Catalog.

O artigo avisa que, se a localização S3 da tabela estiver registrada no Lake Formation, você precisa criar a tabela antes de registrá-la. O Athena não oferece suporte a DDL em tabelas Iceberg cuja localização esteja registrada no Lake Formation.

## Passo 5: Implantar a stack do CloudFormation
O template provisiona o stream do Kinesis, a replicação do DMS Serverless, a função Lambda, o delivery stream do Firehose, as funções do IAM e os logs do CloudWatch. Ele não cria o endpoint de origem, a infraestrutura de VPC, o bucket do S3, o banco do Glue nem a tabela Iceberg. Os recursos da stack são: um stream do Kinesis com quatro shards e retenção de 24 horas, replicação do DMS Serverless com 1 a 2 DCUs no modo full-load-and-CDC, um endpoint de destino do Kinesis, a função Lambda, um delivery stream do Firehose com destino Iceberg, funções do IAM, uma chave KMS para criptografia dos logs e grupos de logs.

### CloudFormation
Você cria a stack no console enviando o template do aws-samples, nomeando-a iceberg-replication-stack e definindo os parâmetros. Entre eles estão DmsSourceEndpointArn, DmsSubnetGroupArn, DmsVpcSecurityGroupId, DmsSourceSecretArn, a tabela de origem do DMS market_rule, o banco do Glue market_db, a tabela do Glue market_rule, IcebergUniqueKey rule_id e o seu bucket do S3. Em seguida, você confirma a criação de recursos do IAM. A stack chega a CREATE_COMPLETE quando termina.

### Lake Formation
Se a conta usa permissões do Lake Formation para o Glue Catalog em vez do padrão somente com IAM, a função do Firehose precisa ser autorizada antes da implantação. Caso contrário, a criação da stack pode falhar. Você pode conceder à função do Firehose o privilégio ALL do Lake Formation em market_db.market_rule, ou mudar o banco e a tabela para acesso baseado em IAM, concedendo IAM_ALLOWED_PRINCIPALS ALL. O artigo diz que a segunda opção enfraquece a governança do Lake Formation, porque então apenas as políticas do IAM controlam o acesso. Também diz que, em produção, você deve conceder somente as permissões mínimas necessárias.

> ❌ **Anti-padrão: atalho IAM_ALLOWED_PRINCIPALS.** Conceder IAM_ALLOWED_PRINCIPALS ALL para que a criação da stack funcione. **Por que as pessoas fazem isso:** é a forma mais rápida de passar por uma implantação de stack que falha. **Consequência:** a governança do Lake Formation fica enfraquecida, porque somente o IAM controla o acesso. **Correção:** conceder apenas as permissões mínimas do Lake Formation de que a função do Firehose precisa.

## Passo 6: Validar a replicação do mecanismo de CDC
Inicie a tarefa do DMS pelo console e escolha Start task. Você pode deixar a avaliação pré-migração ativada ou desativá-la. O provisionamento da capacidade serverless pode levar até cerca de 40 minutos na primeira inicialização, e a tarefa mostra "Provisioning capacity" durante esse tempo. O artigo diz que isso é normal, não uma falha.

### Estatísticas de tabela do DMS
Na visão detalhada da replicação, verifique três coisas. O estado de carga deve mostrar "Table completed". A contagem de linhas da carga completa deve coincidir com a da tabela de origem. As operações de CDC devem mostrar atividade contínua de INSERT, UPDATE e DELETE. O exemplo do artigo mostra market_rule com Table completed e Total rows 2.

Depois, execute SELECT * FROM market_db.market_rule no Athena e verifique se a contagem de linhas coincide com a contagem de 2 linhas da carga completa do DMS. O artigo diz que isso confirma que as linhas chegaram à tabela Iceberg, e não apenas ao destino do Kinesis.

## Passo 7: Gerar um evento de CDC
Com a carga completa concluída e a tarefa mostrando "Load completed, replication ongoing", insira uma linha de teste no banco de dados Aurora MySQL de origem. O mecanismo de CDC lê a alteração do binary log e a transmite pelo pipeline. As etapas seguintes verificam cada estágio. A instrução de inserção não está no texto extraído.

## Passo 8: Verificar a transformação do Lambda
No console do Lambda, abra a função chamada [stack-name]-lambda-processor e verifique a aba Monitor. As invocações devem estar registradas, os erros devem ser 0 e a taxa de sucesso deve ser 100%. Os logs devem mostrar a detecção de INSERT, UPDATE ou DELETE e a transformação bem-sucedida, sem erros. O artigo se refere à Figura 9, que está ausente do texto extraído.

## Passo 9: Monitorar a entrega do Firehose
No console do Firehose, abra a aba Monitoring do stream. Os registros recebidos devem mostrar dados fluindo do Kinesis. A entrega ao Iceberg deve corresponder aos registros recebidos, com Failed em 0. O artigo destaca o DataFreshness. Ele deve permanecer estável, e um valor crescente significa que o Firehose está ficando para trás. Quando o Firehose tiver entregue o registro, consulte-o no Athena. O novo registro aparecendo na tabela Iceberg confirma a replicação quase em tempo real.

O artigo sugere um teste opcional: atualize uma linha e exclua outra na origem, depois consulte ambas no Athena e no Redshift Serverless com SELECT * FROM awsdatacatalog.market_db.market_rule, e confirme que as duas retornam as linhas atuais.

## Passo 10: Configurar o painel de monitoramento
Esta etapa é opcional. Crie um painel do CloudWatch com estas métricas:
- DMS CDCLatencySource e CDCLatencyTarget, que mostram o quanto a replicação está atrasada em relação à origem e ao destino.
- Kinesis GetRecords.IteratorAgeMilliseconds, que mostra o quanto o Firehose está atrasado em relação ao stream.
- Lambda Invocations e Errors da função de transformação.
- Firehose DeliveryToIceberg.DataFreshness e DeliveryToIceberg.FailedRowCount.

O artigo diz para definir os limites dos alarmes com base nos seus próprios requisitos de latência.

## Monitoramento operacional
Depois da implantação, a United estabeleceu procedimentos para manter as tabelas Iceberg com bom desempenho. Cada commit do Firehose cria novos arquivos de dados, e cada atualização também grava um arquivo de exclusão, então uma tabela movimentada acumula muitos arquivos pequenos que tornam as leituras mais lentas. A compactação resolve isso, seja pela compactação automática no Glue Data Catalog, seja por um OPTIMIZE agendado no Athena. O armazenamento é recuperado com o VACUUM do Athena, que expira snapshots antigos e remove arquivos órfãos. A United também monitora a saúde da replicação e tem um procedimento definido para reiniciar e reconciliar dados se uma tarefa parar ou falhar.

> ❌ **Anti-padrão: nenhuma manutenção de tabela.** Executar o pipeline sem compactação nem VACUUM. **Por que as pessoas fazem isso:** o pipeline funciona desde o primeiro dia, então a manutenção é fácil de adiar. **Consequência:** arquivos pequenos e arquivos de exclusão se acumulam e tornam as leituras mais lentas. **Correção:** agendar OPTIMIZE ou habilitar a compactação do catálogo, e executar VACUUM para expirar snapshots.

## Considerações de custo
O artigo traz os preços de lista da us-east-1 na data da publicação e pede que o leitor confira as tarifas atuais.
- DMS Serverless: US$ 0,0819 por DCU-hora para Single-AZ e US$ 0,1639 para Multi-AZ. O template é Multi-AZ com 1 a 2 DCUs, então cerca de US$ 0,16 a US$ 0,33 por hora enquanto a replicação estiver em execução.
- Kinesis Data Streams: US$ 0,015 por shard-hora, ou US$ 0,06 por hora para quatro shards, mais US$ 0,014 por milhão de unidades de payload PUT.
- Lambda: cobrado por requisição e duração.
- Firehose: US$ 0,045 por GB para entrega Iceberg a partir do Kinesis (primeiros 250 TB por mês). Os bytes extras adicionados pela transformação Lambda são cobrados à mesma taxa.
- S3: armazenamento mais requisições PUT. O arquivo de exclusão de cada atualização adiciona cobranças de PUT.
- Endpoints VPC: os endpoints de interface do Kinesis e do Secrets Manager são cobrados por hora por zona de disponibilidade, mais US$ 0,01 por GB processado.
- Manutenção de tabelas: a compactação do Glue ou o OPTIMIZE do Athena geram cobranças do Glue ou do Athena.

## Limpeza
> 📋 **Checklist: ordem de limpeza**
> - [ ] Pare a replicação do DMS Serverless e aguarde até que ela mostre Stopped. O DMS só pode excluir uma replicação serverless quando ela está parada ou com falha.
> - [ ] Exclua a stack do CloudFormation. Isso remove a replicação, o endpoint de destino do Kinesis, o stream do Kinesis, a Lambda, o stream do Firehose, as funções do IAM, a chave KMS e os grupos de logs.
> - [ ] Exclua os recursos criados manualmente: o endpoint de origem do DMS, os endpoints de interface do Kinesis e do Secrets Manager, o grupo de sub-redes e o grupo de segurança do DMS, a tabela e o banco do Glue, o bucket do S3 (esvazie-o antes, incluindo os prefixos firehose-errors/ e processing-failed/), o segredo do Secrets Manager e sua função de acesso, quaisquer concessões do Lake Formation adicionadas para o Firehose, a dms-vpc-role se nenhum outro recurso do DMS a usar, e o cluster Aurora e o grupo de parâmetros, caso não queira mantê-los.

A etapa de verificação do artigo é confirmar que a stack do CloudFormation aparece como excluída e que o console não mostra recursos cobráveis ainda ativos.

## Conclusão
A conclusão do artigo é que essa arquitetura serverless oferece replicação quase em tempo real do Aurora MySQL para o Iceberg, com latência de ponta a ponta de até cerca de 10 segundos. A latência pode ser trocada por custo pelo intervalo de buffer do Firehose. Os componentes de pagamento por uso mantêm o gasto vinculado ao consumo. O Iceberg adiciona transações ACID e viagem no tempo, e o AWS Glue, o Amazon Redshift e o Amazon Athena podem consultar os mesmos dados. A United usou o resultado para ampliar o uso do Iceberg em análises de negócio e para fornecer a várias centenas de usuários da sua plataforma de dados de Gestão de Receita dados de referência quase em tempo real.

## 🧠 Modelos mentais
### O intervalo de buffer é um controle de latência
**O modelo:** o Firehose agrupa registros por um intervalo definido antes de confirmar, então cada commit custa dinheiro e cada lote adiciona atraso. Intervalos curtos reduzem a latência e aumentam a sobrecarga de commits e arquivos; intervalos longos fazem o contrário.
**Como aplicar:** defina o intervalo por tabela, a partir da meta de latência, e verifique o tamanho dos arquivos e o custo, como a United fez ao passar de 1 segundo nos testes para 60 segundos na grande carga inicial.

### A borda específica do mecanismo
**O modelo:** só a transformação Lambda conhece o formato de registro do mecanismo de CDC. Tudo, do Kinesis ao Iceberg, é neutro em relação ao mecanismo.
**Como aplicar:** mantenha a análise específica do mecanismo em uma única função, para que a troca de mecanismo de CDC afete apenas um componente.

### A chave de partição decide tanto a carga quanto a ordem
**O modelo:** a chave de partição determina em qual shard cai cada alteração. Defini-la como a chave primária distribui a carga entre os shards e mantém as alterações de uma linha em ordem.
**Como aplicar:** escolha a chave primária da tabela na regra de mapeamento de objetos do DMS, em vez de aceitar o padrão.

## 🔑 Principais conclusões
- Um pipeline de CDC orientado pelo DMS, com Kinesis, Firehose e Iceberg, entregou latência de ponta a ponta de até cerca de 10 segundos nos testes da United.
- O Firehose confirma cada lote como um snapshot Iceberg ACID e faz a entrega sem uma aplicação personalizada nem servidores.
- O intervalo de buffer do Firehose define o equilíbrio entre latência e custo: a United usou 1 segundo em um teste de 120 linhas e 60 segundos em uma carga de 1,5 milhão de linhas.
- O Iceberg com merge-on-read precisa de format-version 2 e de uma chave única, e cada atualização grava um arquivo de exclusão.
- A chave de partição padrão do DMS envia todo o stream de alterações de uma tabela para um único shard, e a correção é usar a chave primária.
- Configurações de demonstração (logs em debug, quatro shards, buffer de 1 MB ou 3 segundos, SSL none) não são configurações de produção; o artigo lista o que revisar primeiro.
- O Lake Formation deve ser mantido, não contornado com IAM_ALLOWED_PRINCIPALS, e a manutenção das tabelas precisa ser agendada.

## 🏛️ Para Kenji
Este é o padrão para um feed de dados de referência quase em tempo real em uma plataforma Iceberg com múltiplos mecanismos: um arquiteto pesaria o intervalo de buffer como uma decisão de latência e custo, a chave de partição como uma decisão de vazão, e o atalho do Lake Formation como uma decisão de governança. Sua interpretação, não a do autor: o artigo mantém a evolução de esquema fora do pipeline de propósito. Quem é responsável por essa fronteira quando o esquema da origem muda, e o seu modelo de governança cobre isso?

## 📚 Plano de leitura
### Do artigo
- Configurando a replicação por binary log (sem URL no texto extraído) — as configurações de binlog e as regras de retenção das quais a Etapa 1 depende.
- Limitações do AWS DMS Serverless (sem URL no texto extraído) — os limites de ponto de início e de exclusão que moldam o roteiro de replicação.
- Considerações e limitações para tabelas Apache Iceberg (sem URL no texto extraído) — as regras de evolução de esquema e de buffer do Firehose a revisar antes da produção.
- Alterando as configurações padrão do seu data lake (sem URL no texto extraído) — os padrões do Lake Formation que podem quebrar a criação da stack.
- Usando SSL com o AWS Database Migration Service (sem URL no texto extraído) — a configuração de certificado para endpoints de produção.
### Para ir além
- Merge-on-read versus copy-on-write no Apache Iceberg — o equilíbrio entre modos de escrita por trás dos arquivos de exclusão que este pipeline grava.
- Chaves de partição do Kinesis e pontos quentes de shards — como a escolha da chave define se os shards dividem a carga.
- Manutenção de tabelas Iceberg (compactação e expiração de snapshots) — como manter arquivos pequenos e arquivos de exclusão longe de deixar as leituras lentas.

## Conceitos
- [Fila de mensagens mortas (dead-letter queue)](/pt-br/concepts/dead-letter-queue.md)
- [Captura de dados de alteração (change data capture)](/pt-br/concepts/change-data-capture.md)
