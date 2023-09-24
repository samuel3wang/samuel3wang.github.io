---
title: 在 Hexo 紀錄怎麼建起 Hexo Blog
date: 2023-08-06 15:41:45
tags: [Note]
description: 在建立H exo Blog 的過程中，記錄一些常用的指令，包括 open a server, add new post ,deploy on GitHub...，這樣就可以在需要的時候直接看到怎麼使用，不用再去翻文件，讚。
---

## Add a new post
`hexo new <"new_post_name">`

## One-command Deploy on GitHub
1. 安裝 hexo-deployer-git
`npm install hexo-deployer-git --save`

2. 在 GitHub 上建立一個新的 Repository，名稱為 <username>.github.io
這裡的 <username> 是GitHub帳號名稱，因為跳過這一步驟一直卡在404

3. 在 _config.yml 設定 deploy
```yaml
deploy:
  type: git
  repo: https://github.com/samuel3wang/samuel3wang.github.io
  # example repo: https://github.com/<username>/<username>.github.io
  branch: gh-pages
```

4. Deploy command
`hexo clean & hexo deploy`

## Change the Theme
建議選 Star 數量多的，Bug 少的，更新頻率高的
Themes: https://hexo.io/themes/

1. 在 themes 資料夾下 install theme，以我選的 next 為例
`git clone https://github.com/theme-next/hexo-theme-next themes/next`

2. Edit _config.yml
修改原本的 _config.yml 下的 Extensions > theme: landscape 改成 next