---
layout: default
title: Git CLI
parent: Git & Github
grand_parent: CI/CD Tools
permalink: /docs/cicdtools/gitgithub-gitcli/
nav_order: 9
---

# Git CLI
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---


![example](/assets/images/gitbash.png)

> First install Git Bash from [https://git-scm.com/download/win](https://git-scm.com/download/win)


----

 
### Procedures

- go to the folder to make git managed
    - `git init .` : initialize .git called git repository
- register user information
    - `git config --global user.name "myname"`
    - `git config --global user.email "abc@gmail.com"`
- `git status` : working tree status
- `git add <filename>` : add to Staging Area
    - `git add . `: all files added in current folder, but at least add FIRST
- `git commit -m "history comment" `: goes to Repository, create version

- `git commit -am "filename"` : add & commit at once
- `git log` : shows file version
- `git log stat` : show file version history
- `git diff`  : shows differences

- `git checkout <[VersionNumber]>` : go back to previous version
- `git checkout master` : go to latest version
- `git reset --hard [VersionNumber]` : reset to [VersionNumber] version, strongly deleted.
- `.gitignore` : ignore fires listed in .gitignore file


### Branch and Conflict
- git branch : make branch
- git checkout : go to the branch name, changes HEAD
- base : common ancestor between branches


```bash

$ git log --graph --oneline  # shows oneline
* 8f928c8 (HEAD -> master) work 3
* 185a6da work 2
* d06d25d work 1

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch
* master

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch apple # make apple branch

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch
  apple
* master

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch
  apple
* master

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git log --graph --oneline
* 8f928c8 (HEAD -> master, apple) work 3
* 185a6da work 2
* d06d25d work 1

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch google

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch
  apple
  google
* master

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git branch ms

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git log --graph --oneline
* 8f928c8 (HEAD -> master, ms, google, apple) work 3
* 185a6da work 2
* d06d25d work 1

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ nano work.txt

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git commit -am "master work 4"
warning: LF will be replaced by CRLF in work.txt.
The file will have its original line endings in your working directory
[master 375e49a] master work 4
 1 file changed, 1 insertion(+)

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git log --graph --oneline
* 375e49a (HEAD -> master) master work 4
* 8f928c8 (ms, google, apple) work 3
* 185a6da work 2
* d06d25d work 1

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (master)
$ git checkout apple
Switched to branch 'apple'

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (apple)
$ git log --graph --oneline
* 8f928c8 (HEAD -> apple, ms, google) work 3
* 185a6da work 2
* d06d25d work 1

samsung1@samsung MINGW64 ~/Desktop/gitStudyFolder/manual (apple)
$ git checkout master
Switched to branch 'master'
```