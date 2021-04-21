# Research into using Discourse as a replacement for the mailman mailing lists

## Background and Goal

LLVM is using a [Mailman server](https://lists.llvm.org/mailman/listinfo)
to host the [mailing lists](https://llvm.org/docs/GettingInvolved.html#mailing-lists). These are one of the main forms of communication
within the community. The Mailman server was causing
[some issues](https://lists.llvm.org/pipermail/llvm-dev/2021-March/149027.html)
recently and also creates significant administration and maintenance overhead.

A while ago there was an
[announcement](https://lists.llvm.org/pipermail/llvm-dev/2019-November/136880.html)
of an [LLVM Discord server](https://llvm.discourse.group/). So goal of this
document is to figure out if/how we could migrate from mailman to Discourse.

## Discord terms

Discord's structure is similar to a set of mailing lists, however different
terms are used there. To help with the transition, here's a translation table
for the terms:

| Mailing list | Discourse |
|--------------|-----------|
| Mailing list | category, consists of topics |
| thread | topic, consists of posts |
| email | post |

## Mailing list archive

To fully shut down mailman, we need to have a solution to also migrate our
mailing list archive. Discourse seems to support [importing mailing
lists](https://meta.discourse.org/t/importing-mailing-lists-mbox-listserv-google-groups-emails/79773).

**TODO:** Can we somehow maintain/redirect the existing mailman links so they keep working?

## Interacting with Discourse via Emails

Some folks want to interact with Discourse purely via their email program. Here
are the typical use cases:

* [Subscribing to a category or topic](https://discourse.mozilla.org/t/how-do-i-subscribe-to-categories-and-topics/16024)
* **TODO:** Replying to a topic is
  [supported](https://meta.discourse.org/t/set-up-reply-via-email-support/14003)
  , not sure if that is set up.
* [Quoting previous topics in an reply](https://meta.discourse.org/t/single-quote-block-dropped-in-email-reply/144802)
* **TODO:** Creating new topics is
  [supported](https://meta.discourse.org/t/start-a-new-topic-via-email/62977)
  , not sure if it configured on our server.Also not sure how to find
  out the email address.

## Code reviews via email

Our [code review
policy](https://llvm.org/docs/CodeReview.html#what-tools-are-used-for-code-review)
allows for code reviews via email. However I was not able to find a single code
review on any of the ``-commits`` mailing lists between 2021-02-01 and
2021-04-21. So maybe this is not used any more?

**TODO:** Ask the community if this is still used anywhere. If not: update the
documentation accordingly.

## Handling automatic notifications

Not only humans are posting to our mailing lists, we also have some services
posting automatic messages to notify folks of some event that occurred.

* *New code reviews on Phabricator:*
  You can set up a custom [Herald](https://reviews.llvm.org/herald/) rule to get
  notified on the reviews you're interested in.

* *Changes to an ongoing code review on Phabricator:*
  This is supported by Phabricator out of the box. Open the revision you're
  interested in and click on "Subscribe" in the box on the right.

* *New commits to the GitHub repositories:*
  You can subscribe to an Atom feed for any GitHub repo. For the LLVM main branch
  point your Atom reader to
  [github.com/llvm/llvm-project/commits/main.atom](https://github.com/llvm/llvm-project/commits/main.atom)
  or
  [github.com/llvm/llvm-project/commits/release/12.x.atom](https://github.com/llvm/llvm-project/commits/release/12.x.atom)
  for the release branch. You can generate these URLs by adding the suffix
  ``.atom`` to the URL of the respective "commits" page.

* *New/modified bugs:*
  After the [migration](https://lists.llvm.org/pipermail/llvm-dev/2019-October/136162.html) from the [LLVM Bugzilla](http://bugs.llvm.org/) to GitHub issues, you can "watch" the issues
  of a repository.

* *LLVM Weekly newsletter:*
  You can subscribe to it on their [website](http://llvmweekly.org/).

* *Sending a post from a script:*
  In case you want to [create a new
  post/topic](https://docs.discourse.org/#tag/Posts/paths/~1posts.json/post)
  automatically from a script or tool, you can use the
  [Discourse API](https://docs.discourse.org/).

**TODO:** Do we need some machine-to-machine notifications? Is any service
parsing the emails on the mailing list?

**TODO:** Figure out who is posting to the [www-scripts mailing list](https://lists.llvm.org/cgi-bin/mailman/listinfo/www-scripts) and who uses
that information.

## other use cases

**TODO:** Figure out if there are additional use cases, not covered above.
