

# Lab 03 — Verify a Certificate Chain

## Overview
The purpose of this lab is to expand on my knowledge of the established trust of certifcate chains in the PKI infrastructure.
In this lab, I will observe and identify the root, intermediate and leaf certs. Also I will inspect the trust hierarchy, understand how browsers validate certificate trust, and using OpenSSL verify a certificate chain.


## Environment
- Operating System: macOS
- Terminal Used: Integrated terminal in Visual Studio Code (zsh shell)
- OpenSSL Version (if applicable): LibreSSL 3.3.6



## Steps Performed
Summarize the key steps you performed to complete the lab.
1. Performed the command to show the entire cert chain to identify and observation.
2. Copy and save each certificate separately in its own .pem file.
3. Ran an inspection of each cert
4. Located the certificate fields for practical knowledge and documentation. 




## Chain Verification Result
Paste the output of your `openssl verify` command: openssl verify -CAfile root.pem -untrusted intermediate.pem server.pem
    server.pem: OK

---

## Certificate Roles

| Certificate  | Role                        | Key Indicator                    |
|--------------|-----------------------------|----------------------------------|
| root.pem     |issuer                       | CA: TRUE                                   
| intermediate.pem |issuer                   | CA: TRUE                                
| server.pem   |issuer                       | CA: FALSE                                     

---



## Observations

1. The chain verified successfully What did the output said "server.pem: OK".
2. I identified the root CA he root is the cert where the Subject field equals the Issuer field (self-signed). 
3. I identify the intermediate CA is confirmed by the fact that its Subject matches the leaf's Issuer.
4. The field that confirms whether a certificate can issue other certificates is the field extension's basic constraints.
5. Removing the intermediate certificate breaks the chain because it [intermediate certificate] acts as a trust bridge between the root and the leaf. Without the intermediate certificate, trust cannot be established. As stated previously in labs week 1, trust flows down, then validation walks up.




## Key Findings
Document the most important observations from the lab.
• Identify the certificate key indicators as either CA:TRUE or CA:FALSE. 
• Observe and identify the certificate's issuer
• Input openssl verify -CAfile root.pem -untrusted intermediate.pem server.pem to observe the output server.pem: OK



## Explanation
- To a PKI practitioner understanding the structure of a certificate matters. A professional will need to identify the key indicators to differentiate a CA from a leaf certificate and to observe and troubleshoot the browser's behavior to the chain if trust is not present.



## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.
- A challenged I experienced during this lab was assisting my counterparts with their openssl challenges. Their outputs showed error 2 at 2 depth lookup: unable to get issuer certificate error server.pem: verification failed. That tells me the intermediate cert is missing. The application using OpenSSL cannot find the complete certificate chain or does not know which certificate authority (CA) to trust, even if the root CA is in the Windows Certificate Manager. Windows cannot fix the gap using system chain. I tried several commands (& "C:\Program Files\OpenSSL-Win64\bin\openssl.exe" verify `-CAfile .\ca_bundle.pem ` .server.pem) to insert with the CA bundle and failed to render "server.pem: OK. I sought more senior assistance with the founder. The solution: Invoke-WebRequest -Uri "https://curl.se/ca/cacert.pem" -OutFile cacert.pem
openssl verify -CAfile cacert.pem -untrusted intermediate.pem server.pem



## Artifacts
List the files generated during this lab.

- certificate roles table
- server.pem
- intermediate.pem
- root.pem
- lab-03-certificate-chain.md