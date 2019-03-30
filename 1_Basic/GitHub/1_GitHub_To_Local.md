# GitHub 上的專案複製到本機上
如果您剛在 GitHub 上建立了一個 Repository，迫不及待的想要複製到自己的電腦上。然後在命令列工具打出下列指令：`git clone git@github.com:KuoWeiEli/GrowthNotes.git`，緊接著就出現了下列一大堆訊息：

![SSH Error Msg](/1_Basic/GitHub/Image/1_GitHubToLocal1.png "SSH Error Msg")

您很緊張的想著為什麼會出現這種情況，先收起您的冷汗，直接聽筆者的結論吧！
> 您選擇使用 SSH 方式複製，但 GitHub 與您的電腦之間沒有相同的 SSH KEY！

* * * 
### 傳輸方式
不管要上傳專案到 GitHub，還是從自己的 GitHub 下載專案到電腦。有兩種傳輸方式可以選擇：
* HTTPS：藉由 GitHub 網站的帳號密碼來驗證身分是否正確。
* SSH：判斷 GitHub 上的公鑰是否與電腦上的公鑰相同，而私鑰的密碼兩者是否一樣。

### HTTPS 傳輸
HTTPS 比較輕鬆易懂，如下圖所示。
1. 第一步在 GitHub 欲複製的容器選擇使用 HTTPS 方式並複製該網址。
2. 如果本身已經有登入 GitHub 就會跳過第二步的 Login 驗證。
3. 第三步在命令列打上指令 `git clone https://github.com/KuoWeiEli/GrowthNotes.git` 即可下載到電腦上，下載到執行命令列工具的目錄下。因為指令沒有指定目錄名稱，所以默認為 GitHub 容器的名稱 GrowthNotes。

![HTTPS DEMO](/1_Basic/GitHub/Image/1_GitHubToLocal2.png "HTTPS DEMO")

### SSH 傳輸
讓我們再一次思考這句話：
>判斷 GitHub 上的公鑰是否與電腦上的公鑰相同，而私鑰的密碼兩者是否一樣。

第一個想到的是，公鑰私鑰從何而來？答案是：都是從命令列工具下指令產生，統稱金鑰！我們就從產生金鑰的步驟開始吧！

1. 開啟命令列工具，打上 `ssh-keygen` 指令，會產生一連串的對話，設定金鑰儲存位置與密碼，如下圖所示：

![ssh-keygen DEMO](/1_Basic/GitHub/Image/1_GitHubToLocal3.png "ssh-keygen DEMO")

2. 接下來再到剛剛金鑰儲存的位置，可以發現產生了兩個檔案，分別為公鑰 id_rsa.pub，及私鑰 id_rsa，我們藉由 Notepad++ 等軟體打開公鑰檔案或是使用 `cat /c/Users/zason/.ssh/id_rsa.pub` 指令將公鑰的內容複製起來，準備下一步！

![ssh key location](/1_Basic/GitHub/Image/1_GitHubToLocal4.png "ssh key location")

3. 到 GitHub → Setting 設定金鑰相關內容如下圖：

![setting ssh on GitHub](/1_Basic/GitHub/Image/1_GitHubToLocal5.png "setting ssh on GitHub")

4. 設置完成後，我們可以藉由指令 `ssh -T git@github.com` 來驗證 SSH KEY 是否設置正確。如果有出現用戶名稱，代表設置是正確的。

![test ssh](/1_Basic/GitHub/Image/1_GitHubToLocal6.png "test ssh")

5. 我們再次使用 SSH 方式來複製 GitHub 上的容器，透過指令 `git clone git@github.com:KuoWeiEli/GrowthNotes.git` 的鍵入，可看見這次不再出現開頭那段錯誤訊息了！可喜可賀，可喜可賀！

![SSH DEMO](/1_Basic/GitHub/Image/1_GitHubToLocal7.png "SSH DEMO")

備註：可到下載的該目錄下，執行命令列工具，鍵入 `git config --local -l` 指令可看見，紅框部分遠端與本地的對應關係已被設置完成！

![Remote config](/1_Basic/GitHub/Image/1_GitHubToLocal8.png "Remote config")






