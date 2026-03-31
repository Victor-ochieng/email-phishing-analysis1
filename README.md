# email-phishing-analysis1

# Phishing Email Analysis – Microsoft Account Alert

## Overview

I received an email that claimed to be from Microsoft, warning about unusual sign-in activity on an account. At first glance, the message looked legitimate. The wording and structure closely resembled real security notifications.

However, a closer look at the sender details raised a few concerns. Some of the domains did not match what I would expect from a legitimate Microsoft email. Rather than ignoring it, I decided to analyze the message in detail using the raw `.eml` format.

The objective of this investigation is to determine whether the email is malicious, identify any indicators of compromise, and understand the infrastructure behind it.

Even at this stage, there are inconsistencies in how the email presents itself. The sender claims to represent Microsoft, but the domains involved do not align with legitimate Microsoft infrastructure.

To keep the analysis structured and verifiable, I extracted the key fields from the email and documented them separately. This makes it easier to review the critical elements without going through the entire raw message.

A structured summary of the extracted email fields is available below:

* [View extracted email summary](email-summary.txt)

In addition to the extracted fields, the full email sample is included to allow complete inspection of the message in its original form. This makes it possible to review all headers, content, and embedded elements without relying only on summarized data.

* [Download full email sample](email-sample.eml)


# SPF, DKIM & DMARC Analysis

## Introduction

When analyzing emails for authenticity, the first step is to verify if the sending domain has proper **SPF, DKIM, and DMARC** records. These are authentication mechanisms designed to prevent spoofing and phishing.

- **SPF (Sender Policy Framework):** Checks if the sending server is allowed to send emails on behalf of the domain.
- **DKIM (DomainKeys Identified Mail):** Uses cryptographic signatures to verify the content wasn’t altered in transit.
- **DMARC (Domain-based Message Authentication, Reporting & Conformance):** Combines SPF and DKIM results and instructs receiving servers how to handle emails failing checks.

---

## How it Applied to Our Email Sample

In our case:

- **SPF:** Not set or failed (`thcultarfdes.co.uk` did not authorize IP `89.144.44.2`).
- **DKIM:** Not signed.
- **DMARC:** Permerror (domain `access-accsecurity.com` has a misconfigured or missing DMARC policy).

This is a strong indication the email is **malicious** or part of a **phishing campaign**, despite claiming to be from Microsoft.

---

## Evidence Screenshot

The following screenshot confirms the SPF and DMARC failures for this email:

![SPF and DMARC check results](screenshots/spf-check.png)

# IP Analysis

## Introduction

After verifying email authentication, the next step is to analyze the **sending IP**. An IP check can reveal whether the sender is associated with spam, malware, or phishing campaigns. This helps us determine if the email is part of a larger threat.

For this email, the **sender IP was `89.144.44.2`**, which does not match the legitimate Microsoft sending servers.

---

## Analysis

- The IP is located in an unrelated region compared to the claimed Microsoft source.
- VirusTotal or other threat intelligence services may flag the IP as **malicious** or previously used in phishing attacks.
- Reverse DNS or PTR lookup can show inconsistencies with the domain it claims to represent.
- Cross-referencing with email headers confirms the server path is suspicious and unusual for genuine Microsoft notifications.

---

## Evidence Screenshot

The following screenshot shows the results of our IP investigation:

![IP analysis results](screenshots/ip-analysis1.png)
![IP analysis results](screenshots/ip-analysis2.png)


# Indicators of Compromise (IOCs)

## Introduction

Indicators of Compromise (IOCs) are traces or artifacts that reveal suspicious activity targeting a system or user. In this investigation, the email sample provided several key IOCs that helped uncover the phishing attempt and assess its potential impact.

---

## Key IOCs

1. **Suspicious Email Addresses**
   - `no-reply@access-accsecurity.com` – The sender address pretends to be from Microsoft.
   - `solutionteamrecognizd02@gmail.com` – The reply-to address uses an unrelated domain.
   - `bounce@thcultarfdes.co.uk` – The return-path is suspicious and does not match legitimate Microsoft servers.

2. **Malicious URL**
   - `<img alt="" src="http://thebandalisty.com/track/...">`  
     This hidden tracking pixel confirms active email addresses and can be used to deliver payloads.

3. **Sender IP**
   - `89.144.44.2` – This IP is flagged in the investigation (details are discussed in the IP Analysis section).

---

## Evidence Screenshot

The screenshot below shows the artifacts collected from the email. It highlights the key indicators that made this campaign suspicious:

![IOC evidence](screenshots/ioc-evidence.png)

# URLs and Payloads

## Introduction

Malicious emails often include links or embedded content designed to compromise recipients. In this case, the email contained a hidden URL inside an image tag. While subtle, this URL functions as a tracking pixel and could also be used to deliver malicious content. Understanding how these elements work helps reveal the attacker's strategy and potential risks.

---

## Suspicious URL

- **Hidden Tracking Pixel**  
  ```html
  <img alt="" src="http://thebandalisty.com/track/...">

  ![Embedded Link](screenshots/link.png)






