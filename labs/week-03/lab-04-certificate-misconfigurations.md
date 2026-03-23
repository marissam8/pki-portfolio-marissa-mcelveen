
# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab develops practical insight into common certificate misconfigurations that lead to TLS failures. I will observe certificate scenarios &determine the reason each certificate would fail validation. These misconfigurations are a common cause of real-world outages. I will be identifying issues invloving: expired certifictes, missing intermediate certificates, missing SANS subject alternative name, and incorrect key usage.


---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**
No modern browsers would not trust this certificate with missing a SAN.

**Analysis:**
Most modern browsers rely on Subject Alternative Name lists to validate a website because it can allow multiple identites to exist the under a single certificate.
When the SAN field is checked by a system and the name provided is not present on the SAN list, the certificate is rejected.

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**
No it would not accept a certificate with an incorrect extended key usage.

**Analysis:**
The EKU defines what a certificate has the permission to do such as server/client authentication and code signing.The values required for HTTPS is TLS Web Server Authentication. The error would read as NET::ERR_CERT_INVALID.
---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**
If this certificate was used today, the system would reject it automatically. 

**Analysis:**
Expiration fails validation because X.509 certificates have a defined validity periods. Expiration helps limits damage/risks, security; Forces periodic credential refresh, Reduces trust in stale credentials, supports key rotation, and limits long-term key exposure risk. Life cycle management matter because it protects both security and reliability; systems remain secure, authentications remains reliable, services function without interruption. 
---

## Scenario 4 — Missing Intermediate Certificate

**Can the browser build a complete trust chain?**
No a browser cannot complete the a trust chain with a missing intermediate certification. 

**Analysis:**
The full trust chain must be served to systems because systems do not trust certificates alone. Systems trust the chain of CA issuers. Removing the intermediate certificate breaks the chain because it acts as a trust bridge between the root and the leaf/server. Without the intermediate certificate, trust is broken and cannot be established. Browser behavior varies; as a result some browsers have cached intermediate certificates allowing trust to flow and the site accessible. Also some behavior depends on the OS trust stores or if the browser is modern.
---

## Key Takeaway
What is the most important thing you learned about certificate misconfigurations from this lab?
The most important things I learned about certificate misconfigurations from this lab was how easy it is to have an outage or certificate rejection. The fields on the certificate 
should be listed correctly in order for the certificate to be utilized as intended. Understanding how these misconfigurations appear can assist me with real-world outages. 
