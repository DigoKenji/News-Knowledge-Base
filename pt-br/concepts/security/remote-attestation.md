---
type: Concept
title: Atestação remota
description: Um protocolo em que uma máquina prova a um verificador remoto, com evidências assinadas por hardware que ela não consegue falsificar, exatamente qual software e configuração está rodando, para que o verificador decida se confia nela antes de enviar segredos ou trabalho.
tags:
- security
- appsec
domain: security
subdomain: appsec
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Birkholz, H. et al. (2023). RFC 9334 - Remote ATtestation procedureS (RATS) Architecture. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc9334'
  kind: standard
- title: Coker, G. et al. (2011). Principles of Remote Attestation. International Journal of Information Security 10(2)
  resource: 'https://doi.org/10.1007/s10207-011-0124-7'
  kind: paper
- title: AWS. Cryptographic attestation (AWS Nitro Enclaves User Guide)
  resource: 'https://docs.aws.amazon.com/enclaves/latest/user/set-up-attestation.html'
  kind: docs
lang: pt-br
translation_of: /concepts/security/remote-attestation.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Atestação remota

**Definição.** Na atestação remota, um atestador produz evidências sobre seu estado (medições de firmware, sistema operacional, código da aplicação), assinadas por uma raiz de confiança em hardware como um TPM ou um módulo de segurança da CPU. Um verificador checa a assinatura e compara as medições com valores de referência conhecidos e bons, e então uma parte confiante age com base no resultado. A arquitetura RATS da IETF (RFC 9334) padroniza esses papéis.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `appsec`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Durante o boot, cada etapa mede (calcula o hash de) a próxima antes de executá-la e registra o hash em registradores protegidos, construindo uma cadeia a partir do hardware. Sob pedido, o hardware assina uma cotação dessas medições junto com um nonce novo enviado pelo verificador, o que impede replay. A computação confidencial estende isso a enclaves e VMs confidenciais (Intel SGX/TDX, AMD SEV-SNP, AWS Nitro Enclaves): um serviço de gestão de chaves libera uma chave de decifração só para um enclave cujo hash de código atestado bate com a política. A atestação prova o que está rodando, não que o código está livre de bugs.

## Por que importa e trade-offs

Permite confiar em máquinas remotas que você não controla, por exemplo processar dados privados na nuvem sem que nem o operador consiga vê-los (o desenho de acesso zero do operador na nota de 2026-11-21). Custos: dependência de hardware, gestão complexa de valores de referência a cada atualização de software e confiança nas chaves e na implementação do fabricante do hardware, que já tiveram vulnerabilidades.

## Exemplo

Antes de um celular enviar a gravação de voz de um usuário a um servidor na nuvem para processamento de IA, ele checa a atestação do servidor e só envia os dados se o servidor estiver rodando exatamente a versão auditada.

## Fontes

1. Birkholz, H. et al. (2023). *RFC 9334 - Remote ATtestation procedureS (RATS) Architecture*. IETF. <https://www.rfc-editor.org/rfc/rfc9334> — Papéis (atestador, verificador, parte confiante), evidências, valores de referência.
2. Coker, G. et al. (2011). *Principles of Remote Attestation*. International Journal of Information Security 10(2). <https://doi.org/10.1007/s10207-011-0124-7> — Princípios fundamentais - evidência recente, abrangente, divulgação restrita, mecanismo confiável.
3. AWS. *Cryptographic attestation* (AWS Nitro Enclaves User Guide). <https://docs.aws.amazon.com/enclaves/latest/user/set-up-attestation.html> — Liberação de chaves condicionada à atestação num serviço de computação confidencial em produção.

## Relações
- (nenhuma ainda)

## Notas
- [Como a Bee implementou Zero Operator Access para IA pessoal na AWS](/pt-br/notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md)
