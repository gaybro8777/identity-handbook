---
title: "GPO Designated Receiver"
description: "How we verify that USPS/GPO address verification is working as expected"
layout: article
category: Team
---

## Intro

The [U.S. Government Publishing Office](https://www.gpo.gov/) sends letters on behalf of Login.gov as part of the identity proofing process. Users who cannot or do not want to verify their address with phone records have the option of receiving a physical letter that contains a code generated by Login.gov. The GPO handles the logistics of printing the letter, putting it in an envelope, and mailing it.

## How and Why?

It's difficult to automate testing of the United States Postal Service and the U.S. Government Publishing Office, so to ensure Login.gov is sending letters as expected, a Login.gov team member is sent a letter every day. We record the day the letters were sent and received along with a few other attributes in a [spreadsheet](https://docs.google.com/spreadsheets/d/1fgRrwNk5GJZbs68Y9JFa4WbmH5OAUKkVrfjNetEh1TY).

## What should I expect and how do I do it?

It takes 5-7 business days for letters to arrive. It is common to not receive any for a few days, and then receive multiple in one day. When you receive a letter, the things to record are:

* The date it was delivered
* The date it was printed
* The date it was enqueued
* The date it was postmarked (not always present)

## Sample Envelope

![GPO envelope with arrow pointing to postmark date]({{site.baseurl}}/images/gpo_envelope.jpg)

## Sample Letter

![GPO letter with arrows pointing to enqueued and printed dates]({{site.baseurl}}/images/gpo_letter.jpg)

## I want to do this!

If you'd like to volunteer to receive and record letters, don't hesitate to post in #login-appdev or add an agenda item to the [Engineering Huddle](https://docs.google.com/document/d/1g_V2vlT79tScBKIVw7M4UXf15TrG69iUrLHE0yE1GGI/). The information is stored in the secrets storage under `gpo_designated_receiver_pii` and requires the keys `first_name`, `last_name`, `address1`, `city` `state` and `zipcode`.
