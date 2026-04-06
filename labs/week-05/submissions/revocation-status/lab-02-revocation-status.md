# Lab — Check Certificate Revocation Status with OCSP

## Overview
This lab develops a practical understanding of certificate revocation by demonstrating how Online Certificate Status Protocol (OCSP) enables systems to verify a certificate’s validity in real time before deciding to trust it.



## Steps Performed
Summarize the key steps you performed to complete the lab.

1. Create an artifact directory for leaf_cert.pem to live in: mkdir -p labs/week-05/submissions/revocation-status
2. Connect to a live site and extract the leaf certificate: echo | openssl s_client -connect github.com:443 2>/dev/null | openssl x509 
  -out labs/week-05/submissions/revocation-status/leaf_cert.pem
3. Confirmation of leaf_cert.pem saved correctly: openssl x509 -in labs/week-05/submissions/revocation-status/leaf_cert.pem -subject -issuer -dates -noout
4. Retrieve the full chain from the server to perform an OCSP query: echo | openssl s_client -connect github.com:443 -showcerts 2>/dev/null > labs/week-05/submissions/revocation-status/fullchain.txt
5. Capture the intermediate certificate and save as issuer_cert.pem: labs/week-05/submissions/revocation-status/issuer_cert.pem
6. Observe and verify the issuer is correct: openssl x509 -in labs/week-05/submissions/revocation-status/issuer_cert.pem -subject -issuer -noout
7. Examine the leaf certificate’s extensions to locate the OCSP responder URL: openssl x509 -in labs/week-05/submissions/revocation-status/leaf_cert.pem -text -noout | grep -A4 "Authority Information Access"
8. Retreive the OCSP URL from the certificate automatically: OCSP_URL=$(openssl x509 -in labs/week-05/submissions/revocation-status/leaf_cert.pem -text -noout | grep "OCSP" | grep "http" | sed 's/.*URI://' | tr -d ' ')
echo "OCSP URL: $OCSP_URL"
9. Query the OCSP responder: openssl ocsp -issuer labs/week-05/submissions/revocation-status/issuer_cert.pem -cert labs/week-05/submissions/revocation-status/leaf_cert.pem -url "$OCSP_URL" -resp_text -noverify 2>/dev/null | tee labs/week-05/submissions/revocation-status/ocsp_response.txt




## Results
Include the important outputs or findings from the lab.

- The leaf certificate Subject is github and Issuer is Sectigo Public Server Authentication CA
- The OCSP URL in the Authority Information Access extension is http://ocsp.sectigo.com
- The OCSP response status for the certificate: "good"
- The "This Update" and "Next Update" are two timestamps that tell me the time window I can resuse the OCSP response. 



## Key Findings
Document the most important observations from the lab.

- I discovered that extracting the fullchain cert in a file adds subject and issuer context to the fullchain certificate.
- How this flag -url "$OCSP_URL" specifies the responder endpoint that OpenSSL should query. 
- If an OCSP responder is unavailable, most systems allow the certificate through anyway, this is called a "soft fail". The security implications of "soft fail" is that an attacker has the ability to use a certificate until the OCSP responder is available again. The security implications of a "hard fail" is that it closes the "gap" and revoked certificates will not be allowed. Hard fails prioritizes security over availabilty. No man in the middle attacks but if the OCSP responder is unavailable, legitmate will fail, causing outages.

## Explanation
Explain why the results matter.

- OCSP needs both the leaf and issuer certificate because a certificate is identified by its serial number and the CA that issued it. 
- The difference between OCSP and CRL in practice is that OCSP searches for the revocation status of a certificate in real time. It checks for a single certificate and returns quickly with the status. The CRL searches for the revocation status of many certificates downloaded (cached) locally. This will take the system time to review. 
- If a system trusted a revoked certificate due to the OCSP was unavailable an attacker has the ability to use a certificate until the OCSP responder is available again.

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.
N/A

## Artifacts
List the files generated or submitted during this lab.

- leaf_cert.pem
- issuer_cert.pem
- ocsp_response.txt
- lab-02-revocation-status.md
