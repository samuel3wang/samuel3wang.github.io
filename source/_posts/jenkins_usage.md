---
title: Jenkins 使用小記
date: 2023-08-26 22:32:19
tags: [CI/CD]
description: 實習的專案需要用到 Jenkins，第一次用到 CI/CD 工具，雖然用法不是太過完整，不過
在從 Local push code 上 GitLab 時，就可以自動判斷行爲去帶入不同的參數，build 出不同的版本，真的很貼切 icon 是一個拿著毛巾的管家。
---
## 創建新專案
由專案的複雜程度可以選擇使用堆疊積木的方式或是寫 Jenkinsfile 的Pipeline。

![](https://i.imgur.com/NEcvLOg.png)

## 撰寫Jenkinsfile
使用區塊的方式，需要不同步驟分開並且將stage命名，後續除錯與檢視執行時間較方便。

下面 Pipeline Syntax 可以將需要的指令透過設定參數直接產生，對於撰寫 Jenkinsfile 的語法上提供便利性，類似翻譯功能。

因為專案主要是將程式 clone 後打包成執行檔，再使用inno setup去打包成安裝檔，所以在打包程式之前的移動文件需要用到大量的 Batch Script，這邊因為流程跟指令搞不懂卡了一段時間。

```
// xcopy這邊前面後面在移動資料夾跟內部檔案要不要斜線有錯過蠻多次
// 把aaa內部檔案丟到sss
xcopy /Y /s /Q "C:\\test\\aaa\\*" "C:\\test\\sss\\"
// 把test目錄的aaa丟到source目錄下
xcopy /Y /s "C:\\test\\aaa" "C:\\source\\aaa\\"

// 更改內容
sed -i 's/test(\\s*\"default\",\\s* false\\s*)/test(\"default\", true)/g'

//刪除資料夾內所有檔案，不小心設錯目錄，東西會全部消失
rm -r -f *
```
![](https://i.imgur.com/Tqgpt1G.png)

## 選擇參數
Jenkins提供在建置前可以選擇參數的功能，我用來修改設定檔，不過應該可以用來帶入圖片或是改變版本號。

![](https://i.imgur.com/XJFcBBW.png)

![](https://i.imgur.com/52bHTul.png)

![](https://i.imgur.com/7mX6yKk.png)

## Git
在安裝git plugin之後就可以使用，專案採用GitHub。

- **Private Project**

這邊設置credentialsId，讓private project可以clone，將token的值放到Jenkins路徑(左側Manage Jenkins > Security > Manage Credentials)，在domain下面可以找到Add credentials，去填入對應的內容，不太好找，有幾次找很久。
> 這個設定在需要用到credential的時候也會可以設定

![](https://i.imgur.com/umrzSi3.png)

![](https://i.imgur.com/GB2W807.png)

- **Checkout**

使用git clone會有兩種方式，起初用的是git，不過發現很多地方checkout可以加設定進去，就直接使用checkout。
```
// before
git branch: 'main', credentialsId: '*******', url: 'https://github.com/samuel3wang/crudwithgo.git'

// after
checkout ([
  $class: 'GitSCM', 
  branches: [[name: '*/main']],
  doGenerateSubmoduleConfigurations: false, 
  extensions: [], 
  submoduleCfg: [],
  userRemoteConfigs: [[
    credentialsId: '01316a0d-28f1-4f8c-b2fe-35d7a60c950b', 
    url: 'https://gitlab.wise-paas.com/aifs-evolution/aiaa-portal-go',
  ]]
])
```


- **WebHook**

這功能雖然沒用到，不過有試玩一下很有趣，主要是當追蹤的GitLab專案，有變動的時候就會去build，不過因為專案只有在需要的時候才去打包，就沒有用到這個功能了。

設定方式是在Build Trigger下，選有GitLab webhook的選項，設定變動方式，這串URL會在GitLab用到，在Advanced最下面去產生token並且複製。

這邊要先打斷一下，因為要測試webhook，需要有public地址，所以用了ngork將本地的服務映射到公開的地址，地址的後面則是填入剛剛複製的URL的後半段。

接著將URL與token填入，設定Trigger方式，新增之後會有test供各種功能測試，成功會得到status code=200。
![](https://i.imgur.com/cSNf4DL.png)

![](https://i.imgur.com/hU338rx.png)

![](https://i.imgur.com/Qa15hgN.jpg)

![](https://i.imgur.com/1Qgdffu.png)

### 參考資料

[官網pipeline doc](https://www.jenkins.io/doc/book/pipeline/)

[WebHook 觸發本地 Jenkins pipeline](https://zoejoyuliao.medium.com/透過-github-webhook-觸發本地-jenkins-pipeline-讓你-push-code-到-github-就會自動跑-ci-cd-7c4bd7a22446)