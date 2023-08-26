---
title: 一直記不起來的的Git概念
date: 2023-08-06 17:09:01
tags: [Git]
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