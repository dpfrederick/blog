---
layout: post
title: Managing Git Credentials
date: 2023-12-11 16:00:00 -0500
tags: software git configuration
---

Git is a distributed version control system that is used to track changes to files in a project. It is the most popular version control system in use today. Git is used by developers to track changes to source code, but it can also be used to track changes to any type of file. Git is also used by DevOps engineers to track changes to infrastructure as code files.

I have two Github accounts; one for work and one for personal projects. Sometimes I want to work on my personal projects from my work laptop, and I want to make sure that I'm using my personal github account and the ssh key that's associated with that account.

You can configure Git to use a specific ssh key with the `GIT_SSH_COMMAND` environment variable. Below you can see the command that I use to set the variable (on a Mac).

```bash
export GIT_SSH_COMMAND="/usr/bin/ssh -i /Users/<username>/.ssh/<path-to-private-key>"
```

You'll also want to make sure you're using the correct email address to sign the commits. For this you can use the `git config` command.

```bash
git config user.email "foo@example.com
```

This will set the email for the current repository. You can use the `--global` flag to set the email globally. You can also use a `.gitconfig` file to set the email per repo, or update the global `.gitconfig` file.

*Example repo .gitconfig*
```bash
[user]
    name = Foobar Man
    email = foo@example.com
```
