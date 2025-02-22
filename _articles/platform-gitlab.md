---
title: "GitLab"
description: "GitLab Setup"
layout: article
category: Platform
redirect_from: /articles/gitlab.html
---

# Introduction

Login.gov's GitLab is an integrated Source Code Management (SCM) and Continuous Integration (CI)/Continuous Deployment (CD)
solution customized for our needs and built with FedRAMP moderate controls to provide low friction and
remove many of the roadblocks we faced when trying to piece together multiple tools with varying
authorization levels.

# User Guide

Notes below are Login.gov specific and aim to get engineers and others started
using https://gitlab.login.gov

See [Learning More About GitLab](#learning-more-about-gitlab) for a list of
documentation and training resources.

## Getting Support

Login.gov's Platform Teams support the GitLab service.  For help from an on-call
platform engineer you can Slack a question in `#login-devops` and `@login-devtools-oncall`

For general GitLab support you can also directly use GitLab support.
See [GitLab Licensing and Support](https://github.com/18F/identity-devops/wiki/GitLab-Ultimate-Licensing-and-Support)

## Getting an Account

Accounts are provisioned in code by the Login.gov Platform Team.  In general
this will be done as part of on-boarding.

If you do not have an account or need to change your group membership in
GitLab ask for help in the `#login-devops` channel and `@login-devtools-oncall`

## Logging In

Prerequisites:
* You MUST use secure.login.gov with your official duty GSA email address to sign in
* If you don't yet have an account on secure.login.gov, go ahead and make one (or be
  ready to create it when you first login to GitLab)
* You must use a phishing resistant multi-factor option for MFA which is one of:
  * PIV or CAC
  * Security Key (WebAuthN with a hardware key)
  * Face or Touch Unlock (Platform Authenticator)

To log in:
* Go to <https://gitlab.login.gov>
* Click "Log in Using Login.gov"
* Sign in with your official duty email address
* Multi-factor authenticate with a security key, face/touch unlock, or PIV

Note - If `secure.login.gov` is not available, existing Personal Access Tokens
continue to function.  We also have break-glass procedures if needed.
See [Runbook: GitLab Access Contingency Plan](https://github.com/18F/identity-devops/wiki/Runbook:-Gitlab-Access-Contingency-Plan)

## Personal Access Tokens

## Cloning A Repository

## Creating A New Repository

## Working With Jobs

## Learning More About Gitlab

GitLab has robust documentation, all available at [docs.gitlab.com](https://docs.gitlab.com/)

Login.gov will be providing opportunities for in depth GitLab training.

# Platform Guide

This section is for Platform Engineers and others supporting the underlying
GitLab infrastructure.  Non-public details are omitted.

## Getting Support from GitLab

See [GitLab Licensing and Support](https://github.com/18F/identity-devops/wiki/GitLab-Ultimate-Licensing-and-Support)

## Authentication Setup

GitLab leverages OmniAuth to allow users to sign in using a variety of sersvices, including Login.gov (via SAML). To configure this:

1. Generate a cert and private key by following the instructions at <https://developers.login.gov/testing/#creating-a-public-certificate>:
```
openssl req -nodes -x509 -days 365 -newkey rsa:2048 -keyout private.pem -out public.crt
```

1. Grab the IDP sandbox signing certificate from <https://developers.login.gov/saml/> and get its fingerprint:
```
curl -s https://idp.int.identitysandbox.gov/api/saml/metadata2021 \
| xml sel -N x="http://www.w3.org/2000/09/xmldsig#" -t -v '(//x:X509Certificate)[1]' \
| sed '1i\
-----BEGIN CERTIFICATE-----
' \
| sed '$a\
-----END CERTIFICATE-----
' \
| fold -w 64 \
| openssl x509 -noout -fingerprint \
| sed -E 's/.*=//'
```

1. Copy the IDP cert fingerprint, generated certificate, and generated private key to the per-enviroment S3 secrets bucket. Name them `saml_idp_cert_fingerprint`, `saml_certificate` and `saml_private_key`, respectively:
```
aws s3 cp - "s3://${SECRET_BUCKET}/alpha/saml_private_key" --no-guess-mime-type --content-type="text/plain" --metadata-directive="REPLACE"
    [...]
```

1. With the public cert generated above, and replacing `$ENVIRONMENT`, configure a test integration at http://dashboard.int.identitysandbox.gov with the following parameters:
  - Issuer: `urn:gov:gsa:openidconnect.profiles:sp:sso:login_gov:gitlab_$ENVIRONMENT`
  - Return to App URL: `'https://gitlab.$ENVIRONMENT.gitlab.identitysandbox.gov'`
  - Level of Service:  `Authentication Only` (*Formerly labeled `IAL1`)
  - Default Authentication Assurance Level (AAL): `AAL2 + Phishing-Resistant MFA`
  - Attribute_bundle: `email`
  - Identity Protocol: `saml`
  - Assertion Consumer Service URL: `'https://gitlab.$ENVIRONMENT.gitlab.identitysandbox.gov/users/auth/saml/callback'`
  - SAML Assertion Encryption: `'aes256-cbc'`
