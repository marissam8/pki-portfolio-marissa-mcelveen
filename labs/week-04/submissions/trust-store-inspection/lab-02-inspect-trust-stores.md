# Lab — Inspect Your Trust Store

## Overview
The purpose of this lab is to build upon the understanding of where trusted roots are stored on an operating system and how the trust store functions in real-world use. In this lab i will locate the trsuted store on my operating system, list the trusted root CAs, inspect a trusted root CA, and connect trust store behavior to real-world certificate validation.




## Steps Performed
Summarize the key steps you performed to complete the lab.

1. List Root CAs in the System Keychain store in root_cas.pem: security find-certificate -a -p /System/Library/Keychains/SystemRootCertificates.keychain > labs/week-04/submissions/trust-store-inspection/root_cas.pem
2. Verify the amount of root certificates in the file: grep -c "BEGIN CERTIFICATE" labs/week-04/submissions/trust-store-inspection/root_cas.pem  
3. Inspect the first certificate in the file usiing Openssl: openssl x509 -in labs/week-04/submissions/trust-store-inspection/root_cas.pem -text -noout 2>/dev/null | head -30
4. OpenSSL to validate a live certificate against my system's trusted roots: openssl s_client -connect google.com:443 -servername google.com -verify_return_error





## Results
Include the important outputs or findings from the lab.

- I found 156 trusted root CAs on my system
- The root CA that I inspected was Sectigo Public Email Protection Root E46. 
    Subject: Amazon Root CA 1 
    Expiration date: Saturday, January 16, 2038 at 7:00:00 PM Eastern Standard Time
- The verify return code output returned code 0. This means OpenSSL successfully verified my server’s certificate chain and trusts it.



## Key Findings
Document the most important observations from the lab.

• Output of the command to view all my root CA certs: security find-certificate -a -p /System/Library/Keychains/SystemRootCertificates.keychain > labs/week-04/submissions/trust-store-inspection/root_cas.pem
• The verify command for Google producting the server certificate. Observing that is labled as the server certificate". Have not seen that before. 
•  Additionally to the verify command, observing the output that verified return code being a self assigned certificate. Have not seen that before as well.
• After confirming that self-assisgned was not the desired output. I corrected the command and addes -servername flag and observed an "ok" output.
• Locating where the certs are stored in my operating system's trsut store: Applications > Utilities. Navigate to: System Roots keychain > Certificates category. 



## Explanation
Explain **why the results matter**.

- These results matter to a professional, because the practical confirms if my operating system's trust store really trust the end entity's root CA. Conducting this lab in real time provided hands-on experience in which I could locate a system's trust store. With the command openssl s_client -connect google.com:443 -servername google.com -verify_return_error I was able to observe that trust in real time. 
- The certificate chain validates successfully by validating each certificate until it reaches the root or fails. One by one the system will check each cert by the issuer. If the cert is an intermediate cert it will continue validating upward to locate its root. When the root is identified, the system will verify if the root CA is in the trust store. If located, it is valid, if not found, its rejected.




## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

- Verify return code: 18 (self signed certificate) was originally the output. I researched what this meant: not trusted. I briefly forgot this was a google command and placed the -servername flag. The ouput was verify return code: 0 (ok)


## Artifacts
List the files generated during this lab.

- root_cas.pem 
- lab-02-inspect-trust-stores.md



CVI PKI Career Pathway — Foundations Phase
