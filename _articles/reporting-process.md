---
title: "Reporting Process"
description: "Reporting process for ad-hoc data requests, query requests and analyses"
layout: article
category: "Reporting"
---

## Requesting

If you have a data question about login.gov data, post it in [`#login-reporting`](https://gsa-tts.slack.com/archives/C5E7EJWF7). Make sure to include key information such as:

- What is the data you are requesting? What question are you trying to answer?
- When do you need this information by?

Once that happens, someone from the data team will acknowledge and file your question as a JIRA ticket,
which will be assigned to a sprint. Once the data has been collected, a summary will be posted to
that Slack thread.

## Examples

- [Example data request](https://docs.google.com/document/d/1rU_70Cp_b2rx-edFDuqLfmTyj0VukkQ4s12DpRl6Dgc/edit#heading=h.f8nspscwbzl2)
- [Example data request (data)](https://docs.google.com/spreadsheets/d/1V9rG8Tdfjzw1cwKkTRWl_bRRcKljYltK2_ELBhIxNQo/edit#gid=0)

## Process

When pulling data for a query, follow this process to help us log requests, and track how we
answered them so we can build on them in the future.

1. Create a Google Doc in the [Data Request Google Drive folder](https://drive.google.com/drive/folders/1wzIwovMQWL2PFrNIJNL6DaV_TqbE7yqO). Optionally, upload supporting data as a spreadsheet **in addition** to the Google Doc with the same JIRA ticket in the name. Make sure to link to the spreadsheet from the document.

   Use this format for naming the documents:

   - LG-1234: Agency XYZ password resets *for the document*
   - LG-1234: Agency XYZ password resets (data) *for the spreadsheet*

2. Make sure to include key information in this document:
   - The JIRA ticket in the title
   - Brief summary of the request
   - Brief summary of the result
   - Any queries (Cloudwatch queries, SQL queries) and any code snippets used to process those results

### Example Document Template

> **LG-1234: Agency XYZ password resets**
>
> **Request**
>
> What is the password reset rate at agency XYZ?
>
> **Result**
>
> *A quick summary of the results in a sentence or two*
>
> Agency XYZ's password reset rate is about the same as the average across all agencies. This
> trend holds across mobile and desktop devices.
>
> *A tabular summary of the data*
>
> | Event | Overall population | Agency XYZ |
> | ----  | ------------------ | ---------- |
> | Password reset (mobile) | 5% | 5% |
> | Password reset (desktop) | 4% | 4% |
>
> **Queries**
>
> *All the Cloudwatch queries used to gather the data*
>
> ```cloudwatch
> fields @message, @timestamp
> | filter name = "Password Reset: Password Submitted"
> ```
