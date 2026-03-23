# Week 2 Reflection — Cryptography Fundamentals

Submit this in your portfolio repository:

reflections/week-02.md

---

## Prompts

### 1. What did you learn this week?

Explain in your own words:

- The difference between confidentiality, integrity, and authenticity is that all protect data in transit at their strengths. Confidentiality, protects data by making it unreadable (encrypts), so only those that possess the private key can decrypt it. Integrity protects data by ensuring that it is not altered and authenticity protects the data by ensuring that the original data is accounted for and one can trust that is it because it was signed from a trusted authority.
- Symmetric encryption works by having a simplier and faster algorithm than asymmetric, in which its ideal to encrypt large amounts data. It uses a public key to do this, so the same key that encrypts the data can decrypt it
- How hashing detects tampering is by taking any input and produces a fixed-length output. You can't reverse it and its unique like a fingerprint.
- Digital signatures require both hashing and asymmetric cryptography because hashing protects the integrity of the data by creating a unique fingerprint. Asymmetric cryptography then uses a key pair to sign the hash with a private key and verify it with a public key, proving the identity of the signer

Avoid copying definitions. Demonstrate understanding.

---

### 2. What concept was most challenging?

Explain what made it challenging and how you worked through it.

- The concept that was most challenging for me was digital signatures. I initially did not understand how hashing enables digital signatures. Once a digital fingerprint (hash) of the data is created, the system signs that hash using a private key. This signature proves the authenticity of the data. When the data is later verified, the system recalculates the hash and compares it with the signed hash. If they match, the data has not been altered and the signature can be trusted. 

---

### 3. Where does this appear in real-world systems?

Provide specific examples such as:

- Real world example would be the software updates, where the operating system (macOS for example) will verify digital signatures before installing updates. MacOS will only install valid digital signatures. This will prevent attackers from sending malicious software to the system.


---

### 4. How would you explain this topic to a non-technical audience?

Explain:

- I understood this concept by comparing it to a check. If someone writes a check for $90 and another person tries to change it to $900, the bank would recognize that the document was altered. Digital signatures work similarly. If the data is modified after it is signed, the verification process detects the change and the signature will no longer be valid.

Keep it simple but accurate.

---

### 5. What questions remain?

List any uncertainties about:

- Enterprise implementation

These questions will inform Week 3.

---

## Professional Growth Check

- Did you explain concepts in your own words?
- Did you connect theory to practice?
- Did you reference your lab observations?
- Is your formatting clean and structured?
- Was your commit message meaningful?

---

