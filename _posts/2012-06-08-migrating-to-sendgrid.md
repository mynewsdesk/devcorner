---
title: Migrating to Sendgrid
author: Kristian Hellquist
layout: post
categories:
  - Uncategorized
---
Mynewsdesk sends a lot of email. Transactional emails (signup emails, activation of account, payments etc), emails from our users to their own network and alerts to journalists, when a company publish a pressrelease.

**Issues with current setup**

One common question from customer service is about status for an email delivery. Until now we have solved this by manually looking in the mail logs of sendmail, which could be a bit tedious and something that customer service can’t do by themselves. We didn’t have any nice statistics overview as well with open-rate, click-rate and so on.

**Moving to Sendgrid**

[Sendgrid][1] is a Software as a Service (SaaS) platform that provides a complete solution for sending emails and getting real-time statistics about the deliveries. It also provides an [API][2] where you for example can use their Event API to call your application for status about each mail you send.

With Sendgrid any person can log in and see general stats, bounce reports and status about a certain delivery. You can’t see the contents in the mails though, you have to use their BCC-app if you also wants to see the contents for each mail.

![Stats for Forgot Password emails](/images/wp/sendgrid-stats1.png "Stats for Forgot Password emails")
Stats for Forgot Password emails

![Bounce stats](/images/wp/sendgrid-bounce-filtered.png "Bounce stats")
Bounce stats

![Searchable email activity](/images/wp/sendgrid-activity-filtered.png "Searchable email activity")
Searchable email activity 

**Integration**

When migrating to Sendgrid we didn’t move all emails at once. We started by categorizing all our outgoing mails and move a category at a time to sendgrid. Using sendgrid is very easy, you just update your smtp-settings to point to your sendgrid account and voilà – integration is completed. You can also tag your emails with sendgrid categories to be able to get statistics for a group of emails.

Here is our integration [code][3] with sendgrid. (Thanks Joakim Kolsjö and Henrik Nyh with idea of how to find calling mailer action!)

 [1]: http://sendgrid.com/ "Sendgrid"
 [2]: http://docs.sendgrid.com/documentation/api/web-api/ "API"
 [3]: https://gist.github.com/2894535 "Ruby code"