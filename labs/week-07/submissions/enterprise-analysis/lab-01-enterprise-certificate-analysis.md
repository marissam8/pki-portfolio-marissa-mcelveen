# Lab — Analyze a Live Enterprise Certificate Deployment

## Overview
By the end of this lab I will have: retrieved and inspected a live enterprise TLS certificate deployment using the full diagnostic framework, identified the CA type, certificate chain, and certificate profile of a real production deployment, determined where TLS appears to terminate — app server, load balancer, or CDN, and documented my findings in a structured analysis formatted as an infrastructure assessment.



## Steps Performed

1. Choose a target hostname: salesforce.com
2. Retrieve the certificate by connecting the live endpoint: openssl s_client -connect salesforce.com:443 -showcerts 2>/dev/null | openssl x509 -outform PEM > enterprise_cert.pem
3. Retrieve the full chain: openssl s_client -connect salesforce.com:443 -showcerts 2>/dev/null > full_chain_output.txt
4. Parse the certificate to readable text: openssl x509 -in labs/week-07/submissions/enterprise_cert.pem -text -noout
5. List all DNS entries in the SAN extension: openssl x509 -in labs/week-07/submissions/enterprise_cert.pem -noout -text | grep -A10 "Subject Alternative Name"
6. Examine the full chain and identify each certificate in the trust path: openssl s_client -connect salesforce.com:443 -showcerts 2>/dev/null
7. Determine Where TLS Terminates by checking for CDN or load balancer indicators: openssl x509 -in labs/week-07/submissions/enterprise_cert.pem -noout -issuer
8. Check for CDN-related SAN entries or wildcard patterns: openssl x509 -in labs/week-07/submissions/enterprise_cert.pem -noout -text | grep -A10 "Subject Alternative Name"
9. Check server response headers for CDN indicators: curl -sI https://salesforce.com:443 | grep -i "server\|cf-ray\|x-cache\|via\|x-amz"




## Results

- https://www.ssllabs.com/ssltest/ for labs analysis
- How the labs are graded by placing them into the above site
- Certificate Type: OV (organization validation)
- TLS versions are supported or not


## Key Findings
Document the most important observations from the lab.

Certificate Summary: Issuer, validity window, certificate type (DV/OV/EV), SAN count, wildcard usage.
- Did the connection succeed: yes 
- How many certificates are in the chain output: 2
- Validity: Not Before: Mar 18 00:00:00 2026 GMT and Not After: Oct  2 23:59:59 2026 GMT
- Approximate remaining validity (in days): 166 days
- Subject: salesforce.com
Chain Analysis: Number of certificates in chain, intermediate CA identity, root CA identity, chain completeness.
- How many certificates are in the chain output: 2
- Common Name (CN): salesforce.com and Organization (O): Salesforce
- How many SAN entries are there: 71
Termination Analysis: Where TLS appears to terminate (app server / load balancer / CDN) and the evidence that supports your conclusion.
- Is an intermediate CA certificate present: yes  What is its Subject: DigiCert Global G3 TLS ECC SHA384 2020 CA1
- The chain does not terminate at a known public root. It terminates at Server: AkamaiGHost
- Is the chain complete — leaf → intermediate → root: No. Is there is a layer missing Or is any layer missing: No.
- Does the issuer suggest this is a CDN-managed certificate (e.g., issued by Cloudflare, Fastly, DigiCert for a CDN provider): Yes
- The server headers indicate a CDN is in the request path: Server: AkamaiGHost
- Based on my findings, the TLS terminates for this deployment — at a CDN edge.
TLS Configuration: SSL Labs grade, TLS versions, HSTS, OCSP stapling.
- SSL Labs overall grade: A+
- TLS versions supported: 1.2, 1.3
- Are deprecated TLS versions (1.0 or 1.1) still supported: No
- Is HSTS (HTTP Strict Transport Security) configured: Yes
- Is OCSP stapling enabled: Yes
CT Log Analysis: CA consistency, any unexpected issuers, certificate validity period pattern.
- N/A



## Explanation
Explain **why the results matter**

These results matter because it provides practical knowledge into certificate coverage when I am in the field. It is important for a professional to know where the live certificate lives, where it terminates, if the full chain is provided, and if the root is in my trusted store. These will be important to know once I am my team encounter some issues that surround the certificate infrastructure and ownership in the event there is concern regarding TLS termination, load balancers, and life cycle problems. 



## Challenges / Troubleshooting
Document any issues encountered during the lab and how I resolved them.

- N/A


## Artifacts

- enterprise_cert.pem
- lab-01-enterprise-certificate-analysis.md





