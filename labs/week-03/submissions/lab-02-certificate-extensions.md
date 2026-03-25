
# Lab 02 — Investigate Certificate Extensions

## Overview
In this lab, I will be expanding on my investigation of X.509 certificates and the extensions defined inside of them. Those extensions will assist me with my understanding of a certificate's usage and trust validation.


---

## Environment
- Operating System: macOS
- Terminal Used: Integrated terminal in Visual Studio Code (zsh shell)
- OpenSSL Version (if applicable): LibreSSL 3.3.6



## Steps Performed
Summarize the key steps you performed to complete the lab.
1. Performed the inspection command to observe the leaf_cert.pem in readable text.
2. Located and record certificate extension fields: key usage, extended key usage, SAN, and basic constraint.
3. Record my observations of what those fields are permitted to do in a certificate.




## Extensions Found

### Subject Alternative Name (SAN)
Paste the value from your output:
DNS:*.google.com, DNS:*.appengine.google.com, DNS:*.bdn.dev, DNS:*.origin-test.bdn.dev, DNS:*.cloud.google.com, DNS:*.crowdsource.google.com, DNS:*.datacompute.google.com, DNS:*.google.ca, DNS:*.google.cl, DNS:*.google.co.in, DNS:*.google.co.jp, DNS:*.google.co.uk, DNS:*.google.com.ar, DNS:*.google.com.au, DNS:*.google.com.br, DNS:*.google.com.co, DNS:*.google.com.mx, DNS:*.google.com.tr, DNS:*.google.com.vn, DNS:*.google.de, DNS:*.google.es, DNS:*.google.fr, DNS:*.google.hu, DNS:*.google.it, DNS:*.google.nl, DNS:*.google.pl, DNS:*.google.pt, DNS:*.googleapis.cn, DNS:*.gstatic.cn, DNS:*.gstatic-cn.com, DNS:googlecnapps.cn, DNS:*.googlecnapps.cn, DNS:googleapps-cn.com, DNS:*.googleapps-cn.com, DNS:gkecnapps.cn, DNS:*.gkecnapps.cn, DNS:googledownloads.cn, DNS:*.googledownloads.cn, DNS:recaptcha.net.cn, DNS:*.recaptcha.net.cn, DNS:recaptcha-cn.net, DNS:*.recaptcha-cn.net, DNS:widevine.cn, DNS:*.widevine.cn, DNS:ampproject.org.cn, DNS:*.ampproject.org.cn, DNS:ampproject.net.cn, DNS:*.ampproject.net.cn, DNS:google-analytics-cn.com, DNS:*.google-analytics-cn.com, DNS:googleadservices-cn.com, DNS:*.googleadservices-cn.com, DNS:googlevads-cn.com, DNS:*.googlevads-cn.com, DNS:googleapis-cn.com, DNS:*.googleapis-cn.com, DNS:googleoptimize-cn.com, DNS:*.googleoptimize-cn.com, DNS:doubleclick-cn.net, DNS:*.doubleclick-cn.net, DNS:*.fls.doubleclick-cn.net, DNS:*.g.doubleclick-cn.net, DNS:doubleclick.cn, DNS:*.doubleclick.cn, DNS:*.fls.doubleclick.cn, DNS:*.g.doubleclick.cn, DNS:dartsearch-cn.net, DNS:*.dartsearch-cn.net, DNS:googletraveladservices-cn.com, DNS:*.googletraveladservices-cn.com, DNS:googletagservices-cn.com, DNS:*.googletagservices-cn.com, DNS:googletagmanager-cn.com, DNS:*.googletagmanager-cn.com, DNS:googlesyndication-cn.com, DNS:*.googlesyndication-cn.com, DNS:*.safeframe.googlesyndication-cn.com, DNS:app-measurement-cn.com, DNS:*.app-measurement-cn.com, DNS:gvt1-cn.com, DNS:*.gvt1-cn.com, DNS:gvt2-cn.com, DNS:*.gvt2-cn.com, DNS:2mdn-cn.net, DNS:*.2mdn-cn.net, DNS:googleflights-cn.net, DNS:*.googleflights-cn.net, DNS:admob-cn.com, DNS:*.admob-cn.com, DNS:*.gemini.cloud.google.com, DNS:googlesandbox-cn.com, DNS:*.googlesandbox-cn.com, DNS:*.safenup.googlesandbox-cn.com, DNS:*.gstatic.com, DNS:*.metric.gstatic.com, DNS:*.gvt1.com, DNS:*.gcpcdn.gvt1.com, DNS:*.gvt2.com, DNS:*.gcp.gvt2.com, DNS:*.url.google.com, DNS:*.youtube-nocookie.com, DNS:*.ytimg.com, DNS:ai.android, DNS:android.com, DNS:*.android.com, DNS:*.flash.android.com, DNS:g.cn, DNS:*.g.cn, DNS:g.co, DNS:*.g.co, DNS:goo.gl, DNS:www.goo.gl, DNS:google-analytics.com, DNS:*.google-analytics.com, DNS:google.com, DNS:googlecommerce.com, DNS:*.googlecommerce.com, DNS:ggpht.cn, DNS:*.ggpht.cn, DNS:urchin.com, DNS:*.urchin.com, DNS:youtu.be, DNS:youtube.com, DNS:*.youtube.com, DNS:music.youtube.com, DNS:*.music.youtube.com, DNS:youtubeeducation.com, DNS:*.youtubeeducation.com, DNS:youtubekids.com, DNS:*.youtubekids.com, DNS:yt.be, DNS:*.yt.be, DNS:android.clients.google.com, DNS:*.android.google.cn, DNS:*.chrome.google.cn, DNS:*.developers.google.cn, DNS:*.aistudio.google.com

### Key Usage
Paste the value from the output:
Digital Signature

### Extended Key Usage (EKU)
Paste the value from the output:
TLS Web Server Authentication

### Basic Constraints
Paste the value from the output:
CA:FALSE
---
 
## Observations

1. The domains that appear in the SAN field were attached to a wildcard and google related such as *.google.co.in, DNS:*.google.co.jp, DNS:*.google.co.uk, DNS:*.google.com.ar, DNS:*.google.com.au, DNS:*.google.com.br, DNS:*.google.com.co, DNS:*.google.com.mx, DNS:*.google.com.tr
2. This certificate is authorized for signing operations based on Key Usage.
3. The EKU field tells me that the certificate can authenticate a server.
4. This not a CA certificate. The Basic Constraints is labled "CA:FALSE" in which it cannot issue certificates. Only CA:TRUE is the indicator that a certiicate is CA certificate and not a leaf.
5. SAN matters more than the Subject CN field in modern TLS because SAN allows multiples identies to exists under one cert instead of having separate certs for several names.



## Key Findings
Document the most important observations from the lab.
• Observe the location of the extension fields are listed (after the cert fields). 
• Observe and identify the SAN, key usage, EKU, and basic constraints. 
• Observe the length of the SAN list. Did not expect it to be this size.



## Explanation
- Understanding the function of certificate extensions is crucial to a PKI practitioner because extensions define how a certtificate can be utilized. Misconfguring the capabilities will likely cause outages such as TLS handshake failures or authentication issues.



## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.
- N/A


## Artifacts
List the files generated during this lab.
- lab-02-certificate-extensions.md