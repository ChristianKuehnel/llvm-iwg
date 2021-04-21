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
allows for code reviews via email.

**TODO:** notifications about new code reviews on Phabricator: create Herald
rule

**TODO:** notifications about new comments/changes to existing code reviews on
Phabricator: should work out of the box

**TODO:** Link to documentation on how to do a code review via email, if that exists.

**TODO:** Understand if people still use this feature. If so: add links to some examples.

**TODO:** Figure out if we can find a specific solution for this with
Phabricator.

## Handling automatic notifications

Not only humans are posting to our mailing lists, we also have some services
posting automatic messages to notify folks of some event that occurred.

**TODO:** find out what automatic notifications end up on the mailing lists
(new commits, new code reviews, CI results, ...).

**TODO:** Figure out what options we have to do that on Discourse (or some other
notification service).
