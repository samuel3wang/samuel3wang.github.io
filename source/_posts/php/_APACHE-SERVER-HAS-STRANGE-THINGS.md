---
title: MacOS and brew Apache Server 遇到的小問題
date: 2023-10-29 00:19:19
categories: [PHP]
description: 在使用 Apache Server 時，因為看到古早的文件，所以想要嘗試使用，但在安裝後出現一些奇怪的問題，所以在這邊紀錄一下。
---
## 前言
在 MAC 上使用想要建立 Apache Server，一開始先用 brew 安裝，之後遇到一些奇怪的問題。

## 過程
從 brew 安裝完後，先用 `brew services start httpd` 啟動，會出現權限不夠的問題，加上 sudo 並打開 localhost:8080 之後出現 nginx 的頁面。
以為自己安裝錯誤，所以就重安裝一次，但出現同樣的結果，就在 config (/opt/homebrew/etc/httpd/httpd.conf) 中去找導入的資料夾 (/opt/homebrew/var/www)，結果發現 `index.html` 裡面是 nginx 的頁面 (可惡被耍了)，以為這樣就會結束，但不小心看到原來 mac 在系統裡面會安裝 Apache server ...

秉持著有裝過的東西就不要再裝一次，就嘗試著要去使用內建的 Apache server，先是執行 `sudo apachectl start` 馬上遇到
```
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using MacBook-Pro.local. Set the 'ServerName' directive globally to suppress this message
httpd (pid 62066) already running
```
先是暫停原本的 `brew httpd service`，再去更改 /etc/apache2/httpd.conf 中的 ServerName，結果試過蠻多方法都沒辦法成功，最後是先將 brew 安裝的 httpd 移除，再去 `sudo apachectl restart`，結果成功了，但這裡因為預設 port 是 80，在瀏覽器輸入 localhost 就會出現 It works!

這時候我想要試一下去更改 config and html 的內容，在是從 /etc/apache2/httpd.conf DocumentRoot 找到導入資料夾位在 /Library/WebServer/Documents，在資料夾下面放了自己寫的 php file 卻發現不像教學一樣可以執行，而是直接回了整份 code 的內容。

再次回去 config 中發現這句話 
> #PHP was deprecated in macOS 11 and removed from macOS 12
試了一些方法卻還是沒辦法 import php，本來氣到想直接將 os 裡面裝的 Apache server 整包刪掉，但查一下發現不建議這麼做，不要用就先放著，否則會殺敵一千可能自損很多。

## 結論
最後就先把內建的 Apache server 暫時關掉，再去使用 brew 安裝的 httpd，希望之後可以順利使用。