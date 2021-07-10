---
title: Multiple git accounts in one machine
date: 2020-09-11 09:45:00 +05:30
tags: [git]
---

Sometimes we would require to handle multiple git accounts say a personal one and one associated with work. It can become frustrating to switch between accounts while committing or pushing any changes.

## Changing names manually

One way to tackle this is to set the name and email before committing the changes.

```text
git config --global user.name "your_first_name your_last_name"
git config --global user.email "your_email"
```

But this can become very much tedious if one is constantly working between official and personal projects.

## What if we can automate this?

Git's `includeIf` to the rescue.

Say we maintain all our work related repositories in our `Work` folder and similarly all our Personal repositories in the `Personal` folder, we can use different git configurations and include them based on some conditions.

First create a git config for your `Work` environment and name `.gitconfig-work`

```bash
vi ~/.gitconfig-work
```

And add the following lines

```bash
[user]
    name = your-work-name
    email = your-work-email
[core]
    autocrlf = input
    editor = "'/usr/local/bin/atom' -n -w"
[filesystem "Oracle Corporation|1.8.0_131|/dev/disk1s5"]
    timestampResolution = 1002 milliseconds
    minRacyThreshold = 0 nanoseconds
[commit]
    gpgSign = true
[tag]
    gpgSign = true
# Add other work related stuffs
```

Create another config for your `Personal` work environment and name the config file `.gitconfig-personal`.

```bash
vi ~/.gitconfig-personal
```

Add add the following lines

```bash
[user]
    name = your-personal-name
    email = your-personal-email
[commit]
    gpgSign = true
[tag]
    gpgSign = true
# Add other personal work related stuffs
```

Now we go to our main git configuration file

```bash
vi ~/.gitconfig
```

and add the following code

```bash
[includeIf "gitdir:/Users/sathish/Work"]
    path = ~/.gitconfig-work
[includeIf "gitdir:/Users/sathish/Personal"]
    path = ~/.gitconfig-personal
```

Now whenever we commit a file in any repository inside our `Work` folder, git automatically sets the configuration from `.gitconfig-work` and your `your-work-name` and `your-work-email` will be used. Similarly any commits made inside the `Personal` folder will have your `your-personal-name` and `your-personal-email`.

## Let's verify!

```bash
$ cd ~/Work
$ git init work-test-repo
$ git config -l
    credential.helper=osxkeychain
    includeif.gitdir:/Users/sathish/Work/.path=~/.gitconfig-work
    user.name=your-work-name**
    user.email=your-work-email**
    core.autocrlf=input
    core.editor= '/usr/local/bin/atom' -n -w
    filesystem.Oracle Corporation|1.8.0_131|/dev/disk1s5.timestampresolution=1002 milliseconds
    filesystem.Oracle Corporation|1.8.0_131|/dev/disk1s5.minracythreshold=0 nanoseconds
    commit.gpgsign=true
    tag.gpgsign=true
    includeif.gitdir:/Users/sathish/Personal/.path=~/.gitconfig-personal

$ cd ~/Personal
$ git init personal-test-repo
$ git config -l
    credential.helper=osxkeychain
    includeif.gitdir:/Users/sathish/Work/.path=~/.gitconfig-work
    includeif.gitdir:/Users/sathish/Personal/.path=~/.gitconfig-personal
    user.name=your-personal-name**
    user.email=your-personal-email**
    commit.gpgsign=true
    tag.gpgsign=true
```

As you can see above, initializing a repository and running `git config -l` in two different folders gives us two different configurations.
