# Lab — Generate a CSR and Simulate the Issuance Workflow

## Overview
This lab demonstrates the X.509 certificate lifecycle, covering the process from private key generation and CSR creation to signing. In this lab I will: generate a private key, create a Certificate Signing Request (CSR) and inspect its fields, self-sign a certificate to simulate what a CA does when it issues a certificate, inspect the signed certificate and map its fields back to the CSR, and connect the CSR Subject fields to the X.509 certificate anatomy.



## Steps Performed
Summarize the key steps performed to complete the lab.
1. Generate a private key: openssl genrsa -out labs/week-05/submissions/generate-csr/test_key.pem 2048
2. Confirm the test_key.pem creation: openssl rsa -in labs/week-05/submissions/generate-csr/test_key.pem -text -noout | head -5
3. Generate a CSR: openssl req -new -key labs/week-05/submissions/generate-csr/test_key.pem -out labs/week-05/submissions/generate-csr/test_csr.pem -subj "/CN=lab01.cvi.internal/O=CyberVisionaries Institute/OU=PKI Career Pathway/C=US/ST=California/L=San Francisco"
4. Inspect the test_csr.pem CSR into readable text: openssl req -in labs/week-05/submissions/generate-csr/test_csr.pem -text -noout
5. Sign the certificate: openssl x509 -req -in labs/week-05/submissions/generate-csr/test_csr.pem -signkey labs/week-05/submissions/generate-csr/test_key.pem -out labs/week-05/submissions/generate-csr/test_cert.pem -days 365 -set_serial 01
6. Observe and inspect the signed certificate: openssl x509 -in labs/week-05/submissions/generate-csr/test_cert.pem -text -noout
7. Compare CSR to Signed Certificate. Confirm the public key matches via extract the public key from the certificate: openssl x509 -in labs/week-05/submissions/generate-csr/test_cert.pem -pubkey -noout > /tmp/cert_pubkey.pem
8. Extract the public key from the private key: openssl rsa -in labs/week-05/submissions/generate-csr/test_key.pem -pubout > /tmp/key_pubkey.pem
9. Compare them: diff /tmp/cert_pubkey.pem /tmp/key_pubkey.pem


| Field | Abbreviation | Value in Your CSR |
|---|---|---|
| Common Name | CN |lab01.cvi.internal |
| Organization | O |CyberVisionaries Institute |
| Organizational Unit | OU |PKI Career Pathway |
| Country | C |US |
| State | ST |California |
| Locality | L |San Francisco |



## Results
Include the important outputs or findings from the lab.

- What Subject fields did you include in your CSR: Common Name, Organization, Organizational Unit, Country, State, and Locality. Each of the identity fields represent where the identity is from.
- The Issuer field in a self-signed certificate is the Subject, they're identical because there is no external CA involved. Certificate is verifying itself.
- The public key comparison (diff) showed me there is a match between the public key in the certificate and the public key extracted from the private key. It tells me they belong to the same key pair.
- The fields in my signed certificate connect to what I learned about X.509 in Week 3 by building onto the concept of the certificate's identity. These additional fields are more descriptive  (Organization Name, Organization, etc) and add location to the certificate owner's identity. 



## Key Findings
Document the most important observations from the lab.

- I discovered the certificate's additional fields (L, ST, C, etc) are in the subject line prior to the start of the public key. 
- This flag "-set_serial 01" set the version to 1 vs 3.
- The validation result indicated the certs belonged to the correct key pair.
- Unexpected behavior was locating the tmp file. I could access this by selecting the file via "cmd + select" but I cannot find the folder's location.



## Explanation
Explain why the results matter.

- The purpose of a CSR in the real certificate issuance workflow is the verifying the certificate's identity. It answers the question of "who does this certificate belong to?". The Subject fields are the identity fields the CA verifies. 
- The CA receives a CSR rather than a private key becuase the private key is the owner's personal key. It is not needed for a CSR. If the CA obtained the private key, everyone in the CA could impersonate the owner. 
- If the public key in the certificate did NOT match the private key it would mean that the pair doesn't belong to the same keypair. There would be errors in the output such as "handshake failure".



## Challenges / Troubleshooting
Document any issues encountered during the lab and how they were resolved.

- N/A



## Artifacts
List the files generated or submitted during this lab.


- test_csr.pem
- test_cert.pem
- lab-01-generate-csr.md



*CVI PKI Career Pathway — Foundations Phase*
