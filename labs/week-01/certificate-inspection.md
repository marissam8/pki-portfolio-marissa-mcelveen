# Week 01 Lab — Certificate Inspection

## Screenshot Evidence

1. Capture a screenshot of the certificate details in your browser.
2. Save it as:

assets/screenshots/week-01/certificate-inspection.png

3. Embed the screenshot below:

![Certificate Inspection](assets/screenshots/week-01)


## Website Information

**Website inspected:**  
(https://digitalomni.navyfederal.org/signin/)

**Issuer (Certificate Authority):**  
DigiCert Inc

**Valid from:**  
Thursday, May 22, 2025 

**Valid until:**  
Friday, May 22, 2026 

**Signature algorithm:**  
ECDSA Signature with SHA-384 ( 1.2.840.10045.4.3.3 )

---

## Subject Alternative Names (SAN Entries)

List at least 2–3 SAN entries:

- digitalomni.navyfederal.org
- www.digitalomni.navyfederal.org
- 

---

## Observations

Document three observations about the certificate.

### Observation 1
I noticed that when I orignially selected the encryptic connection for Mac, There are three 
certifications shown -the root, the intermediate and the Website SSL Certificate

### Observation 2
I observed the intermedite certificate has a longer validation period. This domain has a year in 
comparison to the intermediate of ten years.

### Observation 3
I also observed that the signature algorithm are identical.

---

## Reflection

Based on your inspection, explain how this certificate contributes to secure HTTPS communication.

The certificate confirms that the website belongs to the domain you are visiting. The browser checks it with a trusted CA to ensure the site is legitimate and not an impersonator.
