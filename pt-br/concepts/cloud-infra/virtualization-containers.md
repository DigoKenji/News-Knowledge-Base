---
type: Concept
title: Virtualização e contêineres
description: A virtualização executa várias máquinas virtuais isoladas, cada uma com o próprio sistema operacional, em um host físico através de um hipervisor; os contêineres isolam aplicações que compartilham o kernel do sistema operacional do host, tornando-os mais leves e rápidos de iniciar.
tags:
- cloud-infra
- containers
- core
domain: cloud-infra
subdomain: containers
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Popek, G. J. and Goldberg, R. P. (1974). Formal Requirements for Virtualizable Third Generation Architectures. Communications of the ACM 17(7)
  resource: 'https://doi.org/10.1145/361011.361073'
  kind: paper
- title: Open Container Initiative. OCI Runtime and Image Specifications
  resource: 'https://opencontainers.org/'
  kind: standard
- title: Docker. What is a container?
  resource: 'https://www.docker.com/resources/what-container/'
  kind: docs
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§12.8 Virtualization Security, pp. 432-436'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 3 Cloud Asset Management and Protection, pp. 29-48'
lang: pt-br
translation_of: /concepts/cloud-infra/virtualization-containers.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Virtualização e contêineres

**Definição.** Popek e Goldberg formalizaram os requisitos de um monitor de máquina virtual (hipervisor): equivalência, controle de recursos e eficiência. Um contêiner empacota uma aplicação com suas dependências e a executa como processo isolado usando recursos do kernel (namespaces, cgroups no Linux); a Open Container Initiative (OCI) padroniza os formatos de imagem e runtime para que a mesma imagem rode em qualquer lugar. Contêineres isolam menos que VMs, então plataformas multi-inquilino às vezes rodam contêineres dentro de VMs leves.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `containers`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A virtualização é o que torna o IaaS possível (agrupamento de recursos); os contêineres são a unidade de implantação de microsserviços, CI/CD e Kubernetes. Contêineres resolvem o "funciona na minha máquina" ao enviar o ambiente junto com o código. As imagens ainda precisam ser verificadas e assinadas, porque são um risco de cadeia de suprimentos.

## Exemplo

Um servidor físico roda 20 máquinas virtuais de clientes diferentes. Dentro de uma VM, uma equipe roda 15 contêineres construídos a partir de imagens OCI - API, worker, cache - cada um iniciando em menos de um segundo e idêntico em desenvolvimento e produção.

## Conceitos mais específicos

- [Orquestração de contêineres (Kubernetes)](/pt-br/concepts/cloud-infra/container-orchestration.md)
- [Serverless (sem servidor)](/pt-br/concepts/cloud-infra/serverless.md)
- [Atestação remota](/pt-br/concepts/security/remote-attestation.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §12.8 Virtualization Security, pp. 432-436: Tipos de hipervisor e questões de segurança da virtualização.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 3 Cloud Asset Management and Protection, pp. 29-48: Inventário e proteção de VMs, contêineres e outros ativos de nuvem.

## Fontes

1. Popek, G. J. and Goldberg, R. P. (1974). *Formal Requirements for Virtualizable Third Generation Architectures*. Communications of the ACM 17(7). <https://doi.org/10.1145/361011.361073> — Requisitos formais para monitores de máquinas virtuais.
2. Open Container Initiative. *OCI Runtime and Image Specifications*. <https://opencontainers.org/> — Padrões abertos para imagens e runtimes de contêineres.
3. Docker. *What is a container?*. <https://www.docker.com/resources/what-container/> — Contêineres versus máquinas virtuais na prática.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §12.8 Virtualization Security, pp. 432-436. Tipos de hipervisor e questões de segurança da virtualização.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 3 Cloud Asset Management and Protection, pp. 29-48. Inventário e proteção de VMs, contêineres e outros ativos de nuvem.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
