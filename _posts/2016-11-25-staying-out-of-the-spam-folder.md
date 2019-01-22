---
layout: post
title: "Staying Out Of The Spam Folder"
description: ""
category: programming
tags: [mail, spam, dmarc, spf, dkim]
---

If you’re a developer working on some sort of SaaS, it’s almost certain that you’ll want to send emails to your users. Whether you use your own email server or an offering like <a href="https://sendgrid.com/">SendGrid</a>, getting your emails to send can be fairly straight forward.

What nobody told me is that getting your emails out of your server isn’t the hard part. Even if it looks like your emails are happily sending, it’s very easy for them to land in the spam folder, and then you have angry customers asking why their mails aren’t going through.

For the sake of this post, I’m going to pretend we’re using a third party mailer and skip over the nitty gritty of actually setting up a mail server. Even when that is taken care of for you, there is a whole bowl of acronym soup to deal with, and a lot of the documentation I read seemed to cast it almost optional. SPF? DKIM? Whatever. Do this, do that, it may increase your deliverability rates! However, it didn’t really make it clear what your deliverability rates were likely to be before or after the changes. It’s a hassle, and it may be tempting to say <em>"okay, I’ll see how it goes, and set these things up if I have any problems."</em>

What I really wish the guides said was:

<strong>STOP. SET UP SPF. SET UP DKIM. SET UP DMARC. NOW.</strong>

And in the case of SendGrid specifically...

<strong>SET UP WHITELABELLING.</strong>

<a href="https://en.wikipedia.org/wiki/Sender_Policy_Framework">SPF</a> stands for Sender Policy Framework and lets a mail exchange verify that an email came from a host that was authorized to send emails from the sender domain.

<a href="https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail">DKIM</a> allows exchanges to verify that an email claiming to be from a certain domain was authorized by the owners of that domain.

<a href="https://en.wikipedia.org/wiki/DMARC">DMARC</a> allows you to specify a policy for what to do if either SPF for DKIM validations fail.

<a href="https://sendgrid.com/docs/User_Guide/Settings/Whitelabel/index.html">SendGrid whitelabelling</a> allows you to authorize SendGrid to send emails from your own domain name, rather than via SendGrid on behalf of your domain name. It’s an important distinction.

A lot of the documentation I read really understated how important these things are to email deliverability. If you’re serious about having your emails land in the inboxes of your customers, I urge you to take these things seriously too.

It’ll save you a headache.