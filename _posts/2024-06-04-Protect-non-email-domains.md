---
layout: post
title: Protect Your Domains That Don’t Send Email
description: Steps to configure on protecting your domain(s) from being spoofed if they do not send email.
date: 2024-06-04 23:00:00 -0500
tags: Security
---

# Protect Your Domains That Don’t Send Email

## Why Should I Do This?

The main question is why not? If owning a domain is really important to you, you should also put policies/records in place to protect your domain(s) reputation against possible email spoofing. Simply speaking you don’t want someone spoofing your domain and sending emails without your knowledge. 

The following post is also recommended if your domain(s) used to send email but its not required anymore. Such scenarios could be that your business name has changed, company has been acquired, or simply you decide to close your business. 

Here is how you can setup policies/records to help make spoofing more difficult for non-email enabled domains.

Note: Every registrar is different on how records can be setup, please read your registrars documenation on setting up your records.

## SPF Record

Create or update your TXT record and make sure that the following is set:

- Type: TXT
- Host: `@`
- Value: `v=spf1 -all`

What the above does is that it tells email servers that no one is a permitted sender and the `-all` flag states that all emails that fail SPF check should be rejected.

## DKIM Record

Create or update the TXT record for your domain (if you have more than one DKIM record delete all of them and ensure that this is the only DKIM record place):

- Type: TXT
- Host: `*._domainkey`
- Value: `v=DKIM1; p=`

The entry above tells that nothing should be signing any emails against your domain as there is no public key to verify against. The wildcard ensures that any possible variations of a spoofed DKIM record cannot be used against your domain.

## DMARC Record

Create or update the following DMARC record for your domain:

Type: TXT

- Host: `_dmarc`
- Value `v=DMARC1;p=reject;sp=reject;adkim=s;aspf=s`

Here is what the above means:
- `p=reject` - This means that the policy is set to reject emails that fail SPF and DKIM checks.
- `sp=reject` – Like p=reject but for sub domains.
- `adkim=s` – This tells that the DKIM alignment for your domain is set to strict, if the server of the email domain that is not aligned with the “from” header DKIM will fail.
- `aspf=s` – Same as for adkim but for SPF.

### Optional DMARC Options

This step is not required but if you want to report any DMARC failures that dont align with SPF or DKIM use the following switches:

- `fo=1` - This is a "Failure Reporting Option", there are other options (0,1,d or s) but 1 is the most recommended. This means that it'll generate a DMARC fail report if either SPF or DKIM alignment doesnt generate a "pass" result.
- `rua=mailto:mailbox@yourdomain.com` - This tells where to send aggregated email reports to. Ideally its recommended to set it up with a 3rd party provider to congest these emails but you can still send them to any mailbox that can receive email. This field can also accept multiple emails so if you need to add another mailbox you can add a comma and put in another email address (eg. `rua=mailto:user1@yourdomain.com, mailto:user2@yourdomain.com`)

Here is what your DMARC record should look like if you want the optional features in place:

With a Single Mailbox:
`v=DMARC1;p=reject;sp=reject;adkim=s;aspf=s;fo=1;rua=mailto:mailbox@yourdomain.com`

With Multiple Mailboxes:
`v=DMARC1;p=reject;sp=reject;adkim=s;aspf=s;fo=1;rua=mailto:user1@yourdomain.com, mailto:user2@yourdomain.com`

## Optional - Set A Null MX Record

Although this is not required, you can setup a null MX record so that it tells that your domain accepts no mail without providing a mail server.

Its important to know that some registrars do not accept a null MX record. If you cannot do this you can skip this step.

- Type: MX
- Host: [Leave this field empty]
- Priority: 0
- Value: `.`
