---
type: Concept
title: Criptografia
description: Transformar dados com uma chave para que só quem tem a chave certa consiga lê-los - a criptografia simétrica usa uma chave compartilhada, a assimétrica (de chave pública) usa um par de chaves - usada para proteger dados em repouso e em trânsito.
tags:
- security
- security-foundations
- core
domain: security
subdomain: security-foundations
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
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 2 Cryptographic Tools, pp. 40-71; ch. 20 Symmetric Encryption, p. 637'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 2 Data Asset Management and Protection, pp. 13-28'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/encryption.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Criptografia

**Definição.** A criptografia converte texto claro em texto cifrado com um algoritmo e uma chave; a decifração faz o inverso. Algoritmos simétricos como o AES (FIPS 197) são rápidos e usam a mesma chave secreta dos dois lados. Algoritmos assimétricos (RSA, curvas elípticas), introduzidos por Diffie e Hellman em 1976, usam uma chave pública que qualquer um pode conhecer e uma chave privada mantida em segredo, o que resolve a troca de chaves e permite assinaturas digitais.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `security-foundations`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Sistemas reais combinam os dois: o TLS usa criptografia de chave pública para autenticar o servidor e combinar uma chave de sessão, e depois criptografia simétrica para os dados (em trânsito). Criptografia de disco e de banco protege dados em repouso. A parte difícil costuma ser a gestão de chaves (geração, armazenamento, rotação, acesso), não o algoritmo. Hash é relacionado mas diferente: é de mão única e não tem chave para revertê-lo.

## Exemplo

Quando você abre https://github.com, o TLS 1.3 verifica o certificado do GitHub com criptografia de chave pública e depois cifra a sessão com AES ou ChaCha20.

## Conceitos mais específicos

- [Atestação remota](/pt-br/concepts/security/remote-attestation.md)
- [Arquitetura zero-trust](/pt-br/concepts/security/zero-trust-architecture.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, ch. 2 Cryptographic Tools, pp. 40-71; ch. 20 Symmetric Encryption, p. 637: Criptografia simétrica e de chave pública, hashes, MACs, assinaturas e gestão de chaves.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 2 Data Asset Management and Protection, pp. 13-28: Criptografia em repouso e em trânsito e gestão de chaves na nuvem.

## Fontes

1. NIST (2020). *SP 800-175B Rev. 1 - Guideline for Using Cryptographic Standards in the Federal Government*. <https://csrc.nist.gov/pubs/sp/800/175/b/r1/final> — Algoritmos simétricos e assimétricos, gestão de chaves, usos aprovados.
2. NIST (2001, updated 2023). *FIPS 197 - Advanced Encryption Standard (AES)*. <https://csrc.nist.gov/pubs/fips/197/final> — A cifra simétrica padrão.
3. Rescorla, E. (2018). *RFC 8446 - The Transport Layer Security (TLS) Protocol Version 1.3*. IETF. <https://www.rfc-editor.org/rfc/rfc8446> — Como criptografia de chave pública e simétrica se combinam para proteger dados em trânsito.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 2 Cryptographic Tools, pp. 40-71; ch. 20 Symmetric Encryption, p. 637. Criptografia simétrica e de chave pública, hashes, MACs, assinaturas e gestão de chaves.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 2 Data Asset Management and Protection, pp. 13-28. Criptografia em repouso e em trânsito e gestão de chaves na nuvem.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
