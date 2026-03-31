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

> **Tip:** Keep screenshots clear, highlight the IP, SPF result, and DMARC action.

