---
layout: post
title: "Reuse or Rewrite?"
date: 2013-03-20 18:53
comments: true
categories: 
---

One of the first technical decisions we faced was whether to reuse or rewrite. Fedora represents over twelve years of development effort and there are some fairly [well-known arguments](http://www.joelonsoftware.com/articles/fog0000000069.html) against undertaking a complete rewrite.

However, the challenge of reusing the existing codebase is daunting, and not only for technical reasons. Fedora is not just an open-source project, but it is a community-sourced project. That is, a significant part of Fedora's value proposition is the strength and expertise of its community. That community has driven and sustained Fedora over the years, but the [technical debt](http://martinfowler.com/bliki/TechnicalDebt.html) of Fedora's codebase has become a liability for engaging and growing our community of developers. A strong and engaged developer community is an essential part of a preservation repository's success and sustainability, so our answer to the question of reuse or rewrite must support this goal.

{% pullquote %}
At nine years, I am the longest-serving, active committer on the Fedora project and to this day I find many parts of the codebase difficult to approach, difficult to test, and therefore, difficult to change. {" We are past the era of relying on grant-funded teams of full-time, salaried developers "} working on Fedora. This doesn't appear likely to change. As a repository service, Fedora's relationship to end-users is indirect at best and consequently, the relationship between Fedora development and end-user value is not well understood or appreciated. Indeed, most of the energy in the Fedora ecosystem is now directed at higher-level frameworks (e.g. Hydra) and vertical products (e.g. Islandora), where the relationship between users and developers is more direct. A key part of Fedora's future has to be a modular, extensible codebase that embraces and facilitates community contribution and collaboration in its many and varied forms.
{% endpullquote %}

Two particular problems that I find the Fedora 3.x codebase ill-equipped to handle are: i) scalability and ii) transactionality. Moreover, I find these two problems argue against reuse.

<!-- more -->

Fedora 3.x has repeatedly demonstrated its ability to manage millions of objects on commodity hardware, but its ability to scale to meet significant concurrent read and write requests is quite poor. Fedora 3.x is implemented as a single-node web application and consequently, cannot easily be scaled out or clustered.

Fedora 3.x also does not support transactions in a robust fashion. Introducing a proper transaction architecture would also represent a fundamental sea-change to the codebase. Fedora 3.x supports a limited form of transactions within the scope of a single API call, but nothing like the transaction support modern applications offer.

Removing Fedora's single-node assumption to address scalability represents a fundamental change in the codebase. Introducing a modern transaction framework likely represents an equally significant labor. I believe that addressing either of these two problems would require a dedicated team months of labor to produce a passable solution. Moreover, we would have increased the net amount of code we are responsible for maintaining, and done little to ease or simplify the burden of developing new features.

{% pullquote %}
A more nuanced version of the dilemma of reuse or rewrite introduces {" a third option: reuse and rewrite "}. Namely, build on top of another software stack. The right stack reduces the amount of code we are directly responsible for maintaining, increases the overall effective developer pool, and ultimately increases our ability to focus on the solving the problems and delivering features that bring real value to our community. Put another way, our decision should be the one that positions us best to {" fix the problems, not the code "}.
{% endpullquote %}

And in fact, the strategy to reuse *and* rewrite is exactly what we have pursued for Fedora 4. I'll delve into more detail in future posts, but here are some highlights:

  1. 1 week to implement the minimum feature set to support running [Hydra](http://hydra.fcrepo.org/) and [Islandora](http://islandora.fcrepo.org/) on top of Fedora 4.
  2. Built-in support for [clustering](https://docs.jboss.org/author/display/MODE/Clustering)
  3. Build-in support for [transactions](https://docs.jboss.org/author/display/MODE/Using+JTA+Transactions)
  4. Fedora 3's [128,359](http://sonar.fcrepo.org/dashboard/index/463) vs. Fedora 4's [5307](http://sonar.fcrepo.org/dashboard/index/1) lines of code
  
Coming up next: a look at some of the "lean" principles we've adopted to avoid the "build it and they will come" syndrome and how to participate and contribute to Fedora Futures.