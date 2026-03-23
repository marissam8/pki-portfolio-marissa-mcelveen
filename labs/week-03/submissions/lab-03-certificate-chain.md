

# Lab 03 — Verify a Certificate Chain

## Overview
The purpose of this lab is to expand on my knowledge of the established trust of certifcate chains in the PKI infrastructure.
In this lab, I will observe and identify the root, intermediate and leaf certs. Also I will inspect the trust hierarchy, understand how browsers validate certificate trust, and using OpenSSL verify a certificate chain.

---

## Environment
- Operating System: macOS
- Terminal Used: Integrated terminal in Visual Studio Code (zsh shell)
- OpenSSL Version (if applicable): LibreSSL 3.3.6

---

## Chain Verification Result
Paste the output of your `openssl verify` command:
    server.pem: OK

---

## Certificate Roles

| Certificate  | Role                        | Key Indicator                    |
|--------------|-----------------------------|----------------------------------|
| root.pem     |                             |                                  |
| intermediate.pem |                         |                                  |
| server.pem   |                             |                                  |

---

## Observations

1. The chain verified successfully What did the output said "server.pem: OK".
2. I identified the root CA by the order it was presented in the output(last). Also the root is labled with the number "2".
3. I identify the intermediate CA by the order it was presented in the output (in the middle). Also the root is labled with the number "1".
4. The field that confirms whether a certificate can issue other certificates is the field extension's basic constraints.
5. Removing the intermediate certificate breaks the chain because it [intermediate certificate] acts as a trust bridge between the root and the leaf. Without the intermediate certificate, trust cannot be established. As stated previously in labs week 1, trust flows down, then validation walks up.