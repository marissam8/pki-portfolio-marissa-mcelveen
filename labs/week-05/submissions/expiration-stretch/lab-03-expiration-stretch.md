# Lab — Simulate a Certificate Expiration Scenario

## Overview
In this lab I will experience practical knowledge of what a certificate expiration looks like -how to identify it, how to confirm it, and how to execute the replacement workflow. In this lab I will create a short-lived certificate and read its validity window, generate an expired certificate to simulate a real-world expiration event, utilize openssl x509 -checkend to programmatically detect expiration, observe what an expired certificate looks like when validated, and practice the full replacement workflow: generate a new key, new CSR, new certificate.


## Steps Performed
Summarize the key steps you performed to complete the lab.

1. Create a key and CSR: openssl genrsa -out labs/week-05/submissions/expiration-stretch/test_key.pem 2048
openssl req -new -key labs/week-05/submissions/expiration-stretch/test_key.pem -out labs/week-05/submissions/expiration-stretch/test_csr.pem -subj "/CN=shortlived.cvi.internal/O=CyberVisionaries Institute/C=US"
2. Issue a certificate with a brief validity window: openssl x509 -req -in labs/week-05/submissions/expiration-stretch/test_csr.pem -signkey labs/week-05/submissions/expiration-stretch/test_key.pem -out labs/week-05/submissions/expiration-stretch/test_cert_short.pem -days 1 -set_serial 01
3. Read and confirm the short validity window: openssl x509 -in labs/week-05/submissions/expiration-stretch/test_cert_short.pem -dates -noout
4. Confirm with openSSL if the certificate is still valid for 1 hour vs 1 day: openssl x509 -in labs/week-05/submissions/expiration-stretch/test_cert_short.pem -checkend 3600
openssl x509 -in labs/week-05/submissions/expiration-stretch/test_cert_short.pem -checkend 86400
5. Simulate a real-world expiration event by creating a certificate with a past validity window: /opt/homebrew/opt/openssl/bin/openssl x509 -req -in labs/week-05/submissions/expiration-stretch/test_csr.pem -signkey labs/week-05/submissions/expiration-stretch/test_key.pem -out labs/week-05/submissions/expiration-stretch/test_cert_expired.pem set_serial 02 -not_before 20230101000000Z -not_after 20230102000000Z
6. Verify the expired certificate's dates: openssl x509 -in labs/week-05/submissions/expiration-stretch/test_cert_expired.pem -dates -noout
7.  Confirm the certificate is detected as expired: openssl x509 -in labs/week-05/submissions/expiration-stretch/test_cert_expired.pem -checkend 0
8. Attempt to verify the expired certificate: openssl verify -untrusted labs/week-05/submissions/expiration-stretch/test_cert_expired.pem labs/week-05/submissions/expiration-stretch/test_cert_expired.pem
9. Execute the Replacement Workflow by creating a new private key: openssl genrsa -out labs/week-05/submissions/expiration-stretch/replacement_key.pem 2048
10. Generate a new CSR with the new key: openssl req -new -key labs/week-05/submissions/expiration-stretch/replacement_key.pem -out labs/week-05/submissions/expiration-stretch/replacement_csr.pem -subj "/CN=shortlived.cvi.internal/O=CyberVisionaries Institute/C=US"
11. Issue the replacement certificate: openssl x509 -req -in labs/week-05/submissions/expiration-stretch/replacement_csr.pem -signkey labs/week-05/submissions/expiration-stretch/replacement_key.pem -out labs/week-05/submissions/expiration-stretch/test_cert_replacement.pem -days 365 -set_serial 03
12. Confirm the replacement certificate is valid: 
openssl x509 -in labs/week-05/submissions/expiration-stretch/test_cert_replacement.pem -checkend 0


## Results
Include the important outputs or findings from the lab.

- The results of the short-lived certificate were: `notBefore` (notBefore=Apr  5 02:54:42 2026 GMT) and `notAfter` (notAfter=Apr  6 02:54:42 2026 GMT).
- The output `openssl x509 -checkend` confirm whether or not if a certificate is valid.
- The error message for `openssl verify` returned "verification failed: 10 (certificate has expired)" for the expired certificate.
- The validity period, key pair and CSR changed in the replacement certificate compared to the expired one. If this was a renewal, I would only need to replace the CSR.





## Key Findings
Document the most important observations from the lab.

Examples:

- The -checkend flag was a key finding in this lab. Good to know its importance of the status of a certificate.
- The replacement flow of a certificate. Needed the practical knowledge to observe the steps to grasp the concept. 
- Observing the output of the -checkend flag for monitoring.




## Explanation
Explain why the results matter.

- Certificate expiration still cause outages despite being 100% predictable due to a few factors: a lack of inventory for all certificates generated, lack of monitoring for validation periods, lack of ownership (a team preferrably) so there is no single point of failure, shadow certificates that are generated for short term projects like a POC aka proof of concept and no one is tracking. 
- The difference between renewal and replacement is availabity vs security. Choose renewal for longevity and replacement for security. Meaning nothing matrial has changed vs something material has changed and the trust can longer be extended. 
- To use `openssl x509 -checkend` in a real monitoring script I would use the 90, 60, and 30 day rule. 90 days out, an alert would be trigger so my team acknowledge that cert is on our radar. 60 days out a follow up to confirm the certificate is added to the team's CSR list with Subject information needed. 30 days out my team will generate the private key and send it as well as the CSR to the CA.
- Certificate inventory is a list of all known certificates owned by the company or end entity to track its validity period. It necessary at enterprise scale to prevent unnecessary outages. If one of the most predictable issues for certificates are outages, creating and maintaining a certificate inventory list is the most proactive things to do.




## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

 In this lab for step 5, I experienced an error to simulate a real-world expiration event by creating a certificate with a past validity window. The issue was that my openssl threw an error for "unknown option -not_before". I attempted to change the flag to "-day" however, placing a "0" or "-1" integer would not be recognized and openssl threw another error "bad number of days: too small". I knew the error was in my openssl itself, to complete the lab ,I needed to do an install 'brew install openssl'. 1: openssl version. I observed LibreSSL 3.3.6, this is macoS openssl default  2./bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)". I observed a password prompt. After inputting my password, the script installed folders/directories with homebrew. 3. I observed a prompt "Press RETURN/ENTER to continue or any other key to abort:" I selected " ENTER" to continue. The downloading and installing of more homebrew objects continued. My cursor returned to my main directory. 4. Added Homebre to my path (for Apple Silicon Macs) : echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile then after eval "$(/opt/homebrew/bin/brew shellenv)". I was then able to successfully run the full command in step 5 following output of "Certificate request self-signature ok".


 

## Artifacts
List the files generated or submitted during this lab.

- lab-03-expiration-stretch.md
- test_cert_replacement.pem
- test_cert_expired.pem
- test_cert_short.pem 

