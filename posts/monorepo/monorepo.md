---
title: More than the Sum of its Parts - Musing on Monorepos
description: Musing on Monorepos
date: 2024-03-03
tags:
  - another tag
layout: layouts/post.njk
---

Monorepos are great. They implicitly enforce the One Version rule, which means that for any dependency, there is only one version used across the code-base. This can mean that there is only one set of lock files for the entire repository (think single `requirements.lock`, `Cargo.lock` etc. for the repo) as well as automated integration testing of all of the latest code.

--excerpt--

<figure>
    <img src="cosmos.jpg" alt="Sum of its parts">
    <figcaption>
      <strong>Cosmos</strong> by Ilya Chashnik.
    </figcaption>
</figure>

Monorepos are great for other reasons too: 
- code can be refactored quickly and a change can be applied with one patch (vs multiple patches in a poly repo flow)
- the friction to share and re-use code is reduced (but it does not mean a monorepo gives permission to turn be turned into a tangled mess of dependencies)
- collaboration can be improved, if every team commits to the same repository, silos are less likely to form

An overwhelming majority of developers use git as their version control system according to a StackOverflow developer survey ([link](https://stackoverflow.blog/2023/01/09/beyond-git-the-other-version-control-systems-developers-use/)). But unfortunately it is not the best tool to implement a monorepo; even smaller teams might experience its limitations at some point:

Storing large files (binary blobs) is an afterthought for git and doing so can lead to performance issues; [git-lfs](https://git-lfs.com/) works well enough these days but even with it, git does not scale indefinitely.


## Silos

And what if a silo is desired or required? Here are some possible scenarios:
- only parts of the monorepo should be shared with contractors; fine-grained access control and security is not trivial to implement with git
- parts of the monorepo should be shared as an Open Source project
- a set of APIs should be shared third parties or customers.

### josh - Just One Single History

I was faced with this conundrum recently and did some research into possible solutions. This is where I discovered [josh](https://github.com/josh-project/josh) (just one single history). It is a git-aware proxy that re-writes the history of a git repository on the fly, both when you pull from and push to it. I reached out to the josh developers to get their input on how a git silo for a contractor could be created that allows for sharing parts of a monorepo in a secure way. They suggested pairing josh with a reverse proxy that adds basic credentials to the requests to the git HTTP endpoint (so a third party does not require access to the credendials) and also rewrites URLs to force checkouts through a josh workspace and exlude the workspace file itself. 

The result is a way to check out josh-rewritten part of an existing repo without requiring access to the original repo. You can find my proof of concept at [github.com/sschaetz/git-silo](https://github.com/sschaetz/git-silo). It is a proof of concept only though. But I learned that josh developers are working on a product based on this and similar ideas at [metahead.dev](https://www.metahead.dev/). I am wondering if their ability to re-write the git history on the fly will change how we use git and how we create and maintain monorepos.


### Copybara

Another more well known tool that does something similar is [copybara](https://github.com/google/copybara). Instead of the implicit transformation done by josh, copybara transforms code explicitly between repositories. A common use-case for Google (the maintainer of the tool) involves maintaining a confidential repository and a public repository in sync.

Now having to run copybara _manually_ is not great; the best setup would automatically sync repos whenever chanages are applied to either the sub-repo (copybara calls it the destination) or the monorepo (called source-of-truth in copybara). This is where [copybara-action](https://github.com/Olivr/copybara-action) enters the stage. It can be set up to mirro pull requests in the destination repo to the source-of-truth and automatically update the trunk branch in the destination when the trunk branch of the source-of-truth changes. The original GitHub action is somewhat out of date so I forked it and upated some aspects of it: [
github.com/sschaetz/copybara-action](https://github.com/sschaetz/copybara-action).


### What about alternatives?

[Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) or some of its alternatives ([subtree](https://git.kernel.org/pub/scm/git/git.git/plain/contrib/subtree/git-subtree.txt), [subrepo](https://github.com/ingydotnet/git-subrepo)) or other multi-repository tools like [subtree split](https://github.com/splitsh/lite) or [git-repo](https://gerrit.googlesource.com/git-repo/) also address the issue of siloing and scaling issues of a large git monorepo.

But I would argue they put too much burden on the individual developer. Take submodules for example: not cloning the submodules is an issue and submodule getting out of sync when you pull are just minor annoyances. Yes, they can be worked around
with extra tooling or improved defaults like (`git config --global submodule.recurse true`) but dealing with multiple pull requests, is not trivial. And what if there are multiple pending changes? The fact that changes are not automatically _atomic_ can lead to issues and confusion.

I should however caveat this criticism by mentioning that I have no professional experience with these tools other than submodules.

## The Future

I'm excited about the future and hope that a new wave of developer tool innovation will improve how we deal with monorepos. I'm optimistic and also excited about the steady acceleration of polyglot cross-platform [build tools](https://bazel.build/).

Or maybe it will all become less important for humans to manage the details of code revisions.
