# Research into using Discourse as a replacement for the mailman mailing lists

## Background and Goal

LLVM is using a [Mailman server](https://lists.llvm.org/mailman/listinfo)
to host the [mailing
lists](https://llvm.org/docs/GettingInvolved.html#mailing-lists). These are one
of the main forms of communication within the community. The Mailman server was
causing
[some issues](https://lists.llvm.org/pipermail/llvm-dev/2021-March/149027.html)
recently and also creates significant administration and maintenance overhead.

A while ago there was an
[announcement](https://lists.llvm.org/pipermail/llvm-dev/2019-November/136880.html)
of an [LLVM Discourse server](https://llvm.discourse.group/). So goal of this
document is to figure out if/how we could migrate from mailman to Discourse.
While there are other alternatives, they are not considered in THIS document.

## Discourse terms

Discourse's structure is similar to a set of mailing lists, however different
terms are used there. To help with the transition, here's a translation table
for the terms:

| Mailing list | Discourse |
|--------------|-----------|
| Mailing list, consists of threads | category, consists of topics |
| thread, consists of emails | topic, consists of posts |
| email | post |

## Mailing list archive

To fully shut down mailman, we need to have a solution to also migrate our
mailing list archive. Discourse seems to support [importing mailing
lists](https://meta.discourse.org/t/importing-mailing-lists-mbox-listserv-google-groups-emails/79773).

**TODO:** Can we somehow maintain/redirect the existing mailman links so they
keep working?

## Interacting with Discourse via Emails

Some folks want to interact with Discourse purely via their email program. Here
are the typical use cases:

* [Subscribing to a category or topic](https://discourse.mozilla.org/t/how-do-i-subscribe-to-categories-and-topics/16024)
* Replying to a topic works, including quoting other peoples texts
  ([tested](https://llvm.discourse.group/t/email-interaction-with-discourse/3306/4) on GMail).
* [Quoting previous topics in an reply](https://meta.discourse.org/t/single-quote-block-dropped-in-email-reply/144802)
* Creating new topics is
  [supported](https://meta.discourse.org/t/start-a-new-topic-via-email/62977)
  but not configured at the moment. We would need to set up an email address
  per category and give Discourse POP3 access to that email account. This sounds
  like a solvable issue.

## Private categories

If needed categories can have individual [security
settings](https://meta.discourse.org/t/how-to-use-category-security-settings-to-create-private-categories/87678)
to limit visibility and write permissions.

## Code reviews via email

Our [code review
policy](https://llvm.org/docs/CodeReview.html#what-tools-are-used-for-code-review)
allows for code reviews via email. It looks like we will abandon the policy and
make Phabricator the tool of choice for code reviews.

As a replacement, we can use the [email
integration](https://secure.phabricator.com/book/phabricator/article/configuring_inbound_email/)
of Phabricator, so that users can reply to code reviews from their email client.
Note: Is is a standard feature of Phabricator that is already usable today.

Proposal for a future email workflow:

* For each of the mailing lists, we create a *Project* with the same name in
  Phabricator ([example project](https://reviews.llvm.org/project/view/104/)).
  Every Phabricator user can join/leave these projects on their own.
* Everyone on these projects will receive the same email notifications from
  Phabricator as we have on the mailing lists. This is configured via *Herald*
  rules in Phabricator, as today, ([example rule](https://reviews.llvm.org/H769)).
* Users can reply to these email notifications and Phabricator will incorporate
  these responses with their email client, see
  [D101432](https://reviews.llvm.org/D101432) for some example emails. Quoting
  and markup is supported as well.
* We do **NOT** migrate the membership lists. Users need to sign up to the
  projects manually once.
* We can send an email with instructions to the mailing
  lists once everything is set up.

## Handling automatic notifications

Not only humans are posting to our mailing lists, we also have some services
posting automatic messages to notify folks of some event that occurred.

* *New code reviews on Phabricator:*
  see section of code reviews above

* *Changes to an ongoing code review on Phabricator:*
  see section of code reviews above

* *New commits to the GitHub repositories:*
  See section of code reviews above.
  As an alternative you can subscribe to an Atom feed for any GitHub repo. For
  the LLVM main branch point your Atom reader to
  [github.com/llvm/llvm-project/commits/main.atom](https://github.com/llvm/llvm-project/commits/main.atom)
  or
  [github.com/llvm/llvm-project/commits/release/12.x.atom](https://github.com/llvm/llvm-project/commits/release/12.x.atom)
  for the release branch. You can generate these URLs by adding the suffix
  ``.atom`` to the URL of the respective "commits" page.

* *New/modified bugs:*
  After the
  [migration](https://lists.llvm.org/pipermail/llvm-dev/2019-October/136162.html)
  from the [LLVM Bugzilla](http://bugs.llvm.org/) to GitHub issues, you can
  "watch" the issues of a repository.

* *Sending a post from a script:*
  In case you want to [create a new
  post/topic](https://docs.discourse.org/#tag/Posts/paths/~1posts.json/post)
  automatically from a script or tool, you can use the
  [Discourse API](https://docs.discourse.org/).

**TODO:** Do we need some machine-to-machine notifications? Is any service
parsing the emails on the mailing list?

## Reply to author only (privately)

On the mailing list you have the opportunity to reply only to the sender of the
email, not to the entire list. That is not supported when replying via email on
Discourse. However you can send someone a private message via the Web UI.

Also Discourse does not expose users' email addresses , so your private replies
have to go through their platform (unless you happen to know the email address
of the user.)

## Mapping the mailing lists

This section will propose a migration for each mailing list we have today. See
above for details on the respective use cases.

| Mailing lists | proposed migration |
|---------------|--------------------|
| all "-commits" lists | use Phabricator projects (see above) |
| all "-dev" and "-users" lists | Use Discourse category with email support |
| all "-bugs" | Use notifications on GitHub after the migration of the bug tracker |
| Bugs-admin <br/> devmtg-organizers <br/> eurollvm-organizers <br/> gsoc <br/> llvm-admin <br/> llvm-announce <br/> llvm-devmeeting <br/> llvm-foundation <br/> Release-testers <br/> Test-list <br/> Docs <br/> WiCT| Use Discourse category with email support. <br/> Permissions can be configured where needed. |
| www-scripts | **TODO** This looks like the output of a CI server. Maybe use that server's notification mechanism? |

## Archiving of code review results

Today we're using the mailing lists as archive of the code review results.
In case we shut down Phabricator (e.g. because of a GitHub Pull Request
migration), we still want to preserve the reviews. One alternative to the
mailing lists would be to create a static html version of the Phabricator
content (e.g. using a web crawler) and then archive the static html.

## Other use cases

**TODO:** Figure out if there are additional use cases, not covered above.
