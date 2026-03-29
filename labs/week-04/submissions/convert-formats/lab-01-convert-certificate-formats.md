# Lab — Convert Certificate Formats

## Overview
The purpose of this lab is to build upon the understanding of certificate formats and how to convert between them using OpenSSL. These formats include PEM, DER, and PFX. In this lab I will be retrieving a live certificate, inspecting raw encoding, converting a live certificate, bundling a certifcate, and verifying that the conversions of the certificate are valid.



## Steps Performed
Summarize the key steps you performed to complete the lab.

1. Retrieve google's PEM format cert by connecting to google's website and retreive it's leaf cert: openssl s_client -connect google.com:443 -servername google.com -showcerts
2. Identify and save the leaf cert as leaf_cert.pem: labs/week-04/submissions/convert-formats/leaf_cert.pem
3. Inspect the leaf_cert.pem file: openssl x509 -in labs/week-04/submissions/convert-formats/leaf_cert.pem -text -noout 
4. Then convert leaf_cert.pem to a .der file: openssl x509 -in labs/week-04/submissions/convert-formats/leaf_cert.pem -outform DER -out labs/week-04/submissions/convert-formats/leaf_cert.der 
5. Observe .leaf_cert.der file. Reverse the conversion of the leaf_cert.der into a .pem file: openssl x509 -in labs/week-04/submissions/convert-formats/leaf_cert.der -inform DER -outform PEM \
-out labs/week-04/submissions/convert-formats/leaf_cert_restored.pem
6. Observe the restored leaf_cert_restored.pem
7. Create a PFX file and pair it with a private key. Generate the a test key pair and self-sign a certificate: openssl genrsa -out labs/week-04/submissions/convert-formats/test_key.pem 2048
    openssl req -new -x509 -key labs/week-04/submissions/convert-formats/test_key.pem -out labs/week-04/submissions/convert-formats/test_cert.pem -days 365 -subj "/CN=Week4-Lab-Test/O=CVI/C=US"
8. Bundle both cert & key into PFX file: openssl pkcs12 -export -out labs/week-04/submissions/convert-formats/test_bundle.pfx -inkey labs/week-04/submissions/convert-formats/test_key.pem -in labs/week-04/submissions/convert-formats/test_cert.pem
9. Confirm the test_bundle.pfx is valid: openssl pkcs12 -info -in labs/week-04/submissions/convert-formats/test_bundle.pfx -noout

## Results
Include the important outputs or findings from the lab.

- openssl genrsa -out labs/week-04/submissions/convert-formats/test_key.pem 2048 to get:
   Generating RSA private key, 2048 bit long modulus
    ..+++++
    ............+++++
    e   is 65537 (0x10001)     
- Verifed output of PFX file
- observed the generation and output of the .der, .pem, .pfx files.
- The password prompt for openssl pkcs12 -export -out labs/week-04/submissions/convert-formats/test_bundle.pfx -inkey labs/week-04/submissions/convert-formats/test_key.pem -in labs/week-04/submissions/convert-formats/test_cert.pem





## Key Findings
Document the most important observations from the lab.

- Observing and identifying the .pem file by the BEGIN header and END footer
- The conversion command from .pem to .der: openssl x509 -in labs/week-04/submissions/convert-formats/leaf_cert.pem -outform DER -out labs/week-04/submissions/convert-formats/leaf_cert.der
- Observing .der garbled files via text editor, not trusting the file type.
- The reverse conversion of .der to .pem: openssl x509 -in labs/week-04/submissions/convert-formats/leaf_cert.der -inform DER -outform PEM -out labs/week-04/submissions/convert-formats/leaf_cert_restored.pem
- Observing the validity of the reverse conversion's .pem and not trusting the file type. Observed the BEGIN header and END footer.
- Observing the keypair generation 
- Observing and verifying the PFX files after generating the keypair


## Explanation
Explain **why the results matter**.

- This practical knowledge is important to PKI professionals to distingush between certificate formats versus a certificate problem. It is crucial to know which is the issue to target for torubleshooting to prevent outages.


## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

- Remembering that using google's certs I need the -servername flag or retrieve the default certificate: openssl s_client -connect google.com:443 -servername google.com -showcerts instead of openssl s_client -connect google.com:443 google.com -showcerts



## Artifacts

- lab-01-convert-certificate-formats.md
- leaf_cert_restored.pem
- leaf_cert.der
- leaf_cert.pem
- test_bundle.pfx
- test_cert.pem


*CVI PKI Career Pathway — Foundations Phase*
