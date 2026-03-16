# Week 02 Lab — symmetric-encrypted

## Overview
The purpose of this lab is to apply and observed symmetric encryption using AES. 
The PKI system I will be observing is confidentiality. How using AES protects my data to be unreadable.

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

1. Created a readable plaintext file to observe it before encryption.
2. Using OpenSSL, I encrypted an identical plaintext file with the AES algorithm by providing a password for access.
3. Observed the encrypted file to see cipher-text, not readable at all.
4. Using OpenSSL, I decrypted the encrypted plaintext file by providing the same password to encrypt it.

---

## Results
Include the important outputs or findings from the lab.
![Certificate Inspection](../../assets/screenshots/week-02/symmetric-encrytion.png)

---

## Key Findings
Document the most important observations from the lab.

• Symmetric Cryptography 
• AES algorithm 

---

## Explanation
Explain **why the results matter**.

- Providing the correct password is equally important for encryption as it is decryption. The result would be information leakage or unreadable files.

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

- novice to AES. I believed two prompts would appear simutaneously for AES. How I resolved it was to test a theory by entering a password and observed the result.

---

## Artifacts
List the files generated during this lab.

- plaintext.decrypted.txt
- plaintext.txt
- plaintext.txt.
- screenshots stored in assets/screenshots/week-02/symmetric-encrytion.png

---

CVI PKI Career Pathway — Foundations Phase
