# Lab 01 — Inspect X.509 Certificate Fields

## Overview
Inspect the structure of a X.509 Certificate. Observe and identify the core fields that authenticate a certificate's identity. The componenets 
of the certificate's anatomy is important to investigate to prevent impersonation and enforce authentication.


## Environment
- Operating System: macOS
- Terminal Used: Integrated terminal in Visual Studio Code (zsh shell)
- OpenSSL Version (if applicable): LibreSSL 3.3.6


## Steps Performed
Summarize the key steps you performed to complete the lab.
1. Performed the command openssl s_client -connect google.com:443 -showcerts to identify the leaf cert as the 
     first cert due to length and order.
2. I copied the leaf cert into a pem file named leaf_cert.pem.
3. Converted the file into a readable text with openssl.
4. Located the certificate fields for practical knowledge and documentation. 



## Certificate Fields

| Field                | Value from your output |
|----------------------|------------------------|
| Version              |3                        
| Serial Number        |1c:fb:1d:f7:99:b3:a3:61:10:d3:9b:8e:7d:3c:a8:53                        
| Signature Algorithm  |sha256WithRSAEncryption                        
| Issuer               |Google Trust Services                        
| Subject              |*.google.com                        
| Not Before           |Not Before: Feb 23 18:19:44 2026 GMT                        
| Not After            |Not After : May 18 18:19:43 2026 GMT                        
| Public Key Algorithm |id-ecPublicKey                        

## Observations

1. Who issued the certificate: Google Trust Services
2. What domain or organization does it represent: *.google.com
3. When does it expire: Not Before: Feb 23 18:19:44 2026 GMT
                        Not After : May 18 18:19:43 2026 GMT
4. What public key algorithm is used: id-ecPublicKey
5. The Issuer field matters in a PKI system because PKI relies on trusted certificate authorities to establish trust. Systems trust certificates when they trust the issuing authority.




## Key Findings
Document the most important observations from the lab.
• Identify the certificate fields. I originally observed "invalid" for subject and issuer. I knew my input command was incorrect. (explained in challenges)
• Observe and identify the serial number. This is my first introduction to it. Serial numbers are generated when a cert is issued to be tracked, audited, and revoked via CRL or OCSP



## Explanation
- To a PKI practitioner understanding the structure of a certificate matters. A professional will need to identify these main components that establish digital trust in the likelihood of troubleshooting failures and errors.



## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.
- A challenged I experienced during this lab was from step 1 of part 1. I needed an additional flag. For my original command "openssl s_client -connect google.com:443 -showcerts" was missing "-servername google.com" flag and SNI. Without it, it provides a default certificate and an "invalid" output for both issuer and subject. I knew this would be important when I scanned the remainder of the lab in sections like certificate fields. I looked for additonal flags online to observe a readable output that would provide the subject and issuer. My new command: openssl s_client -connect google.com:443 -showcerts

## Artifacts
List the files generated during this lab.

- certificate fields table
- leaf_cert.pem
- lab-01-certificate-fields.md
