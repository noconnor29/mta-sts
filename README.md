# MTA-STS Policy Management on Github
[![Update Policy File](https://github.com/noconnor29/mta-sts/actions/workflows/update-mta-sts-policy.yml/badge.svg)](https://github.com/noconnor29/mta-sts/actions/workflows/update-mta-sts-policy.yml) [![Update DNS Record](https://github.com/noconnor29/mta-sts/actions/workflows/update-mta-sts-record.yml/badge.svg)](https://github.com/noconnor29/mta-sts/actions/workflows/update-mta-sts-record.yml)

## Purpose of This Repository

This repository is configured to automatically update and maintain [MTA-STS](https://datatracker.ietf.org/doc/html/rfc8461) (Mail Transfer Agent Strict Transport Security) policy file and DNS record. It:

1. Fetches the latest MTA-STS policy each day from my mail provider, [Proton](https://proton.me/mail).
2. Updates the policy file stored at .well-known/mta-sts.txt in this repository.
3. Commits and pushes changes back to the repository to publish the new policy.
4. Updates the _mta-sts TXT record so senders will refresh their policy cache.

### Check Your Domain
1. https://www.hardenize.com/
2. https://www.mailhardener.com/tools/

## What is MTA-STS?

MTA-STS is a protocol that helps ensure secure transmission of emails between mail servers. It:
1. Requires that sending mail servers validate the receiving server's identity via TLS certificates.
2. Enforces the use of TLS encryption for email delivery, preventing downgrade attacks or MITM (man-in-the-middle) attacks.

### Key Components of MTA-STS:
**DNS Records**
1. TXT Record (required) - `_mta-sts.<your-domain>` points to your domain's policy.
2. TXT Record (recommended) - `_smtp._tls.<your-domain>` to configure reporting through [TLS-RPT](https://datatracker.ietf.org/doc/html/rfc8460).
3. A/CNAME Record (if needed) - points `mta-sts.<your-domain>` to your policy file.

**HTTPS Policy File**: A text file hosted at `https://<your-domain>/.well-known/mta-sts.txt`, which defines the rules for your domain’s mail servers.

### Example MTA-STS Policy for Proton Mail:
```plaintext
version: STSv1
mode: enforce
mx: mail.protonmail.ch
mx: mailsec.protonmail.ch
max_age: 604800
```
This record indicates that a domain requires MTA-STS be enforced (cert validation and connection encryption) and that only certificates for the named MX systems should be accepted.
