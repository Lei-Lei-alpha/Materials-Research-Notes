---
title: Git basics
author: Lei Lei
category: notes
layout: post
---

## Install Git

Git appears to come as standard as part of Linux. You can test this by running:

```shell
$ git --version
git version 2.17.1
```

If for some reason Git is not installed then you can simply pull it down:

```shell
$ sudo apt install git
```

On windows, I'd recommend to use [GitHub Desktop]([GitHub Desktop | Simple collaboration from your desktop](https://desktop.github.com/)) app + [VS code]([Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)).

## Setup global configuration

Firstly, you need to configure your name and email address, e.g:

```shell
$ git config --global user.name "Lei-Lei-alpha"
$ git config --global user.email "Lei.Lei2@nottingham.ac.uk"
```

This is used to mark you as the author of the changes you make. I also recommend turning off the auto line endings feature as this can hide certain issues especially when working with both Linux and Windows. There is another config setting to achieve this:

```shell
$ git config --global core.autocrlf false
```

If you prefer to use `vim` as your default editor, you can set up by:

```shell
git config --global core.editor "vim"
```

You can just edit the global config for the current user directly:

```shell
$ git config --global --edit
```

This does use the default editor, which seems to default to [Nano](https://www.nano-editor.org/). If you’ve never used Nano before you may prefer to just edit the file directly with something like [Vi](http://ex-vi.sourceforge.net/):

```shell
$ vi ~/.gitconfig
```

Anyway this file should look something like:

```
# This is Git's per-user configuration file.
[user]
        name = Lei-Lei-alpha
        email = "Lei.Lei2@nottingham.ac.uk"

[core]
        autocrlf = false
```

## Check configuration

If you want to check your settings you can run:

```shell
$ git config --list --show-origin
file:/home/pete/.gitconfig      user.name=Lei-Lei-alpha
file:/home/pete/.gitconfig      user.email=Lei.Lei2@nottingham.ac.uk
file:/home/pete/.gitconfig      core.autocrlf=false
```

This shows you the settings and the value they have been set to. It also shows which configuration file the values come from so if you run this within a git repository for example there may be some extra project specific settings included.

## Prepare your ssh key pair

If you have an SSH key already, you can use it rather than creating a new one. (**Note** that PuTTY keys do not work here, but you can convert the PuTTY keys to supported format). Make the `~/.ssh` directory if it does not already exist. Then copy you key pair to the directory. Linux has some rules about how visible the key is. So you need to make sure the access levels are strict enough to meet these requirements:

```shell
mkdir ~/.ssh
chmod 700 ~/.ssh
cp /mnt/c/Users/stcik/.ssh/id_rsa* ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

|     Element     |    Permission    |
| :-------------: | :--------------: |
| .ssh directory  | 700 (drwx------) |
|   public keys   | 644 (-rw-r--r--) |
|  private keys   | 600 (-rw-------) |
| authorized_keys | 600 (-rw-------) |
|   known_hosts   | 600 (-rw-------) |
|     config      | 600 (-rw-------) |

## Add your public key

To make use of your key, the public key needs to be added to remote systems. This will then allow secure connections using your private key. The details of the public key can be retrieved with:

```shell
$ cat ~/.ssh/id_rsa.pub
```

Add this public SSH key to the services you use e.g. [GitHub](https://github.com/) and [Bitbucket](https://bitbucket.org/). You may also want to add it to the _authorized_keys_ file on remote servers to allow SSH access using the key.

## Connect to Github using your `ssh` key pairs

It is always good to check the `ssh `version installed.

```shell
ssh -V
```

You will need to start `ssh-agent` to add your `ssh` key pairs.

```shell
eval "$(ssh-agent -s)"
```

Use the following command to add the `ssh` key pairs stored in the `~/.ssh` directory.

```shell
ssh-add ~/.ssh/id_rsa
```

```shell
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_rsa
```

Once this is done, you should be able to connect to github and add commits to your code without further authentication.

```shell
ssh -T git@github.com
```

## Github token for authentication

Generate follow the steps on GitHub webpage.

To clone a private repository from remote:

```
git clone https://<username>:<githubtoken>@github.com/<username>/<repositoryname>.git
```

## Create repository

### On your machine

Use `git init` to create a new repository from a folder (with existing code).

```python
git init # this creates a repository from current directory
git init project_path # this creates a repository from the folder project_path
```

### On webpage/ GitHub desktop app

Follow the steps, then upload your code.

## Basic commands

### Add identity

```bash
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_rsa
```

### Commit changes

The `git commit` command captures a snapshot of the project's currently staged changes. Committed snapshots can be thought of as “safe” versions of a project—Git will never change them unless you explicitly ask it to. Prior to the execution of `git commit`, The `git add` command is used to promote or 'stage' changes to the project that will be stored in a commit. These two commands `git commit` and `git add` are two of the most frequently used.

```bash
git commit
```

Commit the staged snapshot. This will launch a text editor prompting you for a commit message. After you’ve entered a message, save the file and close the editor to create the actual commit.

```bash
git commit -a
```

Commit a snapshot of all changes in the working directory. This only includes modifications to tracked files (those that have been added with `git add` at some point in their history).

```bash
git commit -m "commit message"
```

A shortcut command that immediately creates a commit with a passed commit message. By default, `git commit` will open up the locally configured text editor, and prompt for a commit message to be entered. Passing the `-m` option will forgo the text editor prompt in-favor of an inline message.

```bash
git commit -am "commit message"
```

A power user shortcut command that combines the `-a` and `-m` options. This combination immediately creates a commit of all the staged changes and takes an inline commit message.

```css
git commit --amend
```

This option adds another level of functionality to the commit command. Passing this option will modify the last commit. Instead of creating a new commit, staged changes will be added to the previous commit. This command will open up the system's configured text editor and prompt to change the previously specified commit message.

#### Quick commands

```bash
git add -A && git commit -m "Changes"
```

### Pull & merge + push

```shell
git pull origin main && git push origin main
```

## Origin & main

When we want to contribute to a git project, we need to make sure how to manage the remote repositories. One can push and pull data from a remote repository when you need to share work with teams. **Origin** and **Main** are two different terminologies used when working and managing the git projects.  

*   **Origin** is the name used for the remote repository.
*   **Main** is the name of the branch.

### Git – Origin

Origin in simple words means from where something is originated or derived.

*   Origin is simply the name given to any remote repository available on GitHub.
*   Whenever we need to push the changes to a remote repository, we use git push along with the remote repository “origin” and “main” branches. The term used is “**git push origin main**“.
*   To pull the changes from the remote repository to local, we use git pull along with remote repository “origin” and “main” branch. The term used is “**git pull origin main**“.

### Git – Main

**Main** is the name of a default branch in git terminology. Whenever a new repository is created in git, git gives the default name to a branch as ‘Main’.

*   When a new repository is initialised using “**git init**” command, git creates a single branch by default such as the “**Main**” branch.
*   When multiple developers collaborate on a single feature/development work, developers create a pull request to merge the changes to Main branch. After the review is done by the senior developer, changes are merged to the Main branch.
*   The Main branch is the most up-to-date branch and has production-ready code.
