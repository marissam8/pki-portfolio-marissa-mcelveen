# Week 02 — Observations


## Part 3 — Observations
Document the following in your Week 2 notes:
- Verification succeeds before tampering because the digital signature contains a hash/digital fingerprint of the original data. When the data is verified, the system compares to the signed hash. Because the data has not changed, the hashes match and the verification succeeds.
- Verification fail because changing the data changes its hash. When the system compares the new hash to the original signed hash, they no longer match, so the signature is rejected.
- Digital signatures require both hashing and asymmetric cryptography because hashing protects the integrity of the data by creating a unique fingerprint. Asymmetric cryptography then uses a key pair to sign the hash with a private key and verify it with a public key, proving the identity of the signer.
- How this relates to certificate signing in PKI is that is the step by step to the full signing process:
    1.Hash the data
    2.Sign the hash with a private key
    3.Verify the signature with a public key