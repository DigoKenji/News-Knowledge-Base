---
type: Concept
title: Criptografia
description: Transformar dados com uma chave para que só quem tem a chave certa consiga lê-los - a criptografia simétrica usa uma chave compartilhada, a assimétrica (de chave pública) usa um par de chaves - usada para proteger dados em repouso e em trânsito.
tags:
- security
- core
domain: security
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2020). SP 800-175B Rev. 1 - Guideline for Using Cryptographic Standards in the Federal Government
  resource: 'https://csrc.nist.gov/pubs/sp/800/175/b/r1/final'
  kind: standard
- title: NIST (2001, updated 2023). FIPS 197 - Advanced Encryption Standard (AES)
  resource: 'https://csrc.nist.gov/pubs/fips/197/final'
  kind: standard
- title: Rescorla, E. (2018). RFC 8446 - The Transport Layer Security (TLS) Protocol Version 1.3. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc8446'
  kind: standard
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/encryption.md
---

# Criptografia

**Definição.** A criptografia converte texto claro em texto cifrado com um algoritmo e uma chave; a decifração faz o inverso. Algoritmos simétricos como o AES (FIPS 197) são rápidos e usam a mesma chave secreta dos dois lados. Algoritmos assimétricos (RSA, curvas elípticas), introduzidos por Diffie e Hellman em 1976, usam uma chave pública que qualquer um pode conhecer e uma chave privada mantida em segredo, o que resolve a troca de chaves e permite assinaturas digitais.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Sistemas reais combinam os dois: o TLS usa criptografia de chave pública para autenticar o servidor e combinar uma chave de sessão, e depois criptografia simétrica para os dados (em trânsito). Criptografia de disco e de banco protege dados em repouso. A parte difícil costuma ser a gestão de chaves (geração, armazenamento, rotação, acesso), não o algoritmo. Hash é relacionado mas diferente: é de mão única e não tem chave para revertê-lo.

## Exemplo

Quando você abre https://github.com, o TLS 1.3 verifica o certificado do GitHub com criptografia de chave pública e depois cifra a sessão com AES ou ChaCha20.

## Conceitos mais específicos

- [Atestação remota](/pt-br/concepts/security/remote-attestation.md)
- [Arquitetura zero-trust](/pt-br/concepts/security/zero-trust-architecture.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. NIST (2020). *SP 800-175B Rev. 1 - Guideline for Using Cryptographic Standards in the Federal Government*. <https://csrc.nist.gov/pubs/sp/800/175/b/r1/final> — Algoritmos simétricos e assimétricos, gestão de chaves, usos aprovados.
2. NIST (2001, updated 2023). *FIPS 197 - Advanced Encryption Standard (AES)*. <https://csrc.nist.gov/pubs/fips/197/final> — A cifra simétrica padrão.
3. Rescorla, E. (2018). *RFC 8446 - The Transport Layer Security (TLS) Protocol Version 1.3*. IETF. <https://www.rfc-editor.org/rfc/rfc8446> — Como criptografia de chave pública e simétrica se combinam para proteger dados em trânsito.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
