---
title: laravel_valet
date: 2023-10-09 13:05:41
tags: Laravel
description: 在初次接觸 Laravel 時，安裝以及使用的過程中記錄一些內容，包括 Laravel 相關功能指令、目錄結構、以及一些常用的套件。
---
## Laravel 相關功能指令
因為使用的 macOS，所以可以使用 brew 的安裝皆會使用 brew 安裝。

### laravel
```zsh
laravel new <new_project>
```

### mysql
```zsh
brew services install mysql
brew services stop mysql
```

### valet
會使用 valet 的原因除了因為輕量之外，更重要的是他專屬於 macOS，專屬的東西是一定要用的。
使用 valet 的目的是可以在 park 指定的資料夾路徑下透過 webserver 去開啟。
- update valet
```zsh
valet stop
valet uninstall
composer global require laravel/valet
valet install
valet start
```
- park a project
cd 到指定資料夾後下 `valet park`，這句指令的用意在於從當下的資料夾去做一個 http 入口，並以資料夾的結構去建立 route。
- link a project
而 `valet link` 指令則是往更下面一層去建立 route，不像 park 的對象是整個資料夾。

## 目錄結構
- App 核心會放在這裡
  - console
  - events
  - exceptions
  - http
  - jobs
  - listens
  - mail
  - notifications
  - policites
  - providers
  - rules
- Bootstrap
  - app.php 用以啟動框架
  - cache file
- Config
- Database
  - database migrations
  - SQLite
- Public
  - index.php enrty point
  - frontend resources
- Resources
  - languages
  - none compiled assets
- Routes
  - web.php
    - RouteServiceProvider
      - session status, CSRF protection and Cookie encryption
  - api.php
    - RouteServiceProvider
      - stateless, 需要使用 token 而不能是 session
  - channels.php
  - console.php
- Storage
  - app 應用程式產生的檔案
  - framework 框架生成的文件 and cache
  - logs
- Tests 
- Vendor composer dependency package
