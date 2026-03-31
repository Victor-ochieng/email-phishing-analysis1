# email-phishing-analysis1
DFIR investigation of a phishing email impersonating Microsoft

# Phishing Email Analysis – Microsoft Account Alert

## Overview

I received an email that claimed to be from Microsoft, warning about unusual sign-in activity on an account. At first glance, the message looked legitimate. The wording and structure closely resembled real security notifications.

However, a closer look at the sender details raised a few concerns. Some of the domains did not match what I would expect from a legitimate Microsoft email. Rather than ignoring it, I decided to analyze the message in detail using the raw `.eml` format.

The objective of this investigation is to determine whether the email is malicious, identify any indicators of compromise, and understand the infrastructure behind it.

