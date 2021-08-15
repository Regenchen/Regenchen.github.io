---
layout: post
title: git checkout is checkmated
tags: Dev
---

Since [Git 2.23](https://www.infoq.com/news/2019/08/git-2-23-switch-restore/), `git switch` and `git restore` have been added to the version control arsenal to switch between branches and restore changes, respectively. This movement aims to put the almighty command `git checkout` on the shelf.

`git checkout` is probably one of the first commands that one should comprehend when getting started with the version control system. However, `git checkout` has a big design flaw within: it does NOT do exactly one thing.

> Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new features.
> 
> -- *UNIX Philosophy by Doug McIlroy*

Let's take a look at the synopsis of `git checkout`.

{% highlight shell %}
NAME
   git-checkout - Switch branches or restore working tree files

SYNOPSIS
   git checkout [-q] [-f] [-m] [<branch>]
   git checkout [-q] [-f] [-m] --detach [<branch>]
   git checkout [-q] [-f] [-m] [--detach] <commit>
   git checkout [-q] [-f] [-m] [[-b|-B|--orphan] <new_branch>] [<start_point>]
   git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <pathspec>...
   git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] --pathspec-from-file=<file> [--pathspec-file-nul]
   git checkout (-p|--patch) [<tree-ish>] [--] [<pathspec>...]
{% endhighlight %}

`git checkout` does two very different things, and accepts very different forms of parameters when doing them. It creates confusions, dysfunctions, and frustrations. To address this problem, the two main functionalities of `git checkout` is now splitted into two: `git switch` and `git restore`. `git checkout` will be, on the other hand, put to euthanasia in the foreseeable future.

And how do we use the `git switch` command? The most used actions are definitely switching to and creating a branch.

- `git switch my-branch` replaces `git checkout my-branch`
- `git switch -c new-branch` replaces `git checkout -b new-branch`

Another action simplified by the addition of `git switch` is creating a local branch that tracks a remote branch. Before `git switch`, one needs to go through the mess of

`git checkout --track origin/remote_branch`,

or even

`git checkout --track -b remote_branch origin/remote_branch`

in earlier git versions. After the update, `git switch remote_branch` will do the job simple and elegant (all commands above creates a local branch named `remote_branch` which tracks the remote branch named `remote_branch`).

## References

- [InfoQ](https://www.infoq.com/news/2019/08/git-2-23-switch-restore/)
- [Offcial doc: git checkout](https://git-scm.com/docs/git-checkout)
- [A blog about git checkout](https://redfin.engineering/two-commits-that-wrecked-the-user-experience-of-git-f0075b77eab1)
