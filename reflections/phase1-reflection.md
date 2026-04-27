# Phase 1 Reflection Project

Choose your submission format:
- [X] Written reflection (500–800 words)

## Part 1 — Your Phase 1 Journey

From Week 1 to now, my understanding of PKI has shifted from seeing it as a background security function to recognizing it as a critical component of how trust is established and maintained across systems. 
Initially, I believed PKI was primarily a responsibility owned by security teams. Based on my experience working on a platform team, we interacted with certificates mainly during rotation events, so I viewed PKI as a task handled by security rather than something deeply tied to platform, development, or any other team.
What surprised me most is I now understand that PKI is not just about issuing certificates, but about managing trust at scale, including certificate lifecycles, validation, and failure scenarios. 
This shift has helped me see PKI as a shared responsibility across security, platform, and infrastructure teams, rather than a isolated function.



## Part 2 — How the Pieces Connect
Phase 1 concepts—cryptographic foundations, certificates, trust, lifecycle, troubleshooting, and real-world deployment—build on one another to form a complete PKI system rather than isolated topics.

At the foundation, cryptographic foundations (such as key pairs and encryption models) establish how secure communication is possible. Certificates then utilize these foundations by binding identities to public keys. This leads into trust, where certificate chains and Certificate Authorities (CAs) validate that identities can be relied upon.

From there, lifecycle management becomes critical —certificates must be issued, renewed, rotated, and eventually expire, making validity windows and automation essential to maintaining trust over time. When failures occur, troubleshooting ties directly back to each layer, requiring analysis of certificate chains, TLS handshakes, and validation paths to identify root causes.

Finally, real-world deployment brings all these elements together, showing how PKI operates across infrastructure—whether at load balancers, application servers, or CDNs—impacting how services securely communicate in production environments.

Understanding how each of these concepts connects has helped me move beyond isolated knowledge and toward a system-level view of PKI architecture and its role in maintaining secure, reliable communication.


## Part 3 — A Lab That Made It Real

Lab referenced: `lab/week-05/submissions/expiration-stretch/lab-03-expiration-stretch.md`

In this lab I created a short-lived certificate and read its validity window, generate an expired certificate to simulate a real-world expiration event, utilize openssl x509 -checkend to programmatically detect expiration, observe what an expired certificate looked like when validated, and practice the full replacement workflow: generate a new key, new CSR, new certificate.



## Part 4 — Explaining PKI to Someone Who Doesn't Know It

PKI isn’t just a security layer—it’s the trust infrastructure behind every secure system interaction. Certificates are only the visible piece; behind them is the process of identity verification, trust chaining, and lifecycle management that determines whether systems can communicate at all. When PKI fails, it doesn’t degrade quietly—it causes outages, breaks trust, and can expose systems to risk. That’s why managing PKI has to be intentional, not reactive.

## Part 5 — Where You Go From Here

As we move into Phase 2, the roles that interest me are Cloud Security, Site Reliability Engineer, and Cybersecurity Engineer. Those roles feel most revelant to where I want to go.  What I would like to understand better is both the diagnostic framework and the remediation path.


## Required Visual
- A complete certificate lifecycle from key generation through revocation

Visual file: `reflections/visual/phase1-diagram.png`

This diagram depicts a certificate's lifecycle. The diagram will follow the flow of the the lifecycle from key generation through revocation through annotation.


## Second Lab Reference

*Reference at least one more lab — different from Part 3. What did you do and what did it reinforce?*

Lab referenced: `labs/week-04/submissions/install-validate/lab-03-install-and-validate-stretch.md`

 In this lab, I experience hands-on experience with the full certificate installation and trust validation workflow. I generated a self-signed root CA certificate, installed it into my operating system's trust store, create a certificate signed by that CA, validate that your system now trusts the signed certificate, and remove the certificate cleanly after the lab. It reinforces the importance of a root CA installation in a trust store. Any certificate signed by that root will be trusted. This is imperative to know as a professional at the enterprise level to understand how to install root CAs on employee devices. 
