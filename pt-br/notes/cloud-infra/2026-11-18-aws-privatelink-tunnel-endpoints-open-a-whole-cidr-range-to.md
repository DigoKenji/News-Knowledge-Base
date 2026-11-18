---
type: Article Summary
title: Endpoints de túnel do AWS PrivateLink liberam uma faixa CIDR inteira para outra conta
description: Endpoints de túnel da AWS permitem que uma VPC consumidora alcance uma faixa CIDR compartilhada inteira em um único sentido, sem peering nem configuração por recurso.
resource: https://www.infoq.com/news/2026/11/aws-privatelink-tunnel-endpoints/
tags:
- cloud-infra
- cloud-platforms
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/aws-privatelink-tunnel-endpoints/
  title: AWS PrivateLink Tunnel Endpoints Open a Whole CIDR Range to Another Account
  author: InfoQ
status: stable
source_url: https://www.infoq.com/news/2026/11/aws-privatelink-tunnel-endpoints/
source: InfoQ
source_type: blog
published: '2026-11-18'
domain: cloud-infra
subdomain: cloud-platforms
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81f982d3d31524991487
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-18-aws-privatelink-tunnel-endpoints-open-a-whole-cidr-range-to.md
---

## 🎯 Tese central
Um endpoint de túnel com escopo CIDR permite que uma VPC consumidora alcance todos os endereços de uma faixa que outra conta compartilha pelo AWS Resource Access Manager, incluindo recursos criados depois do compartilhamento, em um único sentido e sem listar cada recurso.

## 🧭 Por que o autor escreveu o texto
Fornecedores e prestadores de serviço precisam alcançar aplicações dentro da rede de um cliente. Configurações de PrivateLink por recurso deixam de funcionar quando os recursos são efêmeros ou quando o cliente não quer enumerá-los. Se a tese do artigo se sustentar, o acesso do fornecedor pode ser definido como um perímetro de rede, e não como um inventário de hosts.

## Visão geral: um perímetro em vez de uma lista de recursos
A AWS publicou um passo a passo técnico sobre endpoints de túnel, um tipo de VPC endpoint lançado em setembro. O provedor compartilha uma configuração de recurso do tipo CIDR pelo RAM, e o consumidor cria um endpoint de túnel sobre ela. Bruce Pinsky e Ashish Kumar, da AWS, enquadram o recurso como um problema de acesso de fornecedores, para "um fornecedor ou prestador de serviço cujo software precisa alcançar aplicações e recursos na rede de um cliente". O cliente define o perímetro em faixas de endereços, e "recursos localizados fora do perímetro não podem ser alcançados pelo fornecedor".

### Configuração de recurso CIDR
Uma configuração de recurso é o objeto do lado do provedor que descreve o que pode ser alcançado. Uma configuração do tipo CIDR descreve uma faixa de endereços, e não um único host ou banco de dados. Depois de compartilhada, qualquer coisa dentro da faixa fica acessível, inclusive recursos que não existiam quando o compartilhamento foi feito. Isso elimina a necessidade de atualizar o acesso sempre que um host é substituído ou escalado.

### Blocos CIDR sobrepostos
As VPCs consumidora e provedora podem usar blocos de endereços sobrepostos, o que é comum quando muitas redes de clientes usam as mesmas faixas privadas. O endpoint de túnel traduz os endereços na fronteira, então as duas redes não precisam ser renumeradas. O artigo apresenta isso como uma propriedade de projeto, e os testes de campo na seção S5 mostram o que isso custa na prática.

### Início apenas em um sentido
As conexões "só podem começar a partir da VPC consumidora, e não da rede do provedor". O provedor pode responder, mas não pode abrir uma sessão dentro do consumidor. Isso importa para o acesso de fornecedores, porque um provedor comprometido não consegue iniciar tráfego para dentro da rede do cliente.

## Comparação com VPC peering e VPC Lattice
Uma thread do Reddit no fórum r/aws sobre o lançamento de setembro começou com a pergunta sobre o que isso faz que o VPC peering não faz. As respostas trouxeram os casos de uso. Uma conexão de peering expõe a VPC inteira nos dois sentidos. Um endpoint de túnel compartilha um subconjunto do espaço de endereços em um único sentido.

### VPC peering
O peering une duas VPCs no nível de rede, então roteamento e alcance abrangem tudo nas duas. Aí está a fonte do risco. Um comentarista de um banco escreveu que "fazer peering com parceiros não regulados é um grande não, os auditores vão despedaçar você por isso". Trate o peering como uma decisão completa de confiança de rede, e não como uma integração de serviço.

> ❌ **Anti-padrão — fazer peering com um parceiro externo para um único serviço:** um peering completo de VPC para dar a um fornecedor acesso a uma aplicação. **Por que as pessoas fazem isso:** o peering é a ferramenta familiar e é rápido de configurar. **Consequência:** o parceiro pode alcançar todo recurso roteável na VPC com peering, nos dois sentidos. **Correção:** compartilhe apenas a faixa de endereços que o parceiro precisa, com uma configuração de recurso CIDR, que funciona em um único sentido.

### VPC Lattice e camada 4 versus camada 7
Os serviços Lattice operam na camada 7, enquanto os endpoints de túnel operam na camada 4 e não precisam de configuração de recurso por destino. Outro comentarista apontou clusters autogerenciados que fazem balanceamento de carga do lado do cliente, como Kafka ou Aerospike. Esses clusters antes precisavam de roteamento por porta em um Network Load Balancer e de um esquema separado de mapeamento de portas para conexões que chegavam por um endpoint de interface.

## Mecânica do lado do consumidor: encapsulamento GENEVE
A parte incomum está do lado do consumidor. O tráfego da aplicação deve chegar ao endpoint de túnel encapsulado em GENEVE, pela porta UDP 6081, na mesma Availability Zone.

### Encapsulamento GENEVE
GENEVE é o protocolo de encapsulamento definido na RFC 8926. Ele envolve um pacote interno em um cabeçalho UDP externo, para que o pacote interno possa atravessar uma rede que não o entende. O endpoint de túnel remove o cabeçalho externo, verifica se o destino interno está dentro da faixa compartilhada, reescreve a origem para um endereço de resource gateway e encaminha um pacote comum para a VPC do provedor. O cliente só precisa produzir o encapsulamento.

### Roteamento em network namespace do Linux
O Linux pode produzir o encapsulamento no kernel. O passo a passo cria um dispositivo com `ip link add ... type geneve id 0 device` dentro de um network namespace próprio e define uma rota padrão por ele. Apenas as faixas do provedor entram no túnel, então o restante do tráfego da instância não é afetado. Esse isolamento é o que torna a abordagem viável em um host compartilhado.

### Security groups no resource gateway
Do lado do provedor, os recursos veem o resource gateway como o cliente, e não a instância por trás do endpoint. O post recomenda escrever regras de security group que façam referência ao security group ou à sub-rede do gateway, e não a um único endereço. Uma regra sobre um endereço deixa de funcionar assim que o gateway muda.

> ❌ **Anti-padrão — permitir um único endereço de gateway:** uma regra de security group que nomeia um IP específico do resource gateway. **Por que as pessoas fazem isso:** parece a regra mais precisa. **Consequência:** a regra falha quando o endereço do gateway muda, e não descreve a origem real. **Correção:** faça referência ao security group ou à sub-rede do gateway, como o post recomenda.

## Restrições e limitações
A documentação lista vários limites rígidos. O tráfego interno deve ser TCP, com UDP permitido apenas para DNS, então ICMP e ping falham. Fragmentos de IP são descartados. Nomes DNS privados não são suportados em endpoints de túnel. As sub-redes do endpoint devem estar em Availability Zones que se sobreponham às do resource gateway, senão a associação aparece como inacessível. A configuração de DNS do resource gateway não pode ser alterada depois da criação.

### Logs de acesso e atribuição
Os logs de acesso identificam o endpoint do consumidor, mas não têm um campo para o endereço de origem interno. Um provedor pode atribuir o tráfego a um fornecedor, mas não à carga de trabalho por trás do endpoint desse fornecedor. Qualquer auditoria que exija atribuição por carga de trabalho precisa ser construída em outro lugar.

### O alerta sobre 0.0.0.0/0
Compartilhar 0.0.0.0/0 concede acesso a tudo o que a VPC do provedor consegue rotear, inclusive redes atrás de VPC peering, Direct Connect ou VPN Site-to-Site. O post recomenda compartilhar as faixas mais restritas que funcionem.

> ❌ **Anti-padrão — compartilhar 0.0.0.0/0:** um compartilhamento que cobre todo o espaço de endereços. **Por que as pessoas fazem isso:** evita descobrir quais faixas o fornecedor precisa. **Consequência:** o consumidor alcança tudo o que o provedor roteia, inclusive redes atrás de peering, Direct Connect e VPN. **Correção:** compartilhe as faixas mais restritas que funcionem e revise-as quando as necessidades do fornecedor mudarem.

## Testes de campo com CIDRs sobrepostos
Ryo Suzuki, da consultoria japonesa Classmethod, testou o desenho, e as afirmações do artigo se confirmaram na prática.

### VPCs sobrepostas
Ele conectou duas VPCs que usam ambas 10.0.0.0/16 e conseguiu HTTP. Três abordagens fizeram funcionar: executar um relay GENEVE como root no cliente, rotear um /32 por host de destino ou colocar um HAProxy ou uma instância NAT na frente. Cada uma acrescenta um salto ou um ponto único de falha. Sua conclusão foi que "é necessário gerenciar rotas de acordo com o número de destinos".

> ❌ **Anti-padrão — roteamento por destino ou relay:** uma rota /32 para cada host de destino, um relay GENEVE como root no cliente, ou um HAProxy ou instância NAT na frente. **Por que as pessoas fazem isso:** são as únicas formas de fazer redes sobrepostas se comunicarem no teste. **Consequência:** cada uma acrescenta um salto ou um ponto único de falha, e o número de rotas cresce com o número de destinos. **Correção:** o artigo não oferece uma correção limpa para faixas sobrepostas; a conclusão do autor é que o esforço de roteamento escala com os destinos, então planeje para isso.

### Compartilhar 0.0.0.0/0 na prática
Em um segundo experimento, Suzuki compartilhou 0.0.0.0/0 a partir de uma VPC com um gateway NAT e alcançou a internet a partir de uma VPC sem gateway algum. Isso confirmou o alerta da AWS vindo do outro lado. Ele considerou a solução viável como um caminho temporário e "difícil de usar em uma configuração de produção permanente".

## Preços, disponibilidade e suporte a Terraform
O preço é uma cobrança por hora por endpoint de túnel, mais uma cobrança por GB de dados processados, segundo a precificação do PrivateLink. O lado do provedor é cobrado por GB através do resource gateway, segundo a precificação do VPC Lattice. O recurso está disponível em 28 regiões.

### Suporte a Terraform
O suporte ao Terraform veio da comunidade. Uma solicitação de contribuidor foi aberta dois dias após o lançamento, e outro contribuidor a implementou. Ela foi incluída na versão 6.71.0 do provider AWS, como um tipo de endpoint Tunnel em `aws_vpc_endpoint` e um bloco `cidr_resource` na configuração de recurso do Lattice.

## 🔑 Principais conclusões
- Uma configuração de recurso CIDR dá ao consumidor alcance a uma faixa inteira, inclusive a recursos criados depois do compartilhamento.
- Endpoints de túnel funcionam em um único sentido, e as conexões só começam a partir da VPC consumidora.
- Blocos CIDR sobrepostos são suportados, mas os testes de campo mostram que o esforço de roteamento cresce com o número de destinos.
- Compartilhar 0.0.0.0/0 expõe tudo o que a VPC do provedor consegue rotear, então compartilhe as faixas mais restritas.
- Os logs de acesso identificam o endpoint do consumidor, mas não a origem interna, então a atribuição por carga de trabalho exige outra fonte.
- O consumidor deve produzir GENEVE na UDP 6081, na mesma Availability Zone do endpoint, e o tráfego interno deve ser TCP, ou UDP apenas para DNS.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. O acesso de fornecedores a serviços internos de IA, como endpoints de modelos ou pipelines de dados operados por um provedor externo, se encaixa bem no modelo de perímetro, e limita o que um fornecedor pode iniciar. A lacuna de atribuição é o problema mais difícil para uma revisão de governança: se os logs de acesso não conseguem nomear a carga de trabalho por trás do endpoint de um fornecedor, como você provaria, em uma auditoria, qual carga de trabalho interna alcançou qual recurso?

## 📚 Plano de leitura
### A partir do artigo
- Nenhum link sobreviveu à extração. O artigo cita o blog AWS Networking & Content Delivery, a thread do r/aws sobre o lançamento de setembro, a RFC 8926 e as páginas de preços do PrivateLink e do VPC Lattice, mas não dá URLs para citar.
### Para aprofundar
- RFC 8926 (Geneve: Generic Network Virtualization Encapsulation) — a especificação de encapsulamento por trás do encapsulamento do lado do consumidor.
- Documentação do AWS PrivateLink e do VPC Lattice sobre configurações de recurso — a documentação do produto para as restrições deste artigo.
- Notas de versão do provider Terraform da AWS 6.71.0 — os nomes de recursos e argumentos para automatizar endpoints de túnel.
