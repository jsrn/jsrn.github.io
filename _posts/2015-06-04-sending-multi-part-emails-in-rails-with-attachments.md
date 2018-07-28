---
layout: post
title: "Sending Multi-part Emails in Rails with Attachments"
description: ""
category: programming
tags: [rails, ruby, mailer, multipart, attachments]
---

If ActionMailer detects that you have multiple view formats for your mailer, it will automatically use these templates as a multi-part email.

You may be surprised to find that if you add an attachment to your mail, this is no longer the case.

<blockquote>
	Implicit template rendering is not performed if any attachments or parts have been added to the email. This means that you'll have to manually add each part to the email and set the content type of the email to multipart/alternative.
	<br>
	~ <a href="http://api.rubyonrails.org/classes/ActionMailer/Base.html">http://api.rubyonrails.org/classes/ActionMailer/Base.html</a>
</blockquote>

You can restore the order of things by explicitly stating the templates you wish to use, like so:

{% highlight ruby %}
attachments["attachment.pdf"] = some_file

mail(
  to:      "dave@example.com",
  from:    "alice@example.com",
  subject: "Here's a multi-part email with an attachment!"
) do |format|
   format.html { render "some_template" }
   format.text { render "some_template" }
end
{% endhighlight %}

Piece of cake.