# SOC Incident Investigation Report

**Incident Type:** Phishing Attempt with Malicious URL

**Classification:** True Positive

**Severity:** Critical

---

## Executive Summary

A security alert was triggered involving a suspected phishing email containing a malicious URL. Investigation and cross-referencing of key artifacts via Open Source Intelligence (OSINT) confirmed that the infrastructure used in this attack is tied to known malicious operations. The incident has been classified as a **True Positive**, and immediate mitigation controls have been enforced to protect organizational assets and user credentials.

---

## Artifact Analysis & Technical Walkthrough

The investigation focused on three critical artifacts extracted from the raw log data:

### Key Artifacts Evaluated

* **Source IP (`src_ip`):** `198.51.100.22`

* **Email Sender (`email_sender`):** `noreply@secure-bank.com`

* **Destination URL (`url`):** `[http://secure-bank.com/login](http://secure-bank.com/login)`


### Artifact Evaluation

* **Source IP (198.51.100.22):** OSINT reputation checks revealed this IP address has been reported **500 times** for hosting active phishing campaigns. This high volume of historical abuse provides strong, undeniable evidence of malicious intent.


* **Malicious URL ([http://secure-bank.com/login](https://www.google.com/url?sa=E&source=gmail&q=http://secure-bank.com/login)):** Although the URL visually mimics a legitimate financial institution, it is actively flagged in OSINT databases for credential harvesting and phishing activities. The domain spoofing tactic is designed to deceive targets into trusting the destination site.


* **Email Sender (noreply@secure-bank.com):** While the sender address appears legitimate at first glance, domain spoofing is trivial. Because email headers can be forged, this artifact was marked as a lower priority ("critical: false") and did not override the malicious indicators found on the IP and URL.



---

## Verdict Reasoning

The alert is classified as a **True Positive** based on the following structured chain of evidence:

1. **Corroborated Infrastructure Threat:** The sending IP address possesses a heavily documented history of adversarial phishing activity.


2. **Confirmed Malicious Destination:** The embedded link is independently flagged in global threat intelligence databases as a phishing landing page.


3. **Spoofing Discrepancy:** The seemingly safe email sender domain is heavily outweighed by the malicious reputation of the underlying delivery infrastructure (`src_ip`) and destination payload (`url`).



> **Note on False Positive Criteria:** This alert would only be considered a false positive if the source IP possessed a clean reputation and the URL was verified as completely legitimate across multiple trusted threat intelligence sources.
> 
> 

---

## Response & Containment Actions

To minimize organizational risk and contain the threat, the following defensive actions were executed:

* **`block_ip` (Infrastructure Block):** The source IP (`198.51.100.22`) has been blocked at the perimeter firewall to prevent further inbound malicious communications.


* **`block_url` (URL Filtering):** The malicious URL (`[http://secure-bank.com/login](http://secure-bank.com/login)`) has been added to the enterprise web proxy and endpoint URL filters to block user access and prevent credential theft.


* **`reset_credentials` (Account Remediation):** As a proactive safeguard, the target user's corporate credentials have been revoked and forced to reset to prevent potential unauthorized access in the event they interacted with the link.



---

## Analyst Pro Tips for Future Triage

1. **Multi-Source IP Verification:** Never rely on a single threat intelligence feed. Use multiple OSINT tools to verify IP reputation thoroughly before dismissing or validating an alert.


2. **Look Past Legitimate Appearance:** Look beyond surface-level domain names. Analyze the broader context, creation history, and database flags of URLs that mirror trusted brands.


3. **Cross-Reference Headers:** Do not take the `From:` field at face value. Always cross-reference the sender address with underlying technical artifacts (IP, SPF/DKIM/DMARC alignment) to verify authenticity.
