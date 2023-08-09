---
title: 在Hexo紀錄怎麼建起Hexo Blog
date: 2023-08-06 15:41:45
tags:
---

## 新增一篇新的文章
```shell
hexo new <"new_post_name">
```

## One-command Deploy on GitHub
1. 安裝 hexo-deployer-git
```shell
npm install hexo-deployer-git --save
```

2. 在GitHub上建立一個新的repository，名稱為 <username>.github.io
這裡的 <username> 是GitHub帳號名稱，因為跳過這一步驟一直卡在404

3. 在 _config.yml 設定 deploy
```shell
deploy:
  type: git
  repo: https://github.com/samuel3wang/samuel3wang.github.io
  # example repo: https://github.com/<username>/<username>.github.io
  branch: gh-pages
```

4. Deploy command
```shell
hexo clean & hexo deploy
```