---
title:    Amazon Web Services (AWS)
category: cloud
---

Almost everything digital that I build, maintain and operate is hosted on Amazon
Web Services (AWS).

This is because:

* They're widely considered to be the gold standard of cloud providers, which
  means that there's likely to be a service for whatever I want to do
* Because they manage the underlying infrastructure, I can focus less on that
  and more on what I'm actually building
* In turn, I only have to pay for what I use when I need it, rather than paying
  for resources that I have access to all of the time but that I don't use

## Access

I access AWS day-to-day through either the [AWS Console][console] or the
[AWS CLI][cli]. However I sign in, I use SSO via Google, which is configured via
[IAM Identity Center][iam-identity-center].

I can sign in with either:

* My day-to-day Google account, which gives me read and write access to services
  that I use regularly
* My administration Google account, which gives me full access to my AWS account
  except some root-only operations

However, I also have two other ways of getting in that deliberately don't use
SSO:

* My break-glass IAM user, which gives me limited access to security and
  identity services, so that I can get in if SSO isn't working
* My root credentials, which I only use [for things][iam-root-only] that only
  that account can do

Given how important my AWS account is to me and the value that it'd have to a
hostile actor, I have:

* Single sign-on in place for most accounts that can sign into AWS, which lets
  me focus on securing one door in rather than multiple
* Multi-factor authentication required for every account that can sign into AWS,
  including hardware tokens where my threat model indicates it's necessary
* Alerts when any of my non-day-to-day accounts are used to sign into AWS, with
  settings to ensure that those alerts both reach me and interrupt me, no matter
  where I am or what I'm doing
* Logs that record every time that my AWS account is signed into and what's done
  in my account, which I keep for at least a year

## Configuration

### Account structure

I only use a single AWS account.

Whilst AWS [recommends using multiple accounts][multiple-accounts], I've decided
that not doing so is an acceptable trade-off for me.

If I had multiple accounts, I'd have to:

* Set each account up with a consistent baseline for security and monitoring,
  and, whilst I can - and do - do that using [infrastructure-as-code][iac], it's
  inevitable that there's more chance of something going wrong across a dozen
  accounts than there is with one
* Consolidate bills across multiple accounts so that I know how much I'm paying
  and what I'm actually paying for, which is something that I have very little
  margin for error on, given that I'm one person rather than a company making
  large profits
* Pay for AWS Support for each account that I might need support with, because
  [only AWS Enterprise Support][support-billing] can be shared between accounts,
  which would become prohibitively expensive very quickly

However, I have set up an [AWS Organizations][organizations] organisation that
contains my account. This is because I can't use
[AWS Security Incident Response][security-ir] without having an organisation.

In the future, I might create one account per environment. This would stop me
from accidentally destroying resources for a workload in production whilst I'm
developing or vice-versa.

## Help and support

I pay for [AWS Business Support+][support-plan] which provides me with access to
their support team for help with my AWS account, their products and services,
and [supported third-party platforms and applications][support-third-party], 24
hours a day, 7 days a week.

If I can't work something out myself, or it's something that only AWS can do,
such as raising [service quotas][quotas], I'll therefore open a case with them.
They'll then respond in writing via either live chat or their online portal, or
by calling me if my case is urgent.

To open a case, I'll follow the instructions in the
[AWS Support User Guide][support-case-instructions].

As well as access to their support team, the plan also gives me:

* [Trusted Advisor][trusted-advisor], which gives me automatic checks and
  recommendations to improve my use of AWS
* [Security Incident Response][security-ir], which gives me access to security
  specialists from AWS when responding to or recovering from a security incident
* Architectural guidance, which provides me with access to engineers who can
  advise me on how best to use AWS for whatever I'm trying to achieve

### Service-level agreement

AWS have said that they'll respond to my support cases in:

* 30 minutes or less, if one of my mission-critical systems is down
* 1 hour or less, if one of my production systems is down
* 4 hours or less, if one of my production systems is impaired but not down
* 12 hours or less, if one of my non-production systems is down or impaired
* 24 hours or less, for anything else

[cli]:                       https://docs.aws.amazon.com/cli/
[console]:                   https://connorgurney-production.signin.aws.amazon.com/console
[iac]:                       /content/knowledge/infrastructure-as-code.md
[iam-identity-center]:       https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html
[iam-root-only]:             https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html#root-user-tasks
[multiple-accounts]:         https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html#aws-accounts
[organizations]:             https://docs.aws.amazon.com/organizations/
[quotas]:                    https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html
[support-billing]:           https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidatedbilling-support.html
[support-case-instructions]: https://docs.aws.amazon.com/awssupport/latest/user/case-management.html
[support-plan]:              https://aws.amazon.com/premiumsupport/plans/business-plus/
[support-third-party]:       https://aws.amazon.com/premiumsupport/third-party-applications/
[security-ir]:               https://docs.aws.amazon.com/security-ir/
[trusted-advisor]:           https://aws.amazon.com/premiumsupport/technology/trusted-advisor/
