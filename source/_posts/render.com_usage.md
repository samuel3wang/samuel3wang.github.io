---
title: Deploy the Website on Render.com
date: 2023-08-26 22:22:52
tags: Render
description: 使用 Render.com 部署前後端以及資料庫組成的個人網站，並申請一個自己的 domain name，雖然使用這類部署工具可以減少自己去設定 web server 的時間，不過在熟悉 Nginx 與各種網路概念之後，一定要使用不同方式再來部署一次。
---
# 將個人網站部署到 Render

### 前言
原先網路上最多人使用的無非是免費的 Heroku，不過在2022年底取消了免費方案，開始尋覓該使用那一套部署的管理工具。在海巡以及比較了多方的服務之後，最後選擇了 Render.com，其實功能上來說，低流量的服務大同小異，會選擇原因是使用社群廣大，可以很好找到問題解答，以及官方文件教學寫得很詳細。


### 部署內容
- 前端 React
- 後端 Golang
- 資料庫 PosrgreSQL

### 資料庫 PosrgreSQL
在 Render 裡面有提供部署 PostgreSQL 的選項，主要就是將連線資訊填入 pgAdmin 中，前後大概10分鐘即可完成。雖然免費版本只提供部署一組資料庫，不過對我來說已經足夠，之後有需要再升級即可。

![](https://i.imgur.com/79mEvXA.png)
>填入Name與選擇方案就創建資料庫

這邊就是架設在 Render 上的資料庫資訊，需要將這些資訊填入後端連線的設定裡

![](https://i.imgur.com/757UjnX.png)

打開pgAdmin將資料庫的資訊填入`Register < Server`

![](https://i.imgur.com/HLihRyf.png)
> 這裡Name需要跟一開始的相同

在Connection內填入Render提供的資料，這邊在`Hostname/address`需要將Hostname帶入`oregon-postgres.render.com`，當初因為眼殘沒看到想說怎麼一直連不上，下面直接複製填進去就好

![](https://i.imgur.com/BBn7wyS.png)

Save之後就可以從pgAdmin去看是否有成功註冊。

### 後端 Golang
在 Render Web Service 裡面提供了多種語言環境供部署，這邊我使用 Gin 框架，有官方文件可以直接參考。

由於後端需要從DB去撈資料，DB 也已經成功部署了，在進階設定的環境參數中設定好要從後端程式要抓的連線資訊就好，Go 可以在程式中使用 os 套件去抓即可 `dbHost := os.Getenv("DB_HOST")`。

![](https://i.imgur.com/VpchEOT.png)


### 前端 React
前端部份花了最多時間，一開始本來想要使用 Static Site 去部署就好，因為版本衝突以及部署時間久到很奇怪。就去使用了 Web Services 的node.js環境，也卡在 node 跟套件不一的問題，在設定中指定 node 版本也無法成功，只好轉去使用Docker。

而因為我使用的是免費方案，只有容量 512MB，如果 Docker Image 不使用 muti-stage build 會因為容量過大而無法部署。

```dockerfile!
# Build stage
FROM node:lts AS builder
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install
COPY . ./
RUN yarn run build

# Deploy stage
FROM nginx:1.19.0
WORKDIR /usr/share/nginx/html
COPY --from=builder /app/build .
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```
>分為Build Stage以及Deploy Stage


另外串接後端 API，需要先去部署完的後端服務下面會有網址，可以使用 Postman 去打打看後端跟資料庫串接有沒有問題。
![](https://i.imgur.com/Pe2eopg.png)

成功之後就可以在前端服務的環境變數內設定 API，設定方式跟資料庫連線資料相同。前端程式我使用`dotenv`去抓取 URL，在 webpack 中宣告`dotenv`方法，在使用`Template literal`去讀取。

### 網域
在部署完前端可以使用URL去訪問網站之後，就想要既然都到這裡了，不如買一個網域來玩。可以購買網域的網站有蠻多的，例如：`GoDaddy`、`Google Domain`、`NameCheap`、`Porkbun`，我則是因為常看到`GoDaddy`的廣告就選了，其實還因為第一年很便宜。

接下來要去設定DNS，將購買的網域填到 Render.com 的 `Custom Domains`內，會生出一段主機位只需要填到 GoDaddy.com DNS設定中的類型`A`，再將前端部署完的那端網址放到名稱`www`，之後就開始等了，網路上很多人說可能會等數小時，但我其實過20分鐘就可以用了。

![](https://i.imgur.com/T827W2T.png)
![](https://i.imgur.com/CZoNyAH.png)

### 後續
從每次部署我可以從其他電腦去拿到資料都很有成就感，尤其是最後真的可以從自己購買的網域連進去個人網站時，雖然其中遇到的問題加上自己眼殘少設定一堆東西都要再回去找，覺得會不會就斷在這裡了，還好最後都有解決完成部署。

最後再提到一下 Render.com 可以設定每次在有 Git Commit 時都自動部署，真的相當方便啊。 