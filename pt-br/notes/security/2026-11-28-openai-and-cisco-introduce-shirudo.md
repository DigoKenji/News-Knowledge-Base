---
type: Article Summary
title: OpenAI e Cisco apresentam o Shirudo
description: O Shirudo, de código aberto da OpenAI e da Cisco, usa escudos eBPF para bloquear exploits em sistemas Linux em execução enquanto os patches são preparados.
resource: https://openai.com/index/introducing-shirudo
tags:
- security
- security-operations
- vulnerability-management
- least-privilege
- remote-attestation
- security-controls
- incident-response
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-28T06:47:00-03:00'
sources:
- resource: https://openai.com/index/introducing-shirudo
  title: OpenAI and Cisco introduce Shirudo
  author: OpenAI
status: stable
source_url: https://openai.com/index/introducing-shirudo
source: OpenAI News
source_type: blog
published: '2026-11-23'
domain: security
subdomain: security-operations
concepts:
- vulnerability-management
- least-privilege
- remote-attestation
- security-controls
- incident-response
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/426baf74172d810386bcd52eaae20e4a
lang: pt-br
translation_of: /notes/security/2026-11-28-openai-and-cisco-introduce-shirudo.md
---

## 🎯 Tese central
O Shirudo é um runtime de código aberto da OpenAI e da Cisco que permite aos operadores aplicar um "escudo" eBPF testado e assinado a um sistema Linux em execução, bloqueando o caminho de código de um exploit enquanto o patch definitivo ainda está sendo preparado.

## 🧭 Por que o autor escreveu
Testar e implantar um patch pode levar dias ou semanas, e o paliativo óbvio, bloquear uma operação, pode tirar um serviço do ar. O artigo defende que os operadores precisam de uma salvaguarda direcionada e aprovada, que possa ser implantada em um serviço em execução. Se isso funcionar, as equipes podem fechar a janela de exposição sem reinícios ou indisponibilidade.

## Apresentando o Shirudo
O artigo anuncia o Shirudo como um runtime de código aberto que ajuda as equipes a proteger sistemas Linux enquanto implantam patches. Em um teste controlado, um escudo gerado com o Codex bloqueou uma tentativa de exploit contra um serviço Elasticsearch em execução, sem reinicialização. A OpenAI e a Cisco convidam desenvolvedores, pesquisadores de segurança e operadores de infraestrutura a ajudar no desenvolvimento, combinando a pesquisa de IA de fronteira, segurança e infraestrutura da OpenAI com a experiência da Cisco em infraestrutura e segurança.

### Shirudo
O Shirudo é o nome do runtime, e significa "escudo" em japonês. Seu propósito é proteger sistemas Linux em execução, para que a proteção possa ser aplicada sem esperar por um patch ou reinício. O artigo apresenta a ferramenta como um primeiro passo no desenvolvimento público, não como um produto acabado, e pede contribuições. O mecanismo que faz a proteção é o escudo, que as seções seguintes explicam.

## A lacuna do patch e a ideia do escudo
Testar e implantar um patch ainda pode levar dias ou semanas. Proteger um serviço nessa janela é difícil, porque um exploit e uma requisição legítima podem depender da mesma operação do Linux. Bloquear essa operação por completo poderia tirar o serviço do ar. O Shirudo ajuda as equipes de segurança a testar se uma salvaguarda temporária, chamada escudo, bloqueia o comportamento que expõe a vulnerabilidade e deixa passar as requisições legítimas. Depois que um operador aprova o escudo, ele pode ser implantado enquanto o serviço continua em execução.

### A lacuna do patch
A lacuna do patch é o período entre descobrir que uma vulnerabilidade existe e ter uma correção testada rodando em todos os lugares. O artigo coloca apenas o teste e a implantação em dias ou semanas, e essa é a janela que o Shirudo mira. Um escudo é uma mitigação provisória, não um substituto do patch, e o artigo o descreve como proteção "enquanto se preparam patches permanentes".
> 🔎 Contexto adicionado: Gestão de vulnerabilidades é o processo contínuo de encontrar, priorizar, mitigar e verificar fraquezas. Uma mitigação provisória como um escudo é uma etapa desse processo, embora o artigo não use o termo.

### Escudo
Um escudo é uma salvaguarda temporária voltada ao comportamento específico que expõe uma vulnerabilidade, e não à operação inteira do Linux. O teste de um escudo é verificar se ele bloqueia o exploit e deixa passar as requisições legítimas. Um operador aprova o escudo antes de ele ser implantado, e então o escudo pode ser aplicado enquanto o serviço segue em execução. O artigo descreve os escudos como uma forma de avaliar e implantar proteções temporárias antes que os patches permanentes estejam prontos.
> 🔎 Contexto adicionado: Um controle compensatório é uma salvaguarda usada quando a correção principal ainda não está em vigor, o que descreve um escudo. O artigo não usa o termo.

> ❌ **Anti-padrão — bloqueio genérico da operação:** bloquear a operação do Linux que o exploit usa, para todos os chamadores. **Por que as pessoas fazem isso:** o artigo não dá motivos; bloquear tudo é a resposta mais simples quando não existe patch. **Consequência:** requisições legítimas falham, e o artigo alerta que o serviço poderia sair do ar. **Correção:** um escudo que mire o caminho de código vulnerável e deixe passar as requisições legítimas, testado antes de ser aplicado.
> 🔎 Contexto adicionado: A parte "por que as pessoas fazem isso" é a minha leitura, não a do artigo.

## Como funcionam os escudos
O Shirudo usa eBPF para observar atividades suspeitas e aplicar proteções direcionadas a sistemas em execução. O artigo organiza seu design em torno de quatro capacidades principais, detalhadas abaixo.

### eBPF
eBPF é uma tecnologia do Linux que permite executar pequenos programas em pontos específicos do sistema operacional. Esses programas podem observar a atividade nesses pontos e aplicar regras direcionadas ali, o que torna o escudo possível. Como os programas atuam em um sistema em execução, uma proteção pode ser adicionada sem reiniciar o sistema. O artigo não dá detalhes de implementação além disso, então o mecanismo é descrito no nível que o autor fornece.

### Proteção em tempo real
O artigo descreve escudos eBPF que bloqueiam caminhos de código vulneráveis sem reiniciar o sistema. As equipes de segurança podem monitorar como um escudo proposto afetaria as aplicações antes de aplicá-lo. A ordem importa: observar primeiro, depois aplicar. Essa ordem é o que permite introduzir um escudo em um serviço ativo sem antes quebrar o tráfego legítimo.

### Defesa em velocidade de máquina
A OpenAI e a Cisco dizem que estão projetando o Shirudo para oferecer controles programáveis aos agentes. Os agentes investigariam vulnerabilidades, desenvolveriam escudos direcionados e validariam sua eficácia contra exploits e cargas de trabalho reais. O artigo deixa explícito que as equipes de segurança controlam o que é implantado. A expressão "are designing" (estão projetando) importa aqui: trata-se de uma meta de design do artigo, não de uma capacidade descrita como concluída.

### Implantação confiável
Os escudos são assinados criptograficamente e verificados antes de serem carregados. O artigo cita duas verificações: o verificador eBPF do Linux checa os programas eBPF contra suas regras de segurança, e os controles de segurança do kernel restringem alterações não autorizadas. Um escudo é, portanto, aceito porque passa nessas verificações. O artigo apresenta isso também como parte do design.

### Verificação com base em hardware
O Shirudo usa um Trusted Platform Module (TPM) para dar suporte a um registro verificável das proteções carregadas ou removidas. Os operadores podem usar essa evidência para checar se seus sistemas atendem aos requisitos de segurança. O registro é uma evidência que o operador pode inspecionar, e não apenas um log que o sistema escreve sobre si mesmo.
> 🔎 Contexto adicionado: Isso é uma forma de atestação remota, em que uma máquina comprova, com evidências assinadas pelo hardware, o que está executando. O artigo não usa o termo.

## O exemplo do Java4Shell
O artigo traz um único exemplo prático. Usando o Codex, a equipe gerou um escudo que adicionou uma verificação de entrada ao decodificador Base64 do Java e o anexou diretamente a um processo Elasticsearch em execução. Quando chegou uma tentativa de exploit, o escudo rejeitou a entrada maliciosa, então o ataque foi bloqueado enquanto o Elasticsearch continuava rodando. A apresentação técnica do Shirudo na Linux Plumbers Conference mostra como aplicar um escudo e verificá-lo no host alvo.

### Escudo do Java4Shell
O Java4Shell é a vulnerabilidade usada para ilustrar como um escudo direcionado protege uma aplicação em execução enquanto um patch permanente é preparado. O escudo é estreito: uma verificação de entrada colocada no caminho que decodifica dados Base64, de modo que só a entrada maliciosa é rejeitada. Anexá-lo a um processo em execução significou que a aplicação não precisou ser recompilada nem reiniciada. O artigo relata o resultado de um teste controlado e não dá mais detalhes sobre o exploit ou o cronograma do patch.
> 🔎 Contexto adicionado: O padrão aqui é gerar uma verificação direcionada, anexá-la ao processo ativo e então verificá-la contra o exploit. O artigo não traz tempos nem taxas de sucesso.

## A direção de longo prazo
O artigo chama o Shirudo de "um primeiro passo" rumo a uma infraestrutura que possa se adaptar a ameaças emergentes mantendo a confiabilidade. Os objetivos mais amplos são endurecer o kernel e os serviços centrais, limitar as permissões das cargas de trabalho e tornar atualizações e recuperações mais seguras. O objetivo de longo prazo é que defensores e agentes de IA identifiquem rapidamente os sistemas afetados, validem proteções e implantem escudos eBPF que bloqueiem exploits sem perturbar as cargas de trabalho.

### Menor privilégio para cargas de trabalho
A meta do artigo é que as aplicações rodem apenas com as permissões de que precisam, governadas por políticas geradas e mantidas automaticamente. Trata-se do princípio do menor privilégio aplicado às cargas de trabalho em execução: uma aplicação comprometida pode causar menos danos quando suas permissões são estreitas. O artigo apresenta a geração automática de políticas como uma meta, não como um recurso atual.

### Contenção e restauração para um estado conhecido como bom
O artigo lista como objetivos detectar e conter comprometimentos, impedir que atacantes se movam entre sistemas e restaurar serviços a um estado verificado e conhecido como bom. Acrescenta que a atestação com base em hardware e os controles de rede independentes dão garantias extras, mesmo que sistemas individuais sejam comprometidos.
> 🔎 Contexto adicionado: Esses objetivos correspondem às fases de contenção e recuperação da resposta a incidentes. O artigo não nomeia esse processo.

## Código aberto e como participar
A OpenAI e a Cisco estão abrindo o código do Shirudo para que desenvolvedores, pesquisadores de segurança e operadores de infraestrutura possam inspecioná-lo, testar proteções e contribuir. O artigo recebe contribuições para o runtime, para novos escudos e para melhorias em Linux e eBPF. Ele lista três formas de começar: seguir o quickstart em VM para assinar, carregar e verificar uma proteção de teste; fazer o laboratório autoguiado da Isovalent; ou se inscrever no webinar de 15 de dezembro sobre o deep dive técnico.

### Licenciamento
O Shirudo é licenciado sob a BSD 2-Clause, e seus programas BPF têm dupla licença, BSD e GPL. O artigo menciona as duas licenças, mas não explica o que a dupla licença significa para qualquer uso específico. Uma equipe que adote ou adapte o Shirudo deve verificar a licença dos programas BPF separadamente da licença do runtime.

### Escudos testados com divulgações
O objetivo declarado do artigo é disponibilizar escudos testados junto com as divulgações de vulnerabilidades. Isso daria aos operadores algo para avaliar e implantar enquanto os patches permanentes são preparados, em vez de esperar pela correção. O artigo não descreve como esses escudos serão publicados nem verificados de forma independente.

## 🧠 Modelos mentais
### Escudo enquanto você corrige
**O modelo:** Um escudo é uma salvaguarda temporária e direcionada que cobre uma fraqueza conhecida até que a correção definitiva seja implantada.
**Como aplicar:** Restrinja-o ao comportamento explorável, e não à operação inteira, e planeje removê-lo quando o patch estiver no lugar.

### Observar antes de aplicar
**O modelo:** Uma proteção deve ser observada com cargas de trabalho reais antes de ter permissão para bloquear qualquer coisa.
**Como aplicar:** Execute o escudo proposto em modo de monitoramento contra o tráfego ativo, verifique seu efeito nas aplicações e só então aprove a aplicação.

### Evidência em vez de afirmação
**O modelo:** O artigo trata um escudo como confiável porque ele é assinado, verificado pelo kernel e registrado pelo hardware, de modo que um operador pode verificá-lo em vez de confiar nele.
**Como aplicar:** Exija um escudo assinado e um registro verificável do que foi carregado ou removido antes de aceitar uma proteção em um host de produção.

## 🔑 Principais conclusões
- Testar e implantar patches pode levar dias ou semanas, e o artigo mira essa janela com proteções temporárias e direcionadas.
- Um escudo deve bloquear o comportamento explorável deixando passar as requisições legítimas; bloquear toda a operação do Linux pode tirar o serviço do ar.
- Escudos eBPF podem ser aplicados a sistemas em execução sem reinício, e as equipes de segurança podem monitorar o efeito de um escudo antes de aplicá-lo.
- Os escudos são assinados criptograficamente e verificados pelo verificador eBPF e pelos controles do kernel antes de carregar, e um TPM dá suporte a um registro verificável de cargas e remoções.
- No teste com Java4Shell, uma verificação de entrada gerada pelo Codex no decodificador Base64 do Java rejeitou a entrada maliciosa enquanto o Elasticsearch continuava rodando.
- O artigo descreve "defesa em velocidade de máquina", em que agentes criam e validam escudos, como uma meta de design, e não como uma capacidade concluída.
- O objetivo declarado é publicar escudos testados junto com as divulgações de vulnerabilidades, para que os operadores possam proteger os serviços enquanto os patches são preparados.

## 🏛️ Para o Kenji
*Interpretação, não do autor:* Para um arquiteto, o padrão útil é um caminho de mitigação aprovado e com prazo definido entre a divulgação e o patch, com evidência do que foi carregado e removido. Isso se aplica a como você desenha runbooks de gestão de vulnerabilidades e a decidir quando um serviço em execução pode ser protegido sem indisponibilidade. A pergunta que levanta: no seu parque tecnológico, um escudo deve ser aprovado pela equipe de segurança ou pelo dono do serviço?

## 📚 Plano de leitura
### A partir do artigo
- O texto extraído não tem alvos de links, então os recursos citados pelo artigo são listados abaixo sem URLs.

### Para ir além
- Código-fonte do Shirudo no GitHub (citado pelo artigo) — inspecionar o runtime e os programas BPF.
- Documentação do projeto Shirudo (citada pelo artigo) — a referência sobre o conteúdo e o uso do projeto.
- Quickstart em VM do Shirudo (citado pelo artigo) — assinar, carregar e verificar uma proteção de teste.
- Laboratório autoguiado da Isovalent (citado pelo artigo) — prática guiada com o Shirudo.
- Apresentação técnica do Shirudo, Linux Plumbers Conference (citada pelo artigo) — aplicar e verificar um escudo em um host alvo.
- Deep dive técnico do Shirudo, webinar de 15 de dezembro (citado pelo artigo) — acompanhamento ao vivo do design.
- Verificador eBPF — as regras de segurança que liberam cada escudo antes de ele ser carregado.
- Trusted Platform Module (TPM) — a base de hardware do registro verificável de proteções.

## Conceitos
- [Gestão de vulnerabilidades](/pt-br/concepts/security/vulnerability-management.md)
- [Menor privilégio e defesa em profundidade](/pt-br/concepts/security/least-privilege.md)
- [Atestação remota](/pt-br/concepts/security/remote-attestation.md)
- [Controles de segurança](/pt-br/concepts/security/security-controls.md)
- [Resposta a incidentes](/pt-br/concepts/security/incident-response.md)
