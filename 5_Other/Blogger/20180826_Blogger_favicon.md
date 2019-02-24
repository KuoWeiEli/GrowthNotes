# Blogger 筆記（2）：Blogger 小圖示
網頁的小圖示，英文稱為 Favicon，通常是一個網站 Logo。Blogger 的小圖示當然就是自家的 Logo 阿~~
![2_blogger_favicon8.png](/5_Other/Blogger/Image/2_blogger_favicon8.png "2_blogger_favicon8.png")

什麼？你說有沒有辦法更換這個小圖示，想要有自己的 Style？當然沒問題，Blogger 提供這個功能，讓你替換屬於自己的 **Favicon**！

什麼？你又說，你不會設計，可是又想要美美的 Favicon？
當然沒問題，來來來！ 讓筆者來介紹一下這個網站 [ICONFINDER](www.iconfinder.com "ICONFINDER")。
![2_blogger_favicon9.png](/5_Other/Blogger/Image/2_blogger_favicon9.png "2_blogger_favicon9.png")

## 主題下載說明
#### 進入 [ICONFINDER](www.iconfinder.com "ICONFINDER") 網站
首先進入這個網站，ICONFINDER 目前提供約莫200萬個以上的 Icon，反正就是很多很多，多到應該可以找到符合自己胃口的！😄
![2_blogger_favicon2.png](/5_Other/Blogger/Image/2_blogger_favicon2.png "2_blogger_favicon2.png")

#### 尋找圖片
打上想要的 Icon 類別，在這裡打上青蛙 **Frog**，就有圖片出來讓你選擇。有付費的也有免費的，當然如果你像筆者的荷包隨時準備發動罷工的話，左邊選擇 `Free`，就可以選擇免費的圖片下載使用囉！個人覺得免費的 Icon 也很好看！在這裡選擇紅框的**黑青蛙 Icon**。
![2_blogger_favicon3.png](/5_Other/Blogger/Image/2_blogger_favicon3.png "2_blogger_favicon3.png")

#### 下載圖片
點擊進去後，可以看到網站提供了許多格式的檔案供下載。一般來說，Favicon 圖示的副檔名是 `.ico`。但是 Blogger 可提供其他格式的使用（但是只能上傳**100KB**，這裡 `.ico` 檔案下載下來都超過大小）。因此這裡選擇 `.png`  的格式下載！
![2_blogger_favicon4.png](/5_Other/Blogger/Image/2_blogger_favicon4.png "2_blogger_favicon4.png")

#### 設置圖片
下載完成後，到 Blogger 的管理後台，點選`版面配置`，再點選網站小圖示的`編輯`。
![2_blogger_favicon5.png](/5_Other/Blogger/Image/2_blogger_favicon5.png "2_blogger_favicon5.png")

這裡就可以看見**上傳100KB大小的限制**。選擇剛剛下載下來的檔案，接著按`儲存`。
![2_blogger_favicon6.png](/5_Other/Blogger/Image/2_blogger_favicon6.png "2_blogger_favicon6.png")

#### 檢視成果
Favicon 就出現在眾人的眼前囉！如果發現沒有出現 Favicon，可以清除 Cache（如果不清楚如何單獨清理的話，可以使用如　[CCleaner](www.ccleaner.com/ccleaner "前去瞧瞧！")  的工具，來協助你清理），或是開啟不同瀏覽器（如 Edge、Firefox） 就可以囉！
> 如果不使用上述方法，也可以等待一段時間後，待 Cache 失效即可顯示更新後的 Favicon！
![2_blogger_favicon7.png](/5_Other/Blogger/Image/2_blogger_favicon7.png "2_blogger_favicon7.png")

## 如何在Blogger使用自己的圖片
什麼？ 說了這麼多，原來你可以設計屬於自己的 Favicon 阿！但你心想圖片要存放在哪裡？

筆者原本存放在 Google 的相簿，新增一個相簿專門管理 Blogger。但後來決定使用**HTML模式**下**插入圖片**，再這裡介紹兩種方法：

#### 從Google相簿取得
在 Google 相簿點選上傳後的相片，想辦法取得圖片的網址（如 Chrome 是對圖片右鍵，點選**複製圖片網址**）後，可以看到如下的網址格式：
```html
https://lh3.googleusercontent.com/vUGvHrIKZbcBKLRFTXEihIrAK8pIzhrcy7b5zgaJXYzaOSGH4YRmK01KYbCTlcsd7Z-SsxOi0vqz4C6jMlltuEtuGr33mjK9V1mSmxi8rj_-dU-z4FBmuA1EzPetzzvg3jzjOgbek-yL1sQMZ3Y2ED0RXnPCroYZQGDYkXW7UuKoh9-ygNLTTVPJKQo8xU-M5_zniMR8OxiexgnETRxx9W7eaCv0tscQzWtzadR8YroyMdPhQxVA2FcMQXM8tCajgSg4Q8oGMrOMGk2x425E28YjmQ7itLMWaBWtMqoV3LvQZGlGpefPtVEU0RaC2sTeMlnKFB2uxcbf_hTmcOqRvAxpKrq9R2-8tbrDJpw94q7L5iqAxk16NFSONMDxcSQFmw6mOhSyr-badlJoeVFJkhvx-sZGa9Jvuud2oXHu3a31KQ2Se1dIV25IeJsn5fPRnhFqPGeQ1kf3uaYjK13HxywcoQq4rDhfZPfpMcMLGIYnX7VPMDzbwpEarzzrGq2YBYIWwg0BtR7gmeJkNdO4xjkS3CXSxyfakuc8-aaEHfEyYmDoBZJZbUB-4aK7iBJMAdwSXZWbevS4WrO1ofiYzlnHgLo7FGcEjm06f2V5tq-dqXDRjjS1pntsb8nLYNI=w1259-h788-no
```
在網址的最後面可看到有一段參數`w1259-h788`，w1259就是寬度1259px、h788就是高度788px。為了保持照片的寬高比  （才不會使照片變形），只要設定 `w` 後面的數字就行，如 `w800`。

編輯好這段網址後，接著就是將圖片載入文章內，選擇 **HTML 編輯模式**，這樣的好處就是編輯器不會再幫你添加額外的參數，甚至壓縮你的圖片。將下列的 HTML 貼近你想要的位置即可。
```html
<img src="https://lh3.googleusercontent.com/vUGvHrIKZbcBKLRFTXEihIrAK8pIzhrcy7b5zgaJXYzaOSGH4YRmK01KYbCTlcsd7Z-SsxOi0vqz4C6jMlltuEtuGr33mjK9V1mSmxi8rj_-dU-z4FBmuA1EzPetzzvg3jzjOgbek-yL1sQMZ3Y2ED0RXnPCroYZQGDYkXW7UuKoh9-ygNLTTVPJKQo8xU-M5_zniMR8OxiexgnETRxx9W7eaCv0tscQzWtzadR8YroyMdPhQxVA2FcMQXM8tCajgSg4Q8oGMrOMGk2x425E28YjmQ7itLMWaBWtMqoV3LvQZGlGpefPtVEU0RaC2sTeMlnKFB2uxcbf_hTmcOqRvAxpKrq9R2-8tbrDJpw94q7L5iqAxk16NFSONMDxcSQFmw6mOhSyr-badlJoeVFJkhvx-sZGa9Jvuud2oXHu3a31KQ2Se1dIV25IeJsn5fPRnhFqPGeQ1kf3uaYjK13HxywcoQq4rDhfZPfpMcMLGIYnX7VPMDzbwpEarzzrGq2YBYIWwg0BtR7gmeJkNdO4xjkS3CXSxyfakuc8-aaEHfEyYmDoBZJZbUB-4aK7iBJMAdwSXZWbevS4WrO1ofiYzlnHgLo7FGcEjm06f2V5tq-dqXDRjjS1pntsb8nLYNI=w800" />
```

什麼？你說上面這個 HTML 怎麼可以這樣呈現？要怎麼做？別急，慢慢來！下一篇將會介紹到如何使用 **Code Block**！

#### 使用 HTML 模式下的插入圖片
使用這個方法，較為簡單輕鬆。其圖片將會存入 Picasa 圖床，可參照 [Google相簿封存檔案](support.google.com/picasa/answer/7008270?hl=zh-Hant "前去瞧瞧！")。另一方面，其圖片網址將較為簡短，編輯時將不會使代碼變得冗長又難看。
> 部分小工具縮圖如 **Related Posts**，將無法順利顯示**從Google相簿取得的圖片**，這時候用此方法是絕佳的選擇！
![2_blogger_favicon10.png](/5_Other/Blogger/Image/2_blogger_favicon10.png "2_blogger_favicon10.png")
