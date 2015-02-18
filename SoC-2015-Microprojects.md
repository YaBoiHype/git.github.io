---
layout: default
title: SoC 2015 Applicant Microprojects
---

## Introduction

It is strongly recommended that students who want to apply to the Git
project for the Summer of Code 2015 submit a small code-related patch
to the Git project as part of their application.  Think of these
microprojects as the "Hello, world" of getting involved with the Git
project; the coding aspect of the change can be almost trivial, but to
make the change the student has to become familiar with many of the
practical aspects of working on the Git project.

*NOTE: Students who plan to work on libgit2, which is also under the
Git umbrella in the Google Summer of Code, should refer to [the
libgit2 list of
projects](https://github.com/libgit2/libgit2/blob/development/PROJECTS.md)
rather than the list below.*

Consider [a sample email
thread](http://thread.gmane.org/gmane.comp.version-control.git/239068),
which shows how a developer proposed a change and a patch to implement
it.  The problem being solved, the design of the proposed solution,
and the implementation of that design were all reviewed and discussed,
and after several iterations an improved version of the patch was
accepted into our codebase.  As a GSoC student, you will be playing
the role of the developer and engaging in a similar discussion.  Get
familar with the flow, need for clarity on both sides (i.e. you need
to clearly defend your design, and need to ask clarifications when
questions/suggestions you are offered are not clear enough), the pace
at which the discussion takes place, and the general tone of the
discussion, to learn what is expected of you.

To complete a microproject, you will have to go through approximately
the following steps:

* Download the source code: clone the repository using the [Git via
  Git](http://git-scm.com/downloads) instructions and read the
  `README` file.

* Build the source code: this is described in the file `INSTALL`.

* Glance over our coding guidelines in the file
  `Documentation/CodingGuidelines`.  We take things like proper code
  formatting very seriously.

* Read about the process for submitting patches to Git: this is
  described in `Documentation/SubmittingPatches`.

* **Make the actual change.** (Funny, this is the only part they teach
  you about in college.)

* Run the test suite and make sure it passes 100%: this is described
  in the file `t/README`.  (If you have added new functionality, you
  should also add new tests, but most microprojects will not add new
  functionality.)

* Commit your change.  Surprise: we use Git for that, so you will need
  to gain at least
  [a basic familiarity](http://git-scm.com/documentation) with using
  Git.  Make sure to write a good commit message that explains the
  reason for the change and any ramifications.  Remember to make sure
  that you agree with our "Developer's Certificate of Origin" (whose
  text is contained in `Documentation/SubmittingPatches`), and to
  signify your agreement by adding a `Signed-off-by` line.

* Submit your change to the Git mailing list.  For this step you
  probably want to use the commands `git format-patch` and `git
  send-email`.  Make sure that your email is formatted correctly: send
  a test version of the email to yourself and see if you can apply it
  to your repository using `git am`.

* Expect feedback, criticism, suggestions, etc. from the mailing list.

  *Respond to it!* and follow up with improved versions of your
  change.  Even for a trivial patch you shouldn't be surprised if it
  takes two or more iterations before your patch is accepted.  *This
  is the best part of participating in the Git community; it is your
  chance to get personalized instruction from very experienced peers!*

The coding part of the microproject should be very small (say, 10-30
minutes).  We don't require that your patch be accepted into master by
the time of your formal application; we mostly want to see that you
have a basic level of competence and especially the ability to
interact with the other Git developers.

When you submit your patch, please mention that you plan to apply for
the GSoC.  This will ensure that we take special care not to overlook
your application among the large pile of others.

Students: Please attempt only **ONE** microproject.  We want quality,
not quantity!  (Also, it takes work to collect the ideas, and it would
be nice to have enough microprojects for everybody.)  If you've
already done a microproject and are itching to do more, then get
involved in other ways, like finding and fixing other problems in the
code, or improving the documentation or code comments, or helping to
review other people's patches on the mailing list, or answering
questions on the mailing list or in IRC, or writing new tests, etc.,
etc.  In short, start doing things that other Git developers do!

## Ideas for microprojects

The following are just ideas.  Any small code-related change would be
suitable.  Just remember to keep the change small!  It is much better
for you to finish a small but complete change than to try something
too ambitious and not get it done.

**TODO** add entries