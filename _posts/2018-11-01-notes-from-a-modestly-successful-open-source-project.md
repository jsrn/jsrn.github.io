---
layout: post
title: "Notes From A Modestly Successful Open Source Project"
description: "A small project I made got way more attention than I expected and I had to learn some things about being the maintainer of an open source project."
category: programming
tags: [programming, open source]
---

About a week ago, I built [this](https://howoldisit.glitch.me/) as an excuse to test out [create-react-app](https://github.com/facebook/create-react-app). I was expecting to get a few half-hearted chuckles out of my friends and then it would be immediately forgotten.

But not this time. This time I decided to post it on [dev.to](https://dev.to) in an attempt to get a half-hearted chuckle out of *strangers.*

In the process, I learned a thing or two about being an open source maintainer.

## Make The Contributing Guidelines Clear

Almost immediately, I had four pull requests. Yay! For the same issue. Whoops. What I had failed to do was make the contribution process clear. Rather than establishing a protocol for how to claim an issue as your own, I just assumed that things would work out. The result was having to awkwardly thank a few people, but tell them that their contribution wasn't needed.

You can make the contributing guidelines clear either in your `README` or by creating a `CONTRIBUTING` file in the root of your repository. You should detail things like the process for claiming an issue as your own, expectations for what makes a merge-worthy patch, and standards for behaviour. By making these expectations clear up front, you can avoid mishaps like I encountered.

## Don't Be Afraid To Tell People

I was reluctant to show anyone what I had made because I was pretty sure that I am not as funny as I think I am. However, I'm glad that I took the step of posting it under the `#showdev` tag, because it turns out a lot of people really enjoyed the idea and it lead to (and is still leading to) a lot of great contributions from the development community.

## Communities Boost Signals

![the official twitter post](/assets/images/successful-project/twitter-post.PNG)

This is an extension of the previous point, but if at all possible, find a community to be a part of. I don't want to sound like it's all about cynically exploiting a captive audience to build your *~personal brand~*, but if you want people to see and comment on things you have made, then a community like [dev.to](https://dev.to) is a great thing to be part of. I have tried creating things and then just casting them into the uncaring void, hoping for interactions, but it doesn't work.

## People Want To Help

At the time of writing, the repository has a fair number of pull requests.

![a lot of pull requests!](/assets/images/successful-project/pull-requests.PNG)

There was a brief period where I was merging them and the new pull requests were coming in *faster than I could merge them and thank people.* I noticed I was getting a lot of repeat business from certain people, so I added them as collaborators to the project. I expected that they would just use this new power to merge their own changes into the master branch, having established that they knew what sort of changes were expected, but no. My new colleagues were quick to set about reviewing and merging pull requests from others, discussing changes, testing proposed features.

If you have a project that people are interested in, and you show people that trust by giving them write access to the repository, more often than not, they will rise to the occasion in a bigger way than you expect.

## Take Advantage Of GitHub's Automatic Replies

![GitHub's saved reply feature](/assets/images/successful-project/git-replies.png)

If you are saying the same thing over and over again, do what programmers ought to do: automate. It feels a little disingenuous, attempting to convey heartfelt thanks with a form letter, but it will save you a lot of time and energy.

## Thank People

The final and most important point. If you have released a project that people are spending their precious time and attention on, thank them. If they contribute code, thank them. If they smugly point out a spelling error, thank them. Because of the work of the **76 people** whose contributions now far outweigh my own, "How Old Is It?" went from six or seven technologies to over 100, complete with project links and icons.

That's pretty special.