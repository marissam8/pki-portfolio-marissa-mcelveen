# Lab [#01] — Diagnose an Expired Certificate

**Week 6 · PKI Incident Diagnosis & Troubleshooting**


## Incident Summary

Target system: portal.metrogeneral.org
Diagnosed by: Marissa M.
Date of diagnosis: 04/09/2026



### What failed

The TLS failure was caused by a server presenting an expired certificate for the patient-facing portal for the Metro General Hopsital.


### Evidence

- notBefore=Apr  9 00:00:00 2015 GMT notAfter=Apr 12 23:59:59 2015 GMT
- Verify return code: 10 (certificate has expired)
- patient portal security warning 


### Why it failed

The TLS failure was due to an expired certificate that was not listed in the certificate life cycle management system. There wasn't a clear owner of this certificate either. Due the aformentioned issues this caused the certificate to lapse and for chain to be terminated at a trusted root, causing the security warnings patients were seeing when they attempted to login.



### Chain status

The chain was structurally intact, I observed the identity fields for all three certificates. The chain does terminate at the trusted root.



### Remediation path

Step:
- 1: Generate new key pair & CSR. This means to create a private key and do not share it. Also create a new CSR with all the SAN entries. Confirm the subject fields are identical to the current naming conventions.
- 2: Submit to the CA to receive the signed certificate. My team would send the CSR to the CA through the CA portal or CLM system. Record the new serial number, expiration date, and assigned owner in the inventory.
- 3. Deploy a certificate with the intermediate certificate included. This step involves the full bundle build: the leaf certificate and the intermediate CA certificate. Should not deploy the leaf certificate alone, if not, the full path to the root cannot be trusted . The server must be present the complete chain.
- 4. Validate the chain after deployment: open verify -CAfile root.pem -untrusted intermediate.pem leaf.pem



### Prevention

One concrete action the organization could take to prevent this issue is to implement a centralized inventory within a Certificate Lifecycle Management (CLM) system that tracks all certificates and their associated chains (including intermediate CAs), ensuring complete and correct deployment.


## Diagnostic Steps

### Step 1 — Retrieve

Command used:

```
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | openssl x509 -outform PEM > expired_cert.pem
```

What I observed:

The certificate I received when placing the command above was the expired_cert.pem. I did not see an error during the connection. 



### Step 2 — Parse

Command used:

```
openssl x509 -in expired_cert.pem -text -noout
```

Key fields from the certificate:

| Field | Value |
|---|---|
| Subject CN | *.badssl.com |
| Issuer | COMODO RSA Domain Validation Secure Server CA  |
| Not Before | notBefore=Apr  9 00:00:00 2015 GMT |
| Not After | notAfter=Apr 12 23:59:59 2015 GMT |
| SAN entries | *.badssl.com, badssl.com |

What you found:

That the Subject name covers all the subdomains under "*.badssl.com" as well as the host. The issuer is "COMODO RSA Domain Validation Secure Server CA". I discovered that the certificate is not currently within its validity period. 



### Step 3 — Validate the Chain

Command used:

```
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null
```

Result:

The output of my command confirmed that the return code of 10 read as "Verify return code: 10 (certificate has expired)". It also confirmed that chain is structurally sound with all three certifications intact. The chain terminates at a trusted root, the leaf certificate is expired. 

What was found:

What this step confirmed is the full chain from leaf to root is present structurally.



### Step 4 — Check Revocation and Trust

Command used:

```
openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"
```

What was found:

OCSP URL is present http://ocsp.comodoca.com. The revocation status was checked there wasnt any trust store issues. 



## Reflection

 The remediation for this issue is: expiration of a certififcate itself, renders the cert as void. To correctly remediate an expired certificate would be to renew/replace certificate, deploy new chain and to add to certificate life management list. The lab provides practical knowldege of the diagnostic framework. The step for this particular issue of expiration is step two of read the certificate fields — find the failure. 



