# Week 02 Lab — hashing-interity

## Overview
The purpose of this lab is to generate and observe hashing. Also to observe how hashing detects tampering.  
What PKI concept is integrity. 

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

1. Created a file to generate a SHA-256 hash to. 
2. Observed the fingerprint. 
3. Tampared/modified the original file with the string "tampered".
4. Generated another SHA-256 hash with the new modified file
5. Compared both files to observe to different hash outputs.

---

## Results
Include the important outputs or findings from the lab.
If you include screenshots, store them in the **assets folder** and reference them here.


![Certificate Inspection](../../assets/screenshots/week-02/hashing-integrity.png)

---

## Key Findings
Document the most important observations from the lab.

• The SHA-256 hash line outputs
• Altered/tampered files

---

## Explanation
Explain **why the results matter**.

- Observing orginal data with the digital fingerprint vs one with the altered data is important to reinforce the PKI 
    concept of integrity. A simple character change made a different a SHA-256 hash.


---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.
- N/A

---

## Artifacts
List the files generated during this lab.

Examples:

- message_tampered.sha256.txt
- message.sha256.txt
- message.txt
- screenshots stored in assets/screenshots/week-02/hashing-integrity.png

---

CVI PKI Career Pathway — Foundations Phase
