# Week 02 Lab — digital-signatures

## Overview
The purpose of this lab is to geberate and observe a digital signature and validate it with a public key.
The PKI concept I will be investigating is both integrity and authenticity. 

---

## Environment
Document the environment used to complete the lab.

- Operating System: macOS
- Terminal Used: Integrated terminal in Visual Studio Code (zsh shell)
- OpenSSL Version (if applicable): LibreSSL 3.3.6

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

Do **not copy the lab instructions**.  
Describe what you actually did.

1. Generate a SHA-256 hash file message.txt
2. Signed the hash message.txt file with the private key
3. Validate the digital signature with the public key
4. Tampered with the orignal document using "tampered" string 
5. Attempted to sign with the public key
6. Observed the verification fail

---

## Results
Include the important outputs or findings from the lab.
If you include screenshots, store them in the **assets folder** and reference them here.

![Signature Output](../../assets/screenshots/week-02/digital-signature.png)

---

## Key Findings
Document the most important observations from the lab.

• Observe the digital verification of original, authentic data
• Observe the rejection of the altered data
• Using the both keys to sign the orginal data (private) and verify signature (public)

---

## Explanation
Explain **why the results matter**.
- These results matters to further prove that a digital signature is not just integrity and authenticity in theory. Through the lab it is proven to enforce both. 

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

-N/A

---

## Artifacts
List the files generated during this lab.

- artifact.sig
- artifact.txt
- public_key.pem
- root.pem
- screenshots stored in assets/screenshots/week-02/digital-signature.png

---

CVI PKI Career Pathway — Foundations Phase
