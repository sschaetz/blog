---
title: Exploring Git Alternatives
description: What is the next iteration of Version Control
date: 2024-05-20
tags:
  - another tag
layout: layouts/post.njk
---


Git has been around since [2005](https://github.com/git/git/tree/e83c5163316f89bfbde7d9ab23ca2e25604af290) and has become the
de-facto standard for controlling versions of source code. I am
wondering if git will continue to dominate or if innovation is
possible or even necessary in this domain. I will go through some
of the major drawbacks of git, that could motivate innovation,
list git alternatives and conclude with my personal thoughts on
the future of version control.

--excerpt--

The most commonly cited limitation of git is related to scalability.
Git is not designed to handle large files well, and using it with large repos
can run into limitations as documented on the [git mailing list](https://public-inbox.org/git/CB5074CF.3AD7A%25joshua.redstone@fb.com/t/#u)
in 2012. The most common response then was to suggest to shard
the repository. Since then, a lot of improvements and additions were made,
for example [git-lfs](https://git-lfs.com/) for large files,
[fsmonitor](https://git-scm.com/docs/git-fsmonitor--daemon) to
improve status performance and
[shallow](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt-code--depthcodeemltdepthgtem) or
[partial](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt-code--filtercodeemltfilter-specgtem)
clones among other things.

<figure>
    <img src="stack-area.jpg" alt="Photograph of Stack Area - National Archives events and personnel">
    <figcaption>Photograph of Stack Area - National Archives events and personnel - <em>The U.S. National Archives</em></figcaption>
</figure>

But that is not the most common critique of git: its user interface
is often called clunky, alien, obnoxious, and difficult to learn.
Engineers famously say they learn just enough to use it but if
something does not go exactly according to plan, they are lost,
don't understand how to solve the problem and simply [start over](https://xkcd.com/1597/).
I personally don't consider this a valid critique; git is a tool
that can be learned; there are a few concepts to understand. There
is excellent documentation and there are many excellent git
clients out there that simplify using git.

One of the issues with git I myself have been wrangling is the question
of access control. I would love the ability to do partial checkouts
and do fine-grained access control, limiting access to privileged
parts of the source code. I have looked at work-around for that in
a [previous](https://soundbarrier.io/posts/monorepo/) blog post.


## Alternatives

So let's dive in and look at what alternatives exists -- lets start
with older ones:

[Mercurial](https://www.mercurial-scm.org/) was
started almost at the same time as git by Olivia Mackall, a kernel
hacker at the time, motivated, just like Linus, by the BitKeeper and
SourcePuller [drama](https://en.wikipedia.org/wiki/BitKeeper#BitKeeper_and_the_Linux_Kernel)
of spring 2005. I've never used it but reading about it's differences
to git, it seems to be understood that git does branching better,
and that Mercurial suggests to users to not re-write history and
edit commits. I find that a bit surprising, since one of the coolest
version control commands I discovered in recent years is [absorb](https://www.mercurial-scm.org/doc/hg.1.html#absorb)
which I first discovered in Mercurial.


[Fossil](https://fossil-scm.org) is a bit of an odd duck in the git-alternative
lineup. In addition to being a distributed VCS like git, it features bug tracking,
wiki, forum, email alerts and chat. In a way it is git + GitHub in one package.
It is built on and is used for SQLite development -- their homepage is I believe
a fossil repo. One can get free hosting on [Chisel](https://chiselapp.com/). It seems
like an interesting choice for smaller projects and self-hosting purists.


And what about big tech companies? Are they working on
git alternatives?

Facebook has released [sapling](https://github.com/facebook/sapling), a
cross-platform, highly scalable, Git-compatible source control system. It is
inspired by Mercurial, the VCS used by Facebook internally. It looks quite
interesting, and they are working on a custom server called Mononoke, which
I presume would replace GitHub once it is available. It seems quite
interesting and I will follow its progress.

[Jujutsu](https://github.com/martinvonz/jj) or jj is a version control system
started by Martin von Zweigbergk as a hobby project in late 2019 that has now
turned into a full-time project at Google. Some of its highlights I find
intruding and that are called out in a talk ([slides](https://docs.google.com/presentation/d/1F8j9_UOOSGUN9MvHxPZX_L4bQ9NMcYOp1isn17kTC_M/view),
[video](https://www.youtube.com/watch?v=bx_LGilOuE4)) about jj are that the
working copy is a commit that gets amended by every command. I find this
very interesting and would love to see how this changes some pathological
existing workflows where `git status` shows tons of untracked files.
Also, one can commit conflicts and delay the resolution and collaborate on
the resolution. jj is written as a Rust library.


Smaller projects
- https://www.breezy-vcs.org/
- https://github.com/ScottArbeit/Grace
-


Commercial endeavors

- perforce
- https://allspice.io
- diversion
-




Git drawbacks

- scalability: large files are not handled well, very large monorepos
  - answer: git-lfs
    but does that work for everybody?
    partial checkouts
    shallow clones

- user interface: clunky, foreign concepts, difficult to learn, statefulness
  - answer: there are many good git clients with better UX
    but can you use them smoothly without learning and
    understanding some of the core concepts?

- access control: in a mono-repo giving users access to only parts of the repo is tricky
  (have talked about this in [previous blog post]())



Talks:
- Grace talk https://www.youtube.com/watch?v=lW0gxMbyLEM
  bad-mouths git
- what comes after git https://www.youtube.com/watch?v=M4KktA_jbOE

- alternative systems
  - pijul nest.pijul.com, path oriented design https://www.youtube.com/watch?v=7MpdZkGj5AI
  - diversion
  - grace
  - https://www.breezy-vcs.org/
  - https://allspice.io
    lay out the difference between source control and version control
    > In a source control repo, you only track changes of original source files. If part of the toolchain generates files like object code, linked libraries, or executables, the generated files are not tracked.
    vs
    > In a version control repo, you track additional output files generated by the toolchain, at every commit. You don’t have to track all of the files at every commit, only the files necessary for your process.
    this makes sense because:
    - documentation: schematic pdfs are useful to look at for bring-up and writing software/drivers
    - it is clear that the generated files, no matter in hardware or software need to be tracked somewhere. Since the source control system is already there, why not use it?



Alternative systems to store versions:
- github releases
- S3 (buckets can have versioning)
- separate Git repo (link in via LFS, build systsem or some other means)




Articles:

- Is Git irreplaceable? https://fossil-scm.org/forum/forumpost/b251b6e48e
  and relevant HN discussion https://news.ycombinator.com/item?id=21975540

- https://www.joelonsoftware.com/2010/03/17/distributed-version-control-is-here-to-stay-baby/
  he initially thought that it is too complicated, stating in a podcast
  > “To me, the fact that they make branching and merging easier just means that your coworkers are more likely to branch and merge, and you’re more likely to be confused.”

  Joel corrects himself and says

  > "When you manage changes instead of managing versions, merging works better, and therefore, you can branch any time your organizational goals require it, because merging back will be a piece of cake."

  This is a bit confusing. I think what is pointed out here is that in systems like SVN, branches are copies of directories at a certain point in time. And when you try to merge these, a lot of information is missing,
  specifically what files were modified together (in a given commit) and how they evolved together. This allows git to seamlessly merge two branches that have changes to different parts of the same file.
  It understands that these changes do not interfere with one another.


- https://lkml.org/lkml/2005/4/6/121
  this is where Linus says goodbye to BitKeeper



# History of VCS

https://blog.plasticscm.com/2010/11/version-control-timeline.html
 - https://web.archive.org/web/20200817171853/https://www.plasticscm.com/documents/version-control-history/version-control-history-poster-A3.pdf
 - https://web.archive.org/web/20200817171541/https://www.plasticscm.com/version-control-history
 - author Pablos Santos Luaces wrote a book: https://docs.plasticscm.com/book/#_branch_naming_convention

consider creating a timeline page like https://en.wikipedia.org/wiki/Template:Timeline_of_iPhone_models
pull from https://en.wikipedia.org/wiki/Comparison_of_version-control_software#History_and_adoption




Images
- https://commons.wikimedia.org/wiki/File:Storage,_file_cabinet,_military_Fortepan_72505.jpg
- https://picryl.com/media/photograph-of-the-acetate-motion-picture-film-storage-vault-at-the-national-6cd5ce
- https://picryl.com/media/photograph-of-flat-file-storage-stacks-628b41
- https://picryl.com/media/photograph-of-stack-area-ed85de
- https://graphite.dev/blog/why-facebook-doesnt-use-git
