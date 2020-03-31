---
author: Lucas Ramage
date: 2020-03-31
---

# March 2020 - Developer Update

**TL;DR: We're working on it!**

## Introduction

Last January, I created an issue the following issue:

> # Publish PRoot / Care Development Status Update #2
> I think it would be ideal to create a blog post describing the current 
> state of PRoot i.e. the transition of maintainership from the original author,
> and the issues faced by the current maintainers leading to the large gap since the last release.

Unfortunately, that never happened...

Let's face it, [everyone is busy](https://vcwithme.co/2020/02/06/everyone-is-busy). I get it.
But it's hard for potential new contributors to get on board with our project if it looks dead!

So let's [just do it](https://en.wikipedia.org/wiki/Just_Do_It).

## Maintainership Transition

PRoot was originally developed by [Cedric Vincent](https://github.com/cedric-vincent) at STMicroelectronics, (See: [AUTHORS](https://github.com/proot-me/proot/blob/master/AUTHORS)).
He was involved up until the [release of v5.1.0 on December 18, 2014](https://github.com/proot-me/proot/releases/tag/v5.1.0).
After that, his involvment continually decreased and eventually stopped.

A few years later (November 3, 2016), a group of developers stepped up to maintain the project, (See: [https://github.com/proot-me/proot/issues/106](https://web.archive.org/web/20200331144358/https://github.com/proot-me/proot/issues/106)).

Unfortunately, none of these developers are still active, (See: [Removing Inactive Members](../posts/org-members.md)).

## Recent Improvements

In the last year or so we have made the following improvements:

- [Core Infrastructure Initiative (CII) Best Practices](https://bestpractices.coreinfrastructure.org/en/projects/2444)
- [GitLab CI/CD Pipelines for builds](https://gitlab.com/proot/proot)
- <s>[Google Summer of Code](https://github.com/proot-me/blog/issues/3)</s>
- [Package for Alpine Linux](https://git.alpinelinux.org/aports/commit/?id=e5bc64161b3a4b079fa324bcb3a52e2303d17c08)
- [Package for Void Linux](https://github.com/proot-me/proot/commit/037e77ef796cf4f10e170007a9929bdc400ca3de)
- [Python Extension](https://github.com/proot-me/proot/pull/82)
- [Rename loader binaries](https://github.com/proot-me/proot/commit/20d619d907e7c80dc33b884a93d0645c7ea96cc2)
- [StackOverflow Advertisement](https://github.com/proot-me/blog/issues/7)

## Current Status

We still have lots of work to do. Most importantly, we need to fix the [seccomp bug](https://github.com/proot-me/proot/issues/106),
and also we need to [overhaul the testsuite](https://github.com/proot-me/proot/issues/164). I am glad to see various contributors
submitting patches and creating issues, and I look forward to working together with everyone to maintain this project.
