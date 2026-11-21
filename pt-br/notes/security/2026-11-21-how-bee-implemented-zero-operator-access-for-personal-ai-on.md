---
type: Article Summary
title: Como a Bee implementou Zero Operator Access para IA pessoal na AWS
description: A Bee comprova que seus servidores executam software aprovado e registrado publicamente antes de liberar as chaves dos usuários, para que os operadores não consigam ler dados sensíveis.
resource: https://aws.amazon.com/blogs/architecture/how-bee-implemented-zero-operator-access-for-personal-ai-on-aws
tags:
- security
- zero-trust-architecture
- remote-attestation
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-bee-implemented-zero-operator-access-for-personal-ai-on-aws
  title: How Bee implemented Zero Operator Access for personal AI on AWS
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-bee-implemented-zero-operator-access-for-personal-ai-on-aws
source: AWS Architecture
source_type: blog
published: '2026-11-20'
domain: security
concepts:
- zero-trust-architecture
- remote-attestation
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d810c89bbfb452ee48a48
lang: pt-br
translation_of: /notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md
---

## 🎯 Tese central
A Bee pode afirmar com credibilidade que seus próprios operadores não conseguem ler os dados sensíveis de IA dos usuários, porque o dispositivo de cada usuário libera chaves apenas para servidores cujas medições de boot e de workload correspondem a software aprovado e registrado publicamente, e porque os controles de rede e organizacionais fecham os caminhos que a atestação não consegue medir.

## 🧭 Por que o autor escreveu
A Bee é uma IA pessoal vestível que escuta o dia do usuário, lembra compromissos e ideias, e lida com conversas sobre saúde, finanças e família. O autor argumenta que esses dados precisam ficar protegidos contra os próprios engenheiros da Bee, outros funcionários da Amazon e os operadores da infraestrutura da AWS, o que o artigo chama de Zero Operator Access (ZOA). Os controles próprios da Bee cobrem os dois primeiros grupos; a proteção contra os operadores da infraestrutura da AWS é herdada do AWS Nitro System. Se a tese estiver correta, o usuário não precisa confiar em uma declaração de política, porque o dispositivo dele pode verificar o software antes de compartilhar qualquer chave.

## Visão geral da solução
O artigo enquadra a segurança com o modelo de responsabilidade compartilhada da AWS: a segurança da nuvem é responsabilidade da AWS, e a segurança na nuvem é responsabilidade do cliente. Os controles de acesso de operadores da AWS são projetados para que o pessoal da AWS não tenha nenhum caminho técnico até os dados dos clientes. A Bee aplica o mesmo princípio aos seus próprios operadores, o que significa endurecer cada camada da pilha.

### Atestação de instâncias EC2
Este é o controle central da Bee. O dispositivo do usuário libera as chaves necessárias para descriptografar seus dados apenas para instâncias que executam software que ele verificou. A atestação prova o que roda dentro de uma instância, mas não consegue medir o que a cerca, como quais redes a instância pode alcançar ou quem pode alterar as permissões da conta. Por isso, a Bee combina a liberação de chaves condicionada à atestação com controles de rede, armazenamento e organizacionais que fecham essas lacunas. A Figura 1 do artigo mostra como os controles se encaixam; apenas a legenda está no texto extraído, então o diagrama não é reproduzido aqui.

### Modelo de responsabilidade compartilhada da AWS
O modelo de responsabilidade compartilhada divide a segurança em dois domínios com dois responsáveis. A AWS é responsável pela segurança da nuvem, e o cliente é responsável pela segurança na nuvem. O artigo usa isso como base: os controles de operadores da AWS protegem a infraestrutura, e a Bee estende a mesma ideia de não acesso às próprias pessoas. É por isso que o artigo separa o que a Bee pode controlar do que herda da AWS.

### Controles de acesso de operadores da AWS
São os controles da AWS que garantem que o pessoal da AWS não tenha nenhuma rota técnica até os dados dos clientes. A Bee depende deles para a camada de infraestrutura e não afirma implementá-los. O trabalho próprio da Bee começa acima dessa camada, com seus operadores e seu software.

### Tempo de build
Um serviço de transparência, executado por uma equipe separada da Amazon que a Bee não controla, mantém uma cópia de cada Amazon Machine Image (AMI), contêiner e modelo aprovado para auditorias posteriores. Ele publica as medições esperadas de cada implantação aprovada no Sigstore Rekor, um log público de transparência. A AMI atestável é compartilhada com a conta da Bee, e as provas de transparência são armazenadas no Amazon S3. No boot, cada instância registra o que está executando no NitroTPM e então obtém um certificado TLS da AWS Private Certificate Authority (AWS Private CA), com as provas de atestação e de transparência incorporadas. O app da Bee verifica esse certificado antes de compartilhar qualquer chave. Se o software tiver sido alterado, o app recusa a conexão.

### Runtime
O app móvel da Bee se conecta apenas a uma instância atestada, por TLS. A instância apresenta seu certificado, e o app verifica a atestação contra a raiz da AWS Nitro e o log de transparência. Somente se ambos corresponderem o app libera uma chave de curta duração. Essa chave existe apenas na memória, nos servidores de aplicação e nos armazenamentos de chaves Redis. Os servidores de inferência nunca recebem chaves. Os dados armazenados no Amazon RDS, no Amazon ElastiCache e no S3 são criptografados antes de sair da fronteira. A visão geral acrescenta que as integrações com AWS Lambda recebem texto plano somente com o consentimento do usuário. O artigo afirma isso de outra forma na seção de rede, onde as funções Lambda nunca veem texto plano. As duas afirmações parecem tratar casos diferentes, mas o artigo não as concilia, e esta nota também não.

## Estabelecer uma base de computação verificável
O primeiro problema que a Bee precisou resolver foi a confiança. O dispositivo do usuário precisa verificar que os servidores da Bee inicializaram apenas com software aprovado, e essa verificação exige evidências ancoradas em um mecanismo fora da própria aplicação do servidor. A Bee usa a atestação de instâncias EC2 para obter medições assinadas do software de boot da instância.

### AMIs atestáveis
A Bee constrói suas imagens base com um pipeline interno e endurecido, que produz AMIs atestáveis para o Amazon EC2. Durante o build, o pipeline calcula medições de referência offline a partir da Unified Kernel Image (UKI) e da configuração do Secure Boot. Esses são os valores esperados para os platform configuration registers (PCRs), que registram medições criptográficas durante o boot. A Bee usa PCR4, PCR7 e PCR12 como alvos de verificação. Como os valores são calculados sem inicializar a imagem, a Bee conhece as medições esperadas antes de qualquer instância ser lançada.

### Propriedades da imagem endurecida
Seguindo a orientação da AWS sobre isolar dados dos próprios operadores, cada AMI atestável da Bee tem estas propriedades:
- Sem acesso interativo: a imagem não inclui SSH, SSM nem shell, então os operadores não têm nenhum caminho para dentro de uma instância em execução.
- Apenas software confiável: a imagem contém somente os componentes de que a carga de trabalho precisa, e cada componente é medido.
- Sistema de arquivos somente leitura e armazenamento local efêmero: o sistema de arquivos raiz é somente leitura, o armazenamento local da instância é criptografado com uma chave mantida apenas na memória, e as instâncias são substituídas em vez de reinicializadas, de modo que nenhum dado sobrevive à instância.

### Documento de atestação do NitroTPM
Em runtime, o bootloader e o sistema operacional medem os componentes de boot e estendem essas medições nos PCRs do NitroTPM. O NitroTPM fornece um documento de atestação com os valores resultantes, assinado pelo AWS Nitro System por meio da AWS Nitro Attestation PKI. Qualquer verificador pode comparar esses valores com as medições de referência aprovadas. Com os mesmos componentes e configuração medidos, os valores de PCR selecionados permanecem consistentes, o que os torna adequados para verificação remota.

## Estender a cadeia de confiança aos seus contêineres e dados
Um SO confiável é necessário, mas não suficiente. Os contêineres de aplicação e os pesos dos modelos da Bee podem mudar independentemente da AMI. Se um usuário não autorizado pudesse trocar uma imagem de contêiner ou um arquivo de modelo sem ser detectado, o documento de atestação ainda mostraria medições de boot limpas, embora a carga de trabalho tivesse sido alterada.

> ❌ **Antipadrão — atestar apenas o SO:** tratar uma medição de boot limpa como prova de que todo o sistema em execução está aprovado. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** uma imagem de contêiner ou um arquivo de modelo pode ser trocado enquanto o documento de atestação ainda mostra medições de boot limpas. **Correção:** medir também a carga de trabalho e incluí-la na mesma atestação assinada, como a Bee faz com o PCR14.

### Medição da carga de trabalho (PCR14)
A Bee resolve isso com uma ferramenta própria de boot medido, que roda antes de qualquer contêiner de aplicação iniciar. A ferramenta resolve cada imagem Docker do manifesto para seu digest imutável no Amazon Elastic Container Registry (Amazon ECR) e obtém o checksum SHA-256 de cada artefato do Amazon S3. Ela calcula um hash determinístico sobre o manifesto, todos os digests dos contêineres e todos os checksums dos artefatos, e então estende o PCR14 do NitroTPM com esse valor. Qualquer alteração em qualquer imagem de contêiner, peso de modelo ou no próprio manifesto produz um PCR14 diferente e um documento de atestação diferente.

### Ordem das medições
O PCR14 é estendido antes de o documento de atestação do NitroTPM ser obtido. O documento de atestação que os servidores de aplicação da Bee apresentam aos clientes, portanto, reflete tanto o estado do SO (PCR4, PCR7, PCR12) quanto o estado da carga de trabalho (PCR14) como uma única medição de hardware assinada.

### Log de transparência (Sigstore Rekor)
Antes da implantação, o serviço de transparência executa um processo de assinatura independente. Ele resolve os mesmos digests do ECR e checksums do S3, recalcula o PCR14 esperado e registra o mapa completo de PCRs no Sigstore/Rekor, um log de transparência independente e auditável publicamente. O bundle de prova resultante é armazenado em buckets do S3 replicados regionalmente, de modo que a verificação no momento do boot é totalmente offline, sem dependência de runtime de infraestrutura externa.

## Validar a atestação em cada conexão
No boot, a instância primeiro confirma que suas próprias medições correspondem à entrada aprovada no log de transparência. Se não corresponderem, ela para por aí. Se corresponderem, solicita um certificado TLS à AWS Private CA, com o documento de atestação e a prova de transparência incorporados ao certificado. A atestação fica vinculada à chave desse certificado, então outra instância não consegue reutilizá-la.

### Verificação do lado do cliente
O dispositivo do usuário decide se confia na instância. Ao conectar, o dispositivo verifica as evidências incorporadas offline, contra raízes embutidas no app, incluindo a raiz da AWS Nitro. Se as medições não corresponderem a uma configuração aprovada, o cliente rejeita a conexão e não compartilha chaves.

### TLS mútuo entre instâncias
Os mesmos certificados protegem o tráfego entre instâncias por TLS mútuo. Apenas os papéis que precisam iniciar conexões, como os servidores de aplicação, recebem certificados de cliente. As instâncias de inferência não os recebem, então não conseguem alcançar os armazenamentos de chaves nem outros serviços.

## Limitar o que a rede pode alcançar
Todas as instâncias EC2 atestadas rodam em sub-redes isoladas de uma Amazon Virtual Private Cloud (Amazon VPC), sem saída para a internet. Os únicos caminhos de saída são endpoints de VPC para o Amazon RDS e o Amazon ElastiCache, e funções AWS Lambda de escopo restrito. As funções Lambda cuidam de integrações externas, como e-mail ou notificações push.

### Integrações Lambda de escopo restrito
No desenho de rede, as funções Lambda recebem apenas dados já criptografados com as chaves dos usuários. Elas nunca veem texto plano. Esta é a afirmação mais restritiva das duas que o artigo faz sobre o Lambda (veja Runtime acima), e o artigo não explica a diferença entre elas.

## Manter as chaves de criptografia fora do armazenamento persistente
As chaves de criptografia dos usuários se originam nos seus dispositivos. Após verificar a atestação de uma instância, o dispositivo libera à instância uma chave de curta duração e com escopo definido. A chave existe apenas na memória, na instância de aplicação e nos armazenamentos de chaves Redis executados dentro de instâncias atestadas com persistência desativada. Ela expira após 7 dias sem atividade do usuário. Como a AWS nunca mantém essa chave, nenhuma camada de armazenamento, backup, política IAM ou concessão do KMS pode dar acesso a ela.

### Texto cifrado em repouso
Os dados sensíveis são criptografados dentro da instância atestada antes de chegar ao Amazon RDS, ao Amazon ElastiCache ou ao Amazon S3, de modo que esses serviços armazenam apenas texto cifrado. O texto plano permanece dentro da fronteira atestada. O artigo remete os leitores ao whitepaper Bee Private Compute para a hierarquia completa de chaves e o fluxo de dados; esse whitepaper não é detalhado aqui.

## Usar controles organizacionais da AWS para eliminar o acesso unilateral
Controles técnicos fortes também exigem limites de permissão fortes. Se uma pessoa pudesse alterar esses controles, eles poderiam ser contornados. A Bee é projetada para que nenhuma equipe isolada consiga fazer isso. A equipe da Bee gerencia a CA privada e o pipeline de deploy, mas não controla o serviço de transparência. A equipe de transparência pode registrar entradas no log do Sigstore, mas não tem acesso às contas AWS da Bee. Implantar código que não aparece no log público de transparência exigiria coordenação entre equipes independentes.

> ❌ **Antipadrão — uma equipe controla tanto os controles quanto o registro deles:** uma única equipe que pode alterar o pipeline de atestação e também o registro de software aprovado. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** essa equipe pode alterar os controles e contorná-los. **Correção:** separar as funções entre equipes independentes, como a Bee faz, e impor essa separação nas contas de nuvem.

### Service control policies (SCPs)
As AWS Service Control Policies (SCPs) sustentam essa separação no nível da infraestrutura. Elas se aplicam a todas as contas da Bee, limitam quais ações são possíveis em cada conta e exigem múltiplos níveis de aprovação para serem alteradas. Isso torna a separação de funções aplicável na própria AWS, e não apenas por meio de processos. As SCPs somam defesa em profundidade. A salvaguarda principal continua sendo a atestação: qualquer alteração no software muda suas medições, e o app da Bee recusa conectar-se a software que não reconhece.

## Conclusão
A conclusão do artigo é que os usuários detêm as próprias chaves e, por isso, decidem quem pode ler seus dados. Antes de o app da Bee compartilhar uma chave, ele verifica que o servidor executa software aprovado e registrado publicamente, e nenhum operador ou política pode sobrepor essa verificação. A atestação de instâncias EC2 torna a verificação auditável. O isolamento de rede, o armazenamento efêmero e os controles organizacionais fecham os caminhos que a atestação não consegue medir.

A Bee está migrando sua inferência para o Amazon Bedrock Mantle, que o artigo descreve como oferecendo zero operator access como capacidade gerenciada. As chaves mantidas pelos usuários e os servidores de aplicação atestados continuam fazendo parte da arquitetura da Bee, de modo que os dados permanecem protegidos contra os operadores da Bee. O artigo apresenta o mesmo padrão como reutilizável para qualquer carga de trabalho que lide com dados sensíveis.

> 📋 **Checklist — Primeiros passos**
> - [ ] Leia a documentação de atestação de instâncias EC2.
> - [ ] Siga o guia sobre como isolar dados dos próprios operadores.
> - [ ] Explore a arquitetura completa no whitepaper Bee Private Compute.
> - [ ] Se você tiver um caso de uso semelhante ou quiser ajuda para construí-lo, entre em contato com a sua equipe de conta da AWS ou com a AWS.

## Leitura complementar
O artigo diz que a Trail of Bits avaliou de forma independente o whitepaper Bee Private Compute, que cobre a arquitetura completa, incluindo a criptografia do lado do cliente. Ele também aponta para o AWS Nitro Sandboxes, que dá a cada pod do Amazon EKS, tarefa do Amazon ECS ou carga de trabalho do Amazon EC2 sua própria VM isolada em hardware, com atestação e zero operator access integrados. Essas são as afirmações do próprio artigo e não foram verificadas aqui.

## Sobre os autores
O texto extraído contém apenas este título e nenhum texto de autoria, então nada é detalhado aqui.

## 🧠 Modelos mentais
### A atestação prova o que roda, não o que o cerca
**O modelo:** Uma medição assinada cobre o software dentro de uma máquina, não as redes, permissões ou contas ao redor. **Como aplicar:** Para cada decisão de confiança, liste o que a medição não consegue ver e dê a cada um desses itens um controle próprio.

### Separação de funções entre equipes independentes
**O modelo:** Um controle só é tão forte quanto a equipe que pode alterá-lo. Se uma equipe pode alterar tanto o controle quanto o seu registro, o controle pode ser contornado. **Como aplicar:** Entregue a CA privada, o pipeline de deploy e o log de software aprovado a responsáveis diferentes, como a Bee faz, e imponha essa separação com políticas de nuvem.

### Substituir em vez de consertar
**O modelo:** As instâncias são substituídas em vez de reinicializadas, então nenhum dado sobrevive a uma instância. **Como aplicar:** Mantenha o estado que precisa sobreviver fora da instância, no sentido do artigo de dados criptografados que saem da fronteira apenas como texto cifrado, e trate a própria instância como descartável.

## 🔑 Principais conclusões
- O controle central da Bee é a liberação de chaves condicionada à atestação: o dispositivo do usuário libera uma chave somente depois de verificar as medições assinadas de boot e de workload da instância.
- Os valores esperados de PCR são calculados offline a partir da imagem antes de qualquer instância ser lançada, então o verificador conhece os valores aprovados de antemão.
- Qualquer alteração em uma imagem de contêiner, peso de modelo ou manifesto muda o PCR14 e o documento de atestação, o que fecha a lacuna que uma medição limpa do SO deixa aberta.
- As provas do Sigstore Rekor tornam o registro de software aprovado publicamente auditável, e a verificação no boot é totalmente offline.
- As chaves existem apenas na memória e expiram após 7 dias sem atividade do usuário. A AWS nunca as mantém, então nenhuma camada de armazenamento, backup ou concessão IAM pode expô-las.
- A separação de funções é imposta com SCPs e aprovação em múltiplos níveis, tendo a atestação como salvaguarda principal.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um arquiteto que trabalha com governança de IA, o padrão transforma "não olhamos os dados dos clientes" de uma promessa de política em um controle que os usuários podem verificar, e a ideia do PCR14 se aplica aos pesos de modelos em qualquer stack de serving. Quais das suas decisões de confiança atuais dependem de uma política ou processo que um controle medido ou imposto poderia substituir, e quem na sua organização seria o responsável pelo registro de transparência?

## 📚 Plano de leitura
### Do artigo
O texto extraído do artigo inclui apenas o texto dos links, não as URLs, então cada item abaixo é identificado pelo texto do link.
- Whitepaper Bee Private Compute — a arquitetura completa, a hierarquia de chaves e a criptografia do lado do cliente, avaliadas de forma independente pela Trail of Bits.
- Documentação de atestação de instâncias EC2 — o mecanismo por trás do controle central.
- Isolando dados dos seus próprios operadores (orientação da AWS) — a base para as propriedades da imagem endurecida.
- Modelo de responsabilidade compartilhada da AWS — a base que o artigo estende.
- AWS Nitro System — a origem da proteção contra os operadores da infraestrutura da AWS.
- Sigstore Rekor — o log público de transparência usado para as medições aprovadas.
### Para saber mais
- Arquitetura zero trust — o princípio de verificar cada conexão independentemente da localização na rede.
- Atestação remota e boot medido com PCRs de TPM — a técnica geral por trás do controle central.
- Logs de transparência para artefatos de software — como um log público torna os builds aprovados auditáveis.
- AWS Nitro Sandboxes — a indicação do artigo para isolamento de hardware por carga de trabalho em contêineres.
- AWS Service Control Policies — como a separação de funções é imposta entre contas.

## Conceitos
- [zero-trust-architecture](/pt-br/concepts/zero-trust-architecture.md)
- [remote-attestation](/pt-br/concepts/remote-attestation.md)
