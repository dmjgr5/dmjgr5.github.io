---
layout: default
title: Git Command
parent: Git & Github
grand_parent: CI/CD Tools
permalink: /docs/cicdtools/gitgithub-command
nav_order: 10
---

# Git Command
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---


## 단계별 명령어

### Git 폴더 생성하기

`PS D:\codingapple_git> git add .`

### Staging / Commit 하기

`PS D:\codingapple_git> git add .`

`PS D:\codingapple_git> git commit -m "save..."`

### Git 이력 보기

`PS D:\codingapple_git> git log --oneline`

### 로컬 레포지토리에서 Main 브랜치명 변경하기

`PS D:\codingapple_git> git branch -M main`

### Push 하기

`PS D:\codingapple_git> git push -u https://github.com/dmjgr5/gitlesson.git main`

### remote 대상을 origin 으로 저장하기

`PS D:\codingapple_git> git remote add origin https://github.com/dmjgr5/gitlesson.git`

### 현재 remote 대상 정보보기  

`PS D:\codingapple_git> git remote -v`

### origin 주소의 main 브랜치로 Push 하기

`PS D:\codingapple_git> git push origin main`

 
{: .note-title }
> Github 에 다른 사람에 의해 Push 된게 있다면?
>
> push 되지 않는다. `git pull [git 주소]` 이후 `git push` 하여야 한다. 따라서 git push 이전에는 git pull 을 하여야 한다.

 

```bash
PS D:\codingapple_git> git push origin main
To https://github.com/dmjgr5/gitlesson.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/dmjgr5/gitlesson.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

PS D:\codingapple_git> git pull https://github.com/dmjgr5/gitlesson.git
From https://github.com/dmjgr5/gitlesson
 * branch            main       -> FETCH_HEAD
Merge made by the 'ort' strategy.
 zzz | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 zzz

PS D:\codingapple_git> git push origin main
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 473 bytes | 473.00 KiB/s, done.
Total 5 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/dmjgr5/gitlesson.git
   48a79a9..a75f918  main -> main
```

---

## 협업 시 간단 절차

1.  개발자는 자기 브랜치로 만듦
2.  `PS D:\codingapple_git> git branch mining PS D:\codingapple_git> git switch mining`
3.  개발 후 Push 자기브랜치
4.  github 에서 pull request
5.  리뷰어는 pull request 에서 merge
    -   충돌나는게 있다면 파일별로 수정하여 resolve 처리 이후 merge
