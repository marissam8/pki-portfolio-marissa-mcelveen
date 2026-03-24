# Week X Lesson Notes — Certificate Anatomy

## 1. Core Concept


The foundational concept of this week was the certificate structure. In real-world systems for certificates, X.509 is the industry standard for digital certificates.
They are the core building block of PKI. Certificates enable trust, encryption and identity verification across systems. Identifying and understanding the core fields are essential to solving misconfigurations, validation issues, authentication errors and the like. 

## 2. Why It Matters

This concept appears in real enterprise environments such as TLS failures, host mismatches, and or application outages. For instance, if a certification validity period expires, every application associated with that certification loses the trusted connection


## 3. Technical Breakdown

Subject: who the cert belongs to?:
	it may rep a person, a server hostname(server.company.com, device entity(device-001.corp)
Issuer: The CA
	the issuer is the CA  that verified identity and signed the cert. inly trusted CAs can issue valid credentials.
	identity requests---->CA verification --> Certificate issued
	CA are responsible for verififying identity and issuing certs.
Validity Period: Certificates Expires
	cert expires = broken trust
	expiration helps limits damage/risks if a key/credentials are compromised
Public Key: the cryptographic identity
	public key- included in the cert, shared openly
	private key- never leaves the owner, kept secret
	the certificate carries public key. Anyone can encrypt data or verify signatures using it. only the private key holder can decrypt or sign.
Serial number: Unique cert identifier
	every cert has a unique serial number assigned by the issuing CA. serial numbers allow certs to be tracked, audited, and revoked via CRL or OCSP
	cert issued-->serial recorded___>revoked if compromised>serial added to revocation list
Digital Signature: Proof of Authenticity
	the digital signature is the CA's cryptographic stamp of aproval
		guarantees authenticity (proves CA issued the cert)
		protects integrity(any tampering invalidates signature)
		prevents forgery(cannot be reproduced w/o CA private key)
Certificate fields and extensions
    Modern certificates include extensions that define how a certificate may be used. I will examine critical X.509 extensions including Subject Alternative Name (SAN), Extended Key Usage (EKU), Key Usage, and Basic Constraints.
    Once a system knows who you are, what are you allowed to do?
Digital certificates are not permanent. Like physical identification credentials, they have a lifecycle that includes issuance, expiration, renewal, and revocation. Understanding the certificate lifecycle is essential for maintaining secure systems and preventing authentication failures in real-world PKI environments.
	The certificate lifecycle describes the full lifespan of a certificate. Managing this lifecycle is critical for security and operational reliability in PKI systems.
Authentication and Trust Verification
	Public Key Infrastructure is designed to allow systems to verify digital identities before granting access.
	In PKI modern systems, authentication uses digital certs and cryptographic keys. 


## 4. Common Misconceptions

- That there is a grace period for the validity period. Once the certification expires, that cert is dead and so is the trust tied to it.
- A certification is only allowed to do what it is configured to do via the certificate exentions. Even if all other fields are valid.


## 5. Where This Shows Up

- Web security
- Internal enterprise systems
- Cloud environments
- DevOps workflows


## Mental Model

Short summary that ties the week back to:

Identity + Trust + Verification
