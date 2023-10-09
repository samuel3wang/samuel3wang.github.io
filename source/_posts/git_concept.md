---
title: 一直記不起來的 Git 概念
date: 2023-08-06 17:09:01
tags: Git
description: 身為軟體工程師，不管是用哪一種版控工具，總是脫離不了 git 的各種操作。因為不是所有指令都每天在用，記錄一些每次遇到都會忘記，看文件還是霧煞煞的概念，用自己的語言記錄下來，節省一些從別人的語言轉換成自己邏輯的時間。
---
# Branch
## Local
Create Local Branch
```shell
git branch <branch-name>

// create and switch to new branch
git checkout -b <branch-name>
```
Switch to Branch
`git checkout <branch-name>`

Delete Local Branch
`git branch -d <branch-name>`

## Remote
Create Remote Branch
`git push origin <branch-name>`

## Merge
Merge Branch
`git merge <branch-name>`

Local 需要透過 git pull 來更新 Local 的資料
`git pull`