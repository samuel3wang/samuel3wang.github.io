---
title: 一直記不起來的 Git 概念
date: 2023-08-06 17:09:01
categories: [Git]
description: 身為軟體工程師，不管是用哪一種版控工具，總是脫離不了 git 的各種操作。因為不是所有指令都每天在用，記錄一些每次遇到都會忘記，看文件還是霧煞煞的概念，用自己的語言記錄下來，節省一些從別人的語言轉換成自己邏輯的時間。
---
# Branch

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

# Merge
Merge Branch 將指定的 branch 合併到當前的 branch，需要先 checkout 到當前的 branch
`git merge <branch-name>`

# Pull
Local 需要透過 git pull 來更新 Local 的資料
`git pull`

指定 Remote Branch 來更新 Local Branch
`git pull <remote_name> <remote_branch>:<local_branch>`
