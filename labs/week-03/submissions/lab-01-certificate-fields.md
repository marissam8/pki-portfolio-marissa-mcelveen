# Lab 01 — Inspect X.509 Certificate Fields

## Overview
Inspect the structure of a X.509 Certificate. Observe and identify the core fields that authenticate a certificate's identity. The componenets 
of the certificate's anatomy is important to investigate to prevent impersonation and enforce authentication.
---

## Environment
- Operating System: macOS
- Terminal Used: Integrated terminal in Visual Studio Code (zsh shell)
- OpenSSL Version (if applicable): LibreSSL 3.3.6

---

## Certificate Fields

| Field                | Value from your output |
|----------------------|------------------------|
| Version              |                        |
| Serial Number        |                        |
| Signature Algorithm  |                        |
| Issuer               |                        |
| Subject              |                        |
| Not Before           |                        |
| Not After            |                        |
| Public Key Algorithm |                        |

## Observations

1. Who issued the certificate: Google Trust Services
2. What domain or organization does it represent: *.google.com
3. When does it expire: Not Before: Feb 23 18:19:44 2026 GMT
                        Not After : May 18 18:19:43 2026 GMT
4. What public key algorithm is used: rsaEncryption
5. The Issuer field matters in a PKI system because PKI relies on trusted certificate authorities to establish trust. Systems trust certificates when
they trust the issuing authority.
