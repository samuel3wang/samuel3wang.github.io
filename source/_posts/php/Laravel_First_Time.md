---
title: Laravel 初次踏入
date: 2023-10-09 13:05:41
categories: [PHP]
description: 在初次接觸 Laravel 時，安裝使用的過程中記錄一些內容，包括 Laravel 相關功能指令、目錄結構、以及一些常用的套件。
---
## 相關功能指令
因為使用的 macOS，所以可以使用 brew 的安裝皆會使用 brew 安裝。

### Laravel
創建一個新的 project，並可以透過 valet 去建立 http 入口。
```zsh
laravel new <new_project>
```
在新建一個新的 Laravel Project，會有官方提供的 Starter kit 可以選擇(Breeze - more simple, Jetstream)，接下來是前端會用什麼框架來撰寫(純粹 API, 模板語言, 主流框架)，再來會是 optional 的設定，測試方式( PHPUnit, Pest - 完全兼容 PHPUnit)，最後還可以選擇 Database( PostgreSQL, MySQL, SQLite, SQL Server)。

### MySQL
```zsh
brew services install mysql

brew services start mysql
brew services stop mysql

# create a new password
mysql_secure_installation

# access to mysql
mysql -u root -p
```

### Valet
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

## Laravel Project 目錄結構
- App 核心會放在這裡
  - console
    - 包含自定義的 artisan commands
  - events
    - 會在 `event` make or generate 時產生，功能定義在有指定東作發生時，可以觸發其他 application。
  - exceptions
    - Error Handling
  - http
    - 處理大部分 application requests
      - controllers
      - middlewares
      - form
  - jobs
    - 需要透過 `make:job` 去產生，用來處理 queue jobs
  - listens
    - 需要執行 `event:generate` and `make:listener`去產生，用來處理是件監聽的 response。
  - mail
    - 需要透過 `make:mail` 去產生，用來處理 mail 相關功能，包括 class, template or config。
  - notifications
    - 需要透過 `make:notification` 去產生，用來處理通知相關功能。
  - policites
    - 需要透過 `make:policy` 去產生，用來authorization class。
  - providers
    - 註冊各種服務、綁定 class、執行任務以及對框架進行配置(其實看不太懂，帶補充)
  - rules
    - 需要透過 `make:rule` 去產生，自定義驗證規則
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

## Blade Template
使用 Laravel 時，會使用到 Blade Template，他的用處會是在將 html 寫進去 php 中，就可以透過模板的方式將 html 套用到不同的頁面上，而且可以使用 php 的語法，語法會用 @section, @yield, @extends ... 來定義。
在資料夾的結構目錄中，會放在 resources/views 下面，如果有額外分層就會從 views 下面開始，例如 resources/views/admin/index.blade.php。

