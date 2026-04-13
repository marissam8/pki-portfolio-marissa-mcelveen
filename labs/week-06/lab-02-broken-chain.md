# Lab [#02] — Diagnose a Broken Certificate Chain

**Week 6 · PKI Incident Diagnosis & Troubleshooting**

## Incident Summary

Target system: Radiology imaging platform
Diagnosed by: Marissa M.
Date of diagnosis: 4/12/2026


### What failed

The TLS failure was caused by the server presenting an incomplete certificate chain after renewal, missing the intermediate CA certificate required for clients to establish trust. 

### Evidence

- Verify return code: 21 (unable to verify the first certificate)
- verify error:num=20:unable to get local issuer certificate
- error 20 at 0 depth lookup: unable to get local issuer certificate & error leaf_cert.pem: verification failed


### Why it failed

The TLS failure was due to a miconfiguration, the intermediiate certificate was not added to the server. As a result, the trust bridge (intermediate cert) from the leaf to the root could not be established causing the connection to fail.  

### Chain status

No, the certificate chain was not structurally intact? The other chain-related issues separate from
the primary failure of the certificate leaf certificate is that my certificate chain is not repaired. I still have an error of "Verify return code: 21 (unable to verify the first certificate)". 




### Remediation path

1. Locate the immediate URI: openssl x509 -in leaf_cert.pem -noout -text | grep -A3 "Authority Information Access". it is th download for the intermediate certificate.
2. Download and convert: curl -s "https://[URI from CA Issuers extension]" -o intermediate.der then openssl x509 -inform DER -in intermediate.der -out issuer_cert.pem.
3. Validate with the intermediate certificate present: openssl verify -untrusted issuer_cert.pem leaf_cert.pem
4. Reconfigure the server to send the full bundle: Together -both the leaf and intermediate certs will be placed in the server's bundle. Reload the TLS config. verify live using openssl s_client: openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null | grep "Verify return code".



### Prevention

One concrete action is to implement automated certificate deployment checks that verify the full certificate chain is correctly installed after renewal, preventing TLS failures due to incomplete chains.


## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

Command used:

```
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM > leaf_cert.pem
openssl s_client -connect incomplete-chain.badssl.com:443 </dev/null 2>&1
```

What was observed:

I observed the leaf_cert.pem get generated. Then afterwards I observed a successful connection. The CN populated but errors followed: verify error:num=20:unable to get local issuer certificate and verify error:num=21:unable to verify the first certificate

### Step 2 — Parse

Command used:

```
openssl x509 -in leaf_cert.pem -text -noout
openssl x509 -in leaf_cert.pem -noout -text | grep -A3 "Authority Information Access"
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

The intermediate certificate was missing, it was not properly configured. The URL points to the intermediate CA certificate URI:http://r13.i.lencr.org/

### Step 3 — Validate the Chain

Command used:

```
openssl verify leaf_cert.pem
curl -s "https://r13.i.lencr.org" -o intermediate.der
openssl x509 -inform DER -in intermediate.der -out issuer_cert.pem
openssl verify -untrusted issuer_cert.pem leaf_cert.pem
```

Result:

CN=*.badssl.com
error 20 at 0 depth lookup: unable to get local issuer certificate
error leaf_cert.pem: verification failed
leaf_cert.pem: OK

What you found:

Step 3 of the diagnostic framework confirmed the broken chain of trust: openssl verify leaf_cert.pem the output was error 20 at 0 depth lookup: unable to get local issuer certificate, error leaf_cert.pem: verification failed. However when I retrieved the missing intermediate, converted the der file to a pem and verify with the intermediate included, my leaf_cert.pem: OK for the output.


### Step 4 — Check Revocation and Trust

Command used:

```
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null | grep "Verify return code"
```

What you found:

The OCSP URL was absent. The revocation status if checked had trust store issues: openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null | grep "Verify return code". The output was: Verify return code: 21 (unable to verify the first certificate).


## Reflection

For this lab's reflection, using the practical diagnostic framework steps assisted me in identifying the issue. Step 3 was the primary focus of this messon. The remediation for the missing intermediate cert is where I am still stuck on due to following all the steps and my intermediate is still broken due to missing intermediate cert. 


