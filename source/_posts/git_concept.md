---
title: 一直記不起來的的Git概念
date: 2023-08-06 17:09:01
tags:
---
## branch
### local
create local branch
```shell
git branch <branch-name>

# create and switch to new branch
git checkout -b <branch-name>
```
switch to branch
```shell
git checkout <branch-name>
```
delete local branch
```shell
git branch -d <branch-name>
```
### remote
create remote branch
```shell
git push origin <branch-name>
```

## merge
merge branch
```shell
git merge <branch-name>
```
local 需要透過 git pull 來更新 local 的資料
```shell
git pull
```