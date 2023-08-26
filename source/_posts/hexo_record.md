---
title: 在Hexo紀錄怎麼建起Hexo Blog
date: 2023-08-06 15:41:45
tags:
---

## Add a new post
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

## Edit the configuration file
- to be continued

## Change the theme
建議選star數量多的，bug少的，更新頻率高的，有人在維護的
https://hexo.io/themes/

1. install theme
2. edit _config.yml
