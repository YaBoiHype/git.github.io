---
title: Git Rev News Edition 13 (March 16th, 2016)
layout: default
date: 2016-03-16 21:06:51 +0100
author: chriscool
categories: [news]
navbar: false
---

## Git Rev News: Edition 13 (March 16th, 2016)

Welcome to the 13th edition of [Git Rev News](http://git.github.io/rev_news/rev_news.html),
a digest of all things Git. For our goals, the archives, the way we work, and how to contribute or to
subscribe, see [the Git Rev News page](http://git.github.io/rev_news/rev_news.html) on [git.github.io](http://git.github.io).

This edition covers what happened during the month of February 2016.

## Discussions

### General

* [RFC: Resumable clone based on hybrid "smart" and "dumb" HTTP](http://thread.gmane.org/gmane.comp.version-control.git/285921/)

It is a well known problem that `git clone` is not resumable. If the
connection comes down during a clone, the clone has to be restarted
from scratch.

A work around that is often suggested is to use `git bundle` first, then
to rsync that bundle and eventually to clone using the rsync'ed bundle. Some
tools [like gitolite](http://thread.gmane.org/gmane.comp.version-control.git/235040/) have even been making it easier to support that.

There was also at one point in 2011
[a patch series](http://thread.gmane.org/gmane.comp.version-control.git/185196/)
to improve the support of this kind of clone workflow internally.

And for some time this was thought of as just a small manpower problem. A
few month of dedicated work by anyone could probably fix that. It was
even proposed as a Google Summer of Code (GSoC) project.

Over time though Git developers realized that it was not so easy
because some very careful design was needed, and it was removed from
the list of possible GSoC projects.

So it was very exciting to see a number of new proposals pop up on the
list during the last few months.

It started on February 5 with
[a "Resumable clone revisited, proof of concept" patch series by Duy Nguyen](http://thread.gmane.org/gmane.comp.version-control.git/285555/)
where he wrote:

> I was reminded by LWN about this. Annoyed in fact because it's
> called a bug while it looks more like an elephant.

and pointed to [a LWN.net article](https://lwn.net/Articles/674392/)
that reports about
[Sarah Sharp speaking at the SCALE 14x conference](https://www.socallinuxexpo.org/scale/14x/presentations/improving-diversity-maslows-hierarchy-needs):
"she noted that Git still does not support interrupting and resuming
download operations, which is an important bug to fix."

Then on February 10 Shawn Pearce sent
[an 'RFC: Resumable clone based on hybrid "smart" and "dumb" HTTP' proposal](http://thread.gmane.org/gmane.comp.version-control.git/285921/)
that he had discussed internally with other people at Google where he works.

This was followed on March 2 by
[an email called "Resumable git clone?"](http://thread.gmane.org/gmane.comp.version-control.git/288088/)
from Josh Triplett, a well known Linux Kernel developer, who asked:

> In a discussion elsewhere, Al Viro suggested taking the partial pack
> received so far, repairing any truncation, indexing the objects it
> contains, and then re-running clone and not having to fetch those
> objects.  This may also require extending receive-pack's protocol for
> determining objects the recipient already has, as the partial pack may
> not have a consistent set of reachable objects.
>
> Before starting down the path of developing patches for this, does the
> approach seem potentially reasonable?

Josh talks about Al Viro who is another well known Linux Kernel
developer, and it's interesting to see Linux Kernel developers
interested again in taking part in Git development. It reminds some
old timers about the "good old time".

All these proposals have been discussed by many important Git
developers and reviewers like Stefan Beller, Junio Hamano, Johannes
Schindelin, Jonathan Nieder, Eric Sunshine, Jeff King, Elia Pinto.

About Shawn's proposal there was also an interesting potential
security issue called out by Blake Burkhart. And other people like
Bhavik Bavishi and Konstantin Ryabitsev also took part of the
discussion following Josh's email.

From the last discussions about Josh's email, it appeared that Git
developers favored Shawn's proposal over others, and maybe that
Shawn's proposal was going to be worked on soon.

Then on March 5 Kevin Wern sent
[an email called "Resumable clone"](http://thread.gmane.org/gmane.comp.version-control.git/288306/),
where he said he began looking at relevant code to start working on it, and he asked:

> Is someone working on this currently?  Are there any things I should
> know moving forward?  Is there a certain way I should break
> down/organize the feature when writing patches?

Duy answered that "Resumable clone is happening." And pointed to
[some preparation work](http://thread.gmane.org/gmane.comp.version-control.git/288205/focus=288222)
by Junio Hamano [going on](http://thread.gmane.org/gmane.comp.version-control.git/288080/focus=288150).
Junio by the way answered with
[a very long email](http://thread.gmane.org/gmane.comp.version-control.git/288306/focus=288317)
that contains "a rough and still slushy outline" of what remains to be
done. This was then discussed and explained further.

It is not clear if Shawn's proposal and Josh's email were inspired by
Sarah Sharp's remark, and LWN.net's report about it, but anyway it
looks like hopefully this old and annoying problem is going to be
fixed not too far away into the future.

### Reviews

* [config: add '--show-origin' option to print the origin of a config value](http://thread.gmane.org/gmane.comp.version-control.git/286198/)

For some time Lars Schneider has been sending versions of a short
patch series to make it possible to see where a config option comes
from:

- [v1](http://thread.gmane.org/gmane.comp.version-control.git/285553/)
- [v2](http://thread.gmane.org/gmane.comp.version-control.git/285894/)
- [v3](http://thread.gmane.org/gmane.comp.version-control.git/286110/)
- [v4](http://thread.gmane.org/gmane.comp.version-control.git/286197/)
- [v5](http://thread.gmane.org/gmane.comp.version-control.git/286485/)
- [v6](http://thread.gmane.org/gmane.comp.version-control.git/286672/)

Version 1 was itself based on a previous patch by Jeff King.

The new feature could be useful because config options can be set in
many ways. The usual way is to use one of the config files:

- the ".git/config" file at the root of the working directory,
- the user's "~/.gitconfig" file,
- the user's "~/.config/git/config" file, or
- a system wide "/etc/gitconfig" file.

But the exact paths of the above files depend on how git was compiled
and on the values of at least the GIT_CONFIG, GIT_CONFIG_NOSYSTEM,
GIT_DIR and XDG_CONFIG_HOME environment variables.

Also config values can also be set on the command line using the 'git
-c' option, like `git -c <key>=<value> config ...`. Or they can be
read from another file using the 'config -f' option, like `git config
-f <file> ...`, or from the standard input. They can also be included
from another file by using a "include.path = <path>" directive in a
config file.

As can be seen in the above threads, even if it seems simple, there
are a lot of details to get right. These details include the name of
the option. This was discussed by Sebastian Schuberth, Jeff King,
Ramsay Jones, Mike Rappazzo and Junio Hamano. So the name was changed
to '--show-origin'.

Other details that were discussed are about the format of the output
in special cases, like when the 'git -c' option is used, or when the
'config -f' option is used, or when config is read from the standard
input.

The `git config` option also has a lot of different modes depending on
which options it is passed, so it was discussed with which other
options it is ok to pass '--show-origin'.

Many details in the code and tests where also discussed by Eric
Sunshine, Johannes Schindelin, Johannes Sixt, Jeff, Ramsay and Junio.

One nice outcome of this patch series though is that error messages
when there are problems in the config can now tell more precisely
where the problems come from.

* [add DEVELOPER makefile knob to check for acknowledged warnings](http://thread.gmane.org/gmane.comp.version-control.git/287345)

Jeff King noted in a [review](http://thread.gmane.org/gmane.comp.version-control.git/285553/focus=285568) 
that *git style doesn't allow declaration-after-statement*. Thereupon
Lars Schneider posted a [patch](http://thread.gmane.org/gmane.comp.version-control.git/285752) 
to check for this warning in the TravisCI build. In the review of this 
patch Jeff [suggested](http://thread.gmane.org/gmane.comp.version-control.git/285752/focus=285761)
to codify the knowledge about the warnings into an optional Makefile knob 
called "DEVELOPER". Lars combined the warnings that [Junio and Jeff](http://thread.gmane.org/gmane.comp.version-control.git/285752/focus=285761) care about and posted a revised version of the [patch](http://thread.gmane.org/gmane.comp.version-control.git/287345).

Git developers with a reasonably modern compiler can now compile Git with 
`DEVELOPER=1 make` or set the flag once for all make executions with 
`echo DEVELOPER=1 >>config.mak` to ensure their patches are clear of all 
compiler warnings that the Git project cares about.

<!---
### Support
-->

## Releases

The last month we saw some maintenance releases of Git, along with some RCs of the upcoming 2.8:

* [Git v2.7.3](https://lkml.org/lkml/2016/3/10/612)
* [Git for Windows 2.7.2](https://github.com/git-for-windows/git/releases/tag/v2.7.2.windows.1)
* [Git v2.8.0-rc2](http://article.gmane.org/gmane.linux.kernel/2174436) 

And then there was a significant, albeit humbly versioned new libgit2, which dominoed through its wrapper projects:

* [Release libgit2 v0.24.0 · libgit2/libgit2](https://github.com/libgit2/libgit2/releases/tag/v0.24.0)
* [Release Rugged v0.24.0 · libgit2/rugged](https://github.com/libgit2/rugged/releases/tag/v0.24.0)
* [Release v0.24.0 · libgit2/pygit2](https://github.com/libgit2/pygit2/releases/tag/v0.24.0)
* [Release 0.11.0: Merge pull request #559 from libgit2/piet/write-merge-conflicted-files · libgit2/objective-git](https://github.com/libgit2/objective-git/releases/tag/0.11.0)
* [Release LibGit2Sharp v0.22 · libgit2/libgit2sharp](https://github.com/libgit2/libgit2sharp/releases/tag/v0.22)
* [Release v0.11.9 · nodegit/nodegit](https://github.com/nodegit/nodegit/releases/tag/v0.11.9)

Other releases:

* [GitLab major release 8.5](https://about.gitlab.com/2016/02/22/gitlab-8-5-released/), with patches up to [8.5.5](https://about.gitlab.com/2016/03/10/gitlab-8-dot-5-dot-5-released/)
* [Magit 2.5](http://emacsair.me/2016/02/10/magit-2.5)

## Other News

__Various__

* [My git aliases](http://kamalmarhubi.com/blog/2016/02/29/my-git-aliases/), by Kamal Marhubi
* [Here's a band you can book with a pull request](https://github.com/rawfunkmaharishi/rawfunkmaharishi.github.io/blob/master/gigs/_posts/HOW_TO_BOOK_THE_BAND.md)
* [Git Cheatsheet](http://satanas.io/cheatsheets/git/), by Wil Alvarez
* [GitHub responds to the Open Source Maintainers "dear-github" letter](https://github.com/dear-github/dear-github/pull/115)
* [A presentation showing off Git as a roleplay: Git Dance](https://www.youtube.com/watch?v=cyMl6PNm5ts)
* [And another Git Dance](https://www.youtube.com/watch?v=8hgNe8Q6kZE)


__Light reading__

* [Don't "Push" Your Pull Requests](https://www.igvita.com/2011/12/19/dont-push-your-pull-requests/), by Ilya Grigorik
* [Git rebase and the golden rule explained](https://medium.freecodecamp.com/git-rebase-and-the-golden-rule-explained-70715eccc372#.93ok9bhsw), by Pierre de Wulf
* [GitHub Workflow (used by Frameworks team at BBC News)](http://www.integralist.co.uk/posts/github-workflow.html)
* [Write, Review, Merge, Publish: Phabricator Review Workflow](https://secure.phabricator.com/phame/post/view/766/write_review_merge_publish_phabricator_review_workflow/)
* [A succesful Git branching model considered harmful](https://barro.github.io/2016/02/a-succesful-git-branching-model-considered-harmful/), by Jussi Judin
* [Versioning a Microservice System with git](https://opencredo.com/versioning-a-microservice-system-with-git/), by David Borsos
* [Understanding git for real by exploring the .git directory](https://medium.freecodecamp.com/understanding-git-for-real-by-exploring-the-git-directory-1e079c15b807#.r1v5tvtqn), also by Pierre de Wulf
* [A Grip On Git: A Simple, Visual Git Tutorial](https://agripongit.vincenttunru.com/), by Vincent Tunru

__Git tools and sites__

* [InfoQ: Google Kick-Starts Git Ketch: A Fault-Tolerant Git Management System](http://www.infoq.com/news/2016/02/google-kick-starts-git-ketch)
* [diff-so-fancy: Good-lookin' diffs with diff-highlight and more](https://github.com/so-fancy/diff-so-fancy)
* [git-blameall](http://1dan.org/git-blameall/)
* [Go-git – low-level and extensible Git client library in Go](https://github.com/src-d/go-git)
* [New version of Atlassian SourceTree is out](http://blog.sourcetreeapp.com/2016/02/22/sourcetree-update-atlassian-account-git-lfs-support-ui-refresh-and-more/)

## Credits

This edition of Git Rev News was curated by Christian Couder &lt;<christian.couder@gmail.com>&gt;,
Thomas Ferris Nicolaisen &lt;<tfnico@gmail.com>&gt; and Nicola Paolucci &lt;<npaolucci@atlassian.com>&gt;,
with help from Josh Triplett.