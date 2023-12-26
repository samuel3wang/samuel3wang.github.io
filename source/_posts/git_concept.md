---
title: 一直記不起來的 Git 概念
date: 2023-08-06 17:09:01
categories: [Git]
description: 身為軟體工程師，不管是用哪一種版控工具，總是脫離不了 git 的各種操作。因為不是所有指令都每天在用，記錄一些每次遇到都會忘記，看文件還是霧煞煞的概念，用自己的語言記錄下來，節省一些從別人的語言轉換成自己邏輯的時間。
---
# The Opperation between Local and Remote Repository
## Situation
有多人同時開發，main 為主要 branch，每個人都有自己的 branch，開發完後要合併到 main branch。需要在 local 建立兩個 branch，一個是 main，一個是自己的 branch，在自己的 branch 開發完後，先將 main branch 更新到最新的狀態，再將 main branch 合併到自己的 branch，將自己的 branch push 到 remote，再發 PR 到 main branch。

## Branch
Create Local Branch
`git branch <branch-name>`

Delete Local Branch
`git branch -d <branch-name>`

List All Local Branches
`git branch`

List All Remote Branches
`git branch -r`

List All Local and Remote Branches 
`git branch -a`

Switch to Branch
`git checkout <branch-name>`

Create and Switch to the new branch
`git checkout -b <branch-name>`

Create Remote Branch
`git push origin <branch-name>`

將指定的 local and remote branch 連結起來，`-u` = `--set-upstream`
`git branch -u origin/<remote-branch> <local-branch>`

## Merge
Merge Branch 將指定的 branch 合併到當前的 branch，需要先 checkout 到當前的 branch
`git merge <branch-name>`

## Pull
Local 需要透過 git pull 來更新 Local 的資料
`git pull`

指定 Remote Branch 來更新 Local Branch
`git pull <remote_name> <remote_branch>:<local_branch>`

# Git Commit and Rebase
## Situation
在開發的過程中，會有很多次的 commit，但是有些 commit 是不需要的，例如：debug commit，或是 commit message 打錯字，這時候就需要用到 rebase 來整理 commit。
依照我遇到的情境，在 commit 之後，因為 review 還沒做完又有下一次的 commit，如果連續 commit，會將 version control 很亂，如果這些 commit 的內容都是同一個功能，就可以使用 rebase 來整理 commit。

## Commit
## Rebase

## Reference
[Git | 我以為的 Git Rebase 與和 Git Merge 做合併分支的差異](https://medium.com/starbugs/git-我以為的-git-rebase-與和-git-merge-做合併分支的差異-cacd3f45294d)
[送 PR 前，使用 Git rebase 來整理你的 commit 吧！](https://medium.com/starbugs/use-git-interactive-rebase-to-organize-commits-85e692b46dd)
[為什麼我愛用"git rebase"](https://billyyyyy3320.com/zh/2019/08/15/why-i-like-git-rebase/)