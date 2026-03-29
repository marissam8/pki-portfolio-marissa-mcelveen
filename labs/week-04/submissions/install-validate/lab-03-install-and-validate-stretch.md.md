# Lab — Install a Certificate and Validate Trust

## Overview
The purpose of this lab is to experience additional hands-on experience with the full certificate installation and trust validation workflow. in this lab I will generate a self-signed root CA certificate, install it into my operating system's trust store, create a certificate signed by that CA, validate that your system now trusts the signed certificate, and remove the certificate cleanly after the lab. 

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

1. Generate a test root by creating a private key and self-signed root CA certificate: openssl genrsa -out labs/week-04/submissions/install-validate/test-root-ca.key 2048
openssl req -new -x509 -key labs/week-04/submissions/install-validate/test-root-ca.key -out labs/week-04/submissions/install-validate/test-root-ca.crt -days 30 -subj "/CN=CVI-Lab-Root-CA/O=CyberVisionaries Institute/C=US"
2. Verify the certificate was created correctly: openssl x509 -in labs/week-04/submissions/install-validate/test-root-ca.crt -text -noout
3. Install the Root CA into my Trust Store: sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain labs/week-04/submissions/install-validate/test-root-ca.crt
4. Create a Certificate Signed by my Test Root CA by generating a key and CSR for a test certificate: openssl genrsa -out labs/week-04/submissions/install-validate/test-signed.key 2048
openssl req -new -key labs/week-04/submissions/install-validate/test-signed.key -out labs/week-04/submissions/install-validate/test-signed.csr -subj "/CN=test.cvi-lab.local/O=CyberVisionaries Institute/C=US"
5. Sign the CSR with my test root CA: openssl x509 -req -in labs/week-04/submissions/install-validate/test-signed.csr -CA labs/week-04/submissions/install-validate/test-root-ca.crt -CAkey labs/week-04/submissions/install-validate/test-root-ca.key -CAcreateserial -out labs/week-04/submissions/install-validate/test-signed.crt -days 30
6. Use OpenSSL to verify the signed certificate chains up to my test root CA: openssl verify -CAfile labs/week-04/submissions/install-validate/test-root-ca.crt labs/week-04/submissions/install-validate/test-signed.crt
7. Remove the test root CA from your trust store: sudo security delete-certificate -c "CVI-Lab-Root-CA" /Library/Keychains/System.keychain 

## Results
Include the important outputs or findings from the lab.

- The certificate output showed that the verified test-root-ca.crt subject and issuer: CVI-Lab-Root-CA. Meaning it is a self-assigned CA. The expiration date: Not Before: Mar 27 16:07:50 2026 GMT
                 Not After : Apr 26 16:07:50 2026 GMT
- The verify output return after signing was "Signature ok" — before and after cleanup.
- What confirmed that the trust chain was established was the output of: "labs/week-04/submissions/install-validate/test-signed.crt: OK".


## Key Findings
Document the most important observations from the lab.

- One of the key finding that reaffirmed the self-assigned concept is the subject and issuer being identical.
- How I could generate a certificate for a validity period as short as 30 days.
- Observing the argument for my system to trust my certificate. This is the first time I have every utilized something like that.
- Understanding the importance of a root CA installation. My system prompted me to sign-in as an administrator when I instructed it to add test-root-ca.crt.




## Explanation
Explain **why the results matter**.

- What made the test root CA self-signed is that it was signed using its own private key.  The key indicator is that the subject and issuer are the same.
- What changed on my system after I installed test-root-ca.crt root CA, is that it is now trusted in my OS's trusted store. Validation will be effective for any certificate signed by test-root-ca.crt.
- In an enterprise environment, a group policy, MDM, or provisioning scripts controls what root CAs are installed on employee machines. 
- If an attacker can install a root CA on a device they could impersonate a trusted service and it would allow them to access to issue certificates, decrypt HTTPS traffic and etc. Trust would be broken. This is a security concern.

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

Examples:

- N/A

---

## Artifacts
List the files generated or submitted during this lab.

Examples:

- test-root-ca.crt
- test-signed.crt
- lab-03-install-and-validate.md


*CVI PKI Career Pathway — Foundations Phase*