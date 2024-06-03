---
title: Version Control Beyond Git
description: What is the next iteration of Version Control
date: 2024-05-20
tags:
  - another tag
layout: layouts/post.njk
---


Git has been around since 2005 and has become the
de-facto standard for controlling versions of source code. I recently asked
myself if Git will continue to dominate and if innovation is
possible or even necessary. I will go through some
of the major drawbacks of Git, that could motivate innovation,
list Git alternatives and conclude with my personal thoughts on
the future of version control.

--excerpt--

__Update:__
*Folks are discussing this post and version control in general over
on [HN](https://news.ycombinator.com/item?id=40424452) so head over
there if you want to read some more opinions and thoughts on this
topic.*

If you are new user to Git, it is likely that your critique will
focus on usability. Its user interface is called obnoxious and arcane
and its underlying concepts can be alien and difficult to learn.
Engineers famously say they learn just enough to use it but if
something does not go exactly according to plan, they are lost,
don't understand how to solve the problem and simply [start over](https://xkcd.com/1597/).

I think in 2024, this opinion is less wide-spread than it once was.
Git is an essential tool for version control and collaboration in
software development. There exist excellent learning resources and
Git clients that focus on user experience. Universities teach Git as
part of introductory software engineering courses.

If you are a long time and heavy Git user and especially if you are
working in large repos (many commits, many files, large files,
or a combination thereof) you might be more concerned with scalability
issues. This was for example [investigated](https://public-inbox.org/Git/CB5074CF.3AD7A%25joshua.redstone@fb.com/t/#u)
in 2012 by engineers at Facebook. Since then, a lot of improvements
and additions were made, to tackle performance problems in Git.
For example [Git-lfs](https://Git-lfs.com/) for large files,
[fsmonitor](https://Git-scm.com/docs/Git-fsmonitor--daemon) to
improve status performance and
[shallow](https://Git-scm.com/docs/Git-clone#Documentation/Git-clone.txt-code--depthcodeemltdepthgtem) or
[partial](https://Git-scm.com/docs/Git-clone#Documentation/Git-clone.txt-code--filtercodeemltfilter-specgtem)
tools like [scalar](https://Git-scm.com/docs/scalar).

<figure>
    <img src="stack-area.jpg" alt="Photograph of Stack Area - National Archives events and personnel">
    <figcaption>Photograph of Stack Area - <em>The U.S. National Archives</em></figcaption>
</figure>

But working with very large repos still can feel a bit awkward, because
Git is a distributed version control system where each repository has a
complete copy of all files and their full history.

Beyond scalability, there are a few features missing in Git, one might
come across when using it for some time or for particular projects.

- granular permissions: Git lacks built-in support for granular permissions
  on different parts of a repository
- binary diffing: Git's diff and merge capabilities are designed for text files.
  Handling and diffing binary files is not well supported.


## Alternatives

So let's look into what Git alternatives exists -- lets start with older ones:

### Open Source

[Mercurial](https://www.mercurial-scm.org/) was
started almost at the same time as Git by Olivia Mackall, a kernel
hacker at the time, motivated, just like Linus, by the BitKeeper and
SourcePuller [drama](https://en.wikipedia.org/wiki/BitKeeper#BitKeeper_and_the_Linux_Kernel)
of spring 2005. It is [famously](https://graphite.dev/blog/why-facebook-doesnt-use-Git)
used at Facebook. I've never used it and I don't think it beats Git
in terms of performance. But studying it for a bit, I discovered one
of my new favorite commands [absorb](https://www.mercurial-scm.org/doc/hg.1.html#absorb).
Also, in the context of studying Mercurial, I discovered stacked-diffs
Overall it is an enriching journey into version control workflows.

[Fossil](https://fossil-scm.org) is a bit of an odd duck in the Git-alternative
lineup. In addition to being a distributed VCS like Git, it features bug tracking,
wiki, forum, email alerts and chat. In a way it is Git + GitHub in one package.
It is built on and is used for SQLite development -- their homepage is I believe
a fossil repo. One can get free hosting on [Chisel](https://chiselapp.com/). It seems
like an interesting choice for smaller projects and self-hosting enthusiasts.

[sapling](https://Github.com/facebook/sapling), releated by Facebook is a
cross-platform, highly scalable, Git-compatible source control system. It is
inspired by Mercurial (no surprise there). It looks quite interesting,
but appears to be a work in progress. They are working on a custom server
called Mononoke, which I presume would replace GitHub once it is available.
It seems quite interesting and I will follow its progress.

[Jujutsu](https://Github.com/martinvonz/jj) or jj is a version control system
started by Martin von Zweigbergk as a hobby project in late 2019 that has now
turned into a full-time project at Google. Some of its highlights I find
intruding and that are called out in a talk ([slides](https://docs.google.com/presentation/d/1F8j9_UOOSGUN9MvHxPZX_L4bQ9NMcYOp1isn17kTC_M/view)) about jj are that the working copy is a commit that gets amended automatically. \
I find this very interesting and would love to see how this changes some pathological
workflows where `Git status` shows tons of untracked files.
Also, one can commit conflicts and delay the resolution and collaborate on
the resolution. jj is written as a Rust library.


### Commercial Alternatives

There are some closed-source commercial alternatives to Git

[Perforce Helix Core](https://help.perforce.com/helix-core/)
is a centralized version control system that is known for its performance
and handling large repositories that hold binary files well. Working
offline is possible and it supports exclusive checkouts (file locking).
Perforce works with changelists. As far as I can tell, a changelist
is a mix of pull request and Git branch with a single commit. Push
and pull commands are implemented as shelve and un-shelve operations
for backup, collaboration and local testing.

[Plastic SCM](https://docs.plasticscm.com) is similar to Perforce,
and is popular with game developers with tight integration with Unity
but can also be used stand-alone. In my investigation into how it is used,
I learned that it does not seem to be uncommon to track repositories
with 70TB size.

[Diversion](https://docs.diversion.dev/quickstart) is a relatively new
contender in the space; just like Perforce and Plastic, they advertise
asset management, so large binary files common in game development.

[AllSpice](https://learn.allspice.io/docs) is not a real Git alternative but
a very interesting addition to Git. It adds first class hardware support
on a GitHub-like platform with file diffing for electronics design files
like schematics and boards.

The world of version control for physical designs is fascinating in general.
These systems are often tied to the design tool itself (like Solidworks PDM
and Autodesk Vault).


### Smaller Open Source Projects

There are a number of smaller Git alternatives that should be mentioned.

[Breezy](https://www.breezy-vcs.org/) is a fork of the abandoned GNU Bazaar.
[Grace](https://Github.com/ScottArbeit/Grace) is an approximately 2 year old
system in development, implemented in F#.
[Pijul](https://pijul.org/) is developed in Rust, some of its interesting
features are commutative changes, explicitly modeling merge conflicts and
line-order preserving changes. Pijul has a GitHub like collaboration platform
called [Nest](https://nest.pijul.com/).
[Darcs](https://darcs.net/) is similar, written in Haskell, focusses on
simplicity and allows collaboration on their [Hub](https://hub.darcs.net/).


## Future

If I could choose the features for my ideal source control system, it would
support very large repositories including large binary files, fine grained
access control, and would allow me to work offline. It would support different
workflows like pull requests and changelists.

Also, it would be to combine a source control system like Git and a build
system like Bazel.

Imagine typing

```
gtb build <my_branch> //my/target
# or
gtb test <sha> //my/test
```

and the system only has to download the necessary files to build or test
that specific version or spin up remote executor to run the requested task.

In a way today, tools like Git and Bazel overlap already. For incremental
builds, Bazel identifies which files have changed using content hashing
and metadata, just like a version control system, and then determines
which actions need to be re-run based on the updated dependency graph.

Furthermore, users could see how their changes affect the build dependency
graph, because the system tracks both file versions and the graph that
describes how build artifacts are derived from sources. This could be
valuable information for larger monorepos where code can cross domains
from backend to frontend, and from firmware and mobile. One could ask
what commits have contributed to changes in an application. This can
be helpful in industries where traceability is important and where risk
assessments have to be conducted for every release.

Switching gears back to engineering, action caches that speed up CI
and save on compute when building and testing changes are gaining
some traction. I think it would be pretty cool if we could retrieve
artifacts from the cache just like we browse source code with Git.

Here is why: there are a number of use cases, where controlling
sources is not enough: build outputs are important artifacts that need
to be version controlled as well. Instead of a adding a separate system
(these systems are sometimes called Product Lifecycle Management or
PLM systems) I would love a tool that tracks both build inputs and
outputs as first class concepts. And since the tool can differentiate
between inputs and outputs, it can track sources and derived files
separately or present them separately to users. No more littering the
source directories with application builds, compiled
libraries, firmware bundles, gerber files, G-code files, technical
drawings, etc. next to source file. No more kludgy integrations with
S3 or other artifact stores. If we could find a way to separate
jupyter notebook output from the notebook itself, we could commit
the notebook runs/output for tracing and sharing as well.

It would be interesting to see if something like this would be useful
in machine learning workflows. I can imagine that reproducibility of
experiments is an something people think about, maybe each experiment
could be associated with a version which would hold every input and
every output of the experiment without quite naturally.

I'm not sure how the UX would work for something like this, but if done
correctly, I think it could be quite useful.
