---
title: Version Control Beyond Git
description: What is the next iteration of Version Control
date: 2024-05-20
tags:
  - another tag
layout: layouts/post.njk
---


Git has been around since 2005 and has become the
de-facto standard for controlling versions of source code. I am
wondering if Git will continue to dominate or if innovation is
possible or even necessary in this domain. I will go through some
of the major drawbacks of git, that could motivate innovation,
list Git alternatives and conclude with my personal thoughts on
the future of version control.

--excerpt--

If you are new user to git, it is likely that your critique will
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
issues. This was for example [investigated](https://public-inbox.org/git/CB5074CF.3AD7A%25joshua.redstone@fb.com/t/#u)
in 2012 by engineers at Facebook. Since then, a lot of improvements
and additions were made, to tackle performance problems in git.
For example [git-lfs](https://git-lfs.com/) for large files,
[fsmonitor](https://git-scm.com/docs/git-fsmonitor--daemon) to
improve status performance and
[shallow](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt-code--depthcodeemltdepthgtem) or
[partial](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt-code--filtercodeemltfilter-specgtem)
tools like [scalar](https://git-scm.com/docs/scalar).

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
of spring 2005. It is [famously](https://graphite.dev/blog/why-facebook-doesnt-use-git)
used at Facebook. I've never used it and I don't think it beats Git
in terms of performance. But studying it for a bit, I discovered one
of my new favorite commands [absorb](https://www.mercurial-scm.org/doc/hg.1.html#absorb).
Also, in the context of studying Mercurial, I discovered stacked-diffs
Overall it is an enriching journey into version control workflows.

[Fossil](https://fossil-scm.org) is a bit of an odd duck in the git-alternative
lineup. In addition to being a distributed VCS like git, it features bug tracking,
wiki, forum, email alerts and chat. In a way it is Git + GitHub in one package.
It is built on and is used for SQLite development -- their homepage is I believe
a fossil repo. One can get free hosting on [Chisel](https://chiselapp.com/). It seems
like an interesting choice for smaller projects and self-hosting enthusiasts.

[sapling](https://github.com/facebook/sapling), releated by Facebook is a
cross-platform, highly scalable, Git-compatible source control system. It is
inspired by Mercurial (no surprise there). It looks quite interesting,
but appears to be a work in progress. They are working on a custom server
called Mononoke, which I presume would replace GitHub once it is available.
It seems quite interesting and I will follow its progress.

[Jujutsu](https://github.com/martinvonz/jj) or jj is a version control system
started by Martin von Zweigbergk as a hobby project in late 2019 that has now
turned into a full-time project at Google. Some of its highlights I find
intruding and that are called out in a talk ([slides](https://docs.google.com/presentation/d/1F8j9_UOOSGUN9MvHxPZX_L4bQ9NMcYOp1isn17kTC_M/view)) about jj are that the working copy is a commit that gets amended automatically. \
I find this very interesting and would love to see how this changes some pathological
workflows where `git status` shows tons of untracked files.
Also, one can commit conflicts and delay the resolution and collaborate on
the resolution. jj is written as a Rust library.


### Commercial Alternatives

There are some closed-source commercial alternatives to Git

[Perforce Helix Core](https://help.perforce.com/helix-core/)
is a centralized version control system that is known for its performance
and handling large repositories that hold binary files well. Working
offline is possible and it supports exclusive checkouts (file locking).
Perforce works with changelists. As far as I can tell, a changelist
is a mix of pull request and git branch with a single commit. Push
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

There are a number of smaller git alternatives that should be mentioned.

[Breezy](https://www.breezy-vcs.org/) is a fork of the abandoned GNU Bazaar.
[Grace](https://github.com/ScottArbeit/Grace) is an approximately 2 year old
system in development, implemented in F#.
[Pijul](https://pijul.org/) is developed in Rust, some of its interesting
features are commutative changes, explicitly modeling merge conflicts and
line-order preserving changes. Pijul has a GitHub like collaboration platform
called [Nest](https://nest.pijul.com/).
[Darcs](https://darcs.net/) is similar, written in Haskell, focusses on
simplicity and allows collaboration on their [Hub](https://hub.darcs.net/).


## Future

merge something like bazel and version control
imagine you want to `bazel build <sha> //...` and your version control
system only has to check out the necessary files to build that

imagine storing build outputs in your version control system, a file-addressable build cache of sorts
this is important because for some workflows build outputs are important to store in a secure way

(actually most flows since we always want to know what we are building)


Improved Collaboration and Code Reviews:

Developers can see how their changes affect the dependency graph,
providing better context for code reviews and collaborative development.




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


[2005](https://github.com/git/git/tree/e83c5163316f89bfbde7d9ab23ca2e25604af290)


Images
- https://commons.wikimedia.org/wiki/File:Storage,_file_cabinet,_military_Fortepan_72505.jpg
- https://picryl.com/media/photograph-of-the-acetate-motion-picture-film-storage-vault-at-the-national-6cd5ce
- https://picryl.com/media/photograph-of-flat-file-storage-stacks-628b41
- https://picryl.com/media/photograph-of-stack-area-ed85de
- https://graphite.dev/blog/why-facebook-doesnt-use-git

