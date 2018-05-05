---
layout: post
title: Upgrade OS X git to 2.7.4
date: '2016-03-20T21:47:53+08:00'
tags:
- git
- osx
- upgrade
- homebrew
tumblr_url: http://hi-tips.tumblr.com/post/141413870326/upgrade-os-x-git-to-274
---

Due to the [CVE-2016-2324, CVE-2016‑2315](https://ma.ttias.be/remote-code-execution-git-versions-client-server-2-7-1-cve-2016-2324-cve-2016-2315/), we should upgrade our git client immediatly to prevent remote code execution :p.

In OS X, git comes with XCode Developer tool, but if we want to use the lastest git, it can be installed through Homebrew.

```bash
brew install git
which git # check git path
```

For second command, you may find that the path still in /usr/bin/git.

Edit your shell configuration file(`~/.bash_profile` or `~/.profile` or even `~/.zshrc`) to place `/usr/local/bin/git` before `/usr/bin`

For example

```bash
export PATH="/usr/local/bin/git:$PATH"
```

start a new shell check git version by

```bash
git --version
```

![](https://i.imgur.com/FrHKOmD.png)

Also remember to set SourceTree git to System provided.

![](https://i.imgur.com/U9e2aDW.png)
