---
title: Python 動態爬蟲接案網站紀錄
date: 2024-01-01 15:36:30
categories: [Python]
description: 雖然有耳聞過爬蟲，但是一直沒有動力去操作，剛好在轉職時間想要看去爬接案網站的資料，就順便記錄下來，也練習要怎麼從頭開始去描寫一個完整的概念。
---
# 概念

簡單來說，爬蟲程式之所以可以拿到網頁上的資料，是因為他去抓到我們在瀏覽網頁所看到的文字，這些內容可以從開發人員選項的 element 去看到，裡面會是一堆 HTML。
在程式的角度他要怎麼選擇這些內容就要由使用者去設定他要去抓到哪一些特定的 tag，在 HTML 上稱為 DOM Data Tag，只要設定好之後，就可以拿到相同結構的資料。

以 PTT Gossip 來看， 每一條貼文都是依照相同的結構去顯示的，只要知道標題是在哪一個 tag 下面就可以去抓到這些標題。
![ptt](https://hackmd.io/_uploads/SkQfyglO6.png)

# 動態爬蟲

為什麼會提到動態爬蟲是因為上面的 PTT 其實是屬於靜態網頁，所謂靜態網頁就是在載進去網頁時，Server 會將所有的內容都丟出來，這時候爬蟲就可以將需要的資料全部都拿到，這也會是 google 在爬哪一些資料最符合使用者搜尋的內容所使用的方式。

要如何區分網頁是不是靜態網頁可以在開發人員工具中(這裡用 Chrome)打開右上角的齒輪，將裡面 Debugger 的 Disable JavaScript 打勾，重新整理網頁，去看網頁有沒有如正常使用相同。
若是網頁的瀏覽上沒有任何問題，就可以使用靜態網站的爬蟲方式，若是發現網站東缺西缺，就需要繼續看下去囉。

![debugger](https://hackmd.io/_uploads/Syau-xxdp.png)

## Client-Side Rendering(CSR)

會需要介紹 CSR 是因為，以前的網頁製作模式都會以靜態網頁為主，但現在在前端框架的盛行下，只需要放一個有標籤的容器，在執行的時候去 render  整個畫面。

```html
<body>
  <div id="root"></div>
  <script>
    ...
  </script>
</body>
```

使用 CSR 會導致在爬蟲時只能爬到容器的名字，所以需要去模擬訪問模擬器的行為，這邊就會去使用到 Selenium 這個工具，讓網頁真正的被訪問去 render 出我們需要爬的內容。

## Chrome Driver

在程式中，python 會透過 Driver 開啟 Browser，需要指定位置讓 Driver 可以被打開，如下
```python
service = Service('/path/to/chromedriver')
```

在使用 Selenium 會需要去使用到 [Chrome Driver](https://chromedriver.chromium.org/downloads)，版本需要與電腦上面的 Google Chrome 相同，Chrome 版本位置會在 Settings > About Chrome > Version，這裡的版本對應很重要，關乎 Selenium 可不可以打開 Browser。

現在 Chrome Driver 並沒有提供 115 以後的版本，因為我是使用 MacOS，使用 Brew 跟官網會無法安裝舊版本的 Chrome，[Google Chrome Old Version](https://www.slimjet.com/chrome/google-chrome-old-version.php)，從這裡下載並且安裝，在打開網頁系統會去偵測現在版本並做更新，從 Library 去刪除自動更新包也沒辦法關掉自動更新，這裡試了一些方法都沒有解決。

最後解決的方式是在安裝舊版的 Chrome 之後，不要打開 App，這時候系統會卡在一開始下載的版本，就可以正常使用了。

## 完整程式

```python
# set ChromeDriver
service = Service('/path/to/chromedriver')
driver = webdriver.Chrome(service=service)

try:
  for page_number in range(1, 11):
    # get url
    url = f"https://exampleurl.com&pa={page_number}"
    driver.get(url)

    # wait for page loading
    WebDriverWait(driver, 10).until(EC.presence_of_all_elements_located((By.CSS_SELECTOR, ".editorInfo h2 a")))

    # get names
    names = driver.find_elements(By.CSS_SELECTOR, ".editorInfo h2 a")
    for name in names:
        print(name.text)
finally:
    driver.quit()
```

# Reference

[Day 12 . 欸 今天要幹嘛 - python 動態爬蟲（下）](https://ithelp.ithome.com.tw/articles/10328690)
[動態網頁爬蟲第二道鎖 - Selenium教學：如何使用find_element(s)取得任何網頁上能看到的內容(附Python 程式碼)](https://tmrmds.co/article-mds-operation/16680/)
