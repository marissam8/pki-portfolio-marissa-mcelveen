# Lab [#03] —  Diagnose a Hostname and SAN Mismatch

**Week 6 · PKI Incident Diagnosis & Troubleshooting**

## Incident Summary

Target system: Staff scheduling portal
Diagnosed by: Marissa M.
Date of diagnosis: 04/12/2026


### What failed

The TLS failure was caused by missing SANs in the certificate which would be a mismatch, the server's identity cannot be verified. 

### Evidence

- error 20 at 0 depth lookup: unable to get local issuer certificate
- error mismatch_cert.pem: verification failed



### Why it failed

The verification failed because the certificate does not include the requested hostname in its Subject Alternative Name (SAN), causing a hostname mismatch.



### Chain status

The certificate chain is not structurally intact, the intermediate certificate is missing causing the chain to be broken.



### Remediation path

1. New CSR with all the appropriate explicitly names listed in the SAN field.
2. Assess scope: are other hostnames migrating? Add them now - one renewal beats many.
3. Wildcard option: * metrogeneral. org covers all current and future subdomains.
4. Multi-SAN certificate if specific subdomains are preferred over wildcard scope
5. New certificate with the correct hostname in the SAN

### Prevention

One concrete thing the organization could do differently to prevent this failure type from happening is to automate DNS updates. The certificate renewal will automatically add new hostnames to the SAN to prevent misconfigurations.

## Diagnostic Steps

### Step 1 — Retrieve

Command used:

```
openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com </dev/null 2>/dev/null | openssl x509 -outform PEM > mismatch_cert.pem
openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com </dev/null 2>&1
```

What you observed:

The certificate retrieved, was mismatch_cert.pem. There was no error connection. I also observed "Verify return code: 0 (ok)". There was no OpenSSL report a hostname mismatch errors.



### Step 2 — Parse

Command used:

```
openssl x509 -in mismatch_cert.pem -text -noout
openssl x509 -in mismatch_cert.pem -noout -text | grep -A5 "Subject Alternative Name"
```

Key fields from the certificate:

| Field | Value |
|---|---|
| Subject CN | *.badssl.com |
| Issuer | R13 |
| Not Before | Mar 24 20:02:52 2026 GMT |
| Not After | Jun 22 20:02:51 2026 GMT |
| SAN entries | *.badssl.com, DNS:badssl.com  |

What you found:

The Subject CN is *.badssl.com and the DNS: *.badssl.com, badssl.com. I have the wildcard option for the Subject CN.


### Step 3 — Validate the Chain

Command used:

```
openssl verify mismatch_cert.pem
```

Result:

CN=*.badssl.com
error 20 at 0 depth lookup: unable to get local issuer certificate
error mismatch_cert.pem: verification failed

What you found:

Step 2 of the diagnostic framework confirms that this is a mismatch SAN issue. Even though I have the wildcard option, the better practice would be to list the multi-san lists. The chain is also broken and does not validate successfully. 

### Step 4 — Check Revocation and Trust

Command used:

```
openssl x509 -in mismatch_cert.pem -noout -text | grep -A2 "OCSP"
```

What you found:

The OCSP URL is absent. The revocation status is still important. The CSR changes not the private key and it is still used for this, 

## Reflection

[2–3 sentences: What did this lab reinforce or clarify for you? Was there a step where
you had to slow down and think carefully?]


