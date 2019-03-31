# 將本機專案複製到 GitHub 上

前一篇─[GitHub 上的專案複製到本機上](1_GitHub_To_Local.md "再去前一篇回味一下！")講到如何將 GitHub 上的專案複製到本機上，那反過來呢？要怎麼將已存在自己電腦上的專案複製一份到 GitHub 上？因為前面已經將 SSH 傳輸設定搞好了，所以這篇難易度大大降低！現在就來瞧瞧吧！

### 步驟說明
1. 首先呢，在 GitHub 上找到 Repositories 的頁籤，會看到綠色 Button，請點擊它。

![LocalToGitHub1](/1_Basic/GitHub/Image/2_LocalToGitHub1.png "LocalToGitHub1")

2. 點擊後會出現 Create a ne repository 頁面設定檔案庫的相關資訊。可以看到**是否建立相關文件**那裏，GitHub 提供了三個文件給用戶選擇：
    * **README** 是個支援類 Markdown 語法的自述檔案。
    * **.gitignore** 為 Git 檔案庫的忽略檔案設定
    * **License（授權）** 是其他用戶在引用檔案庫時，必須遵照檔案庫所聲明的規範。PROGRESSBAR 的文章─[在開源時代的興起下，如何透過 License 共享並保有權益](https://progressbar.tw/posts/61 "點我去看 License 好文！")，這篇講的淺顯易懂，大家可以參考一下！

![LocalToGitHub2](/1_Basic/GitHub/Image/2_LocalToGitHub2.png "LocalToGitHub2")

3. 在剛剛建立好的 GitHub 檔案庫找到 Clone or download，點擊後選擇 HTTPS 或是 SSH 傳輸方式都可以。這邊使用 SSH 來作示範，並複製網址。

![LocalToGitHub3](/1_Basic/GitHub/Image/2_LocalToGitHub3.png "LocalToGitHub3")

4. 現在，我們需要建立 GitHub 檔案庫與本地檔案庫的對應關係（~姻緣~）。打開命令列工具（~進到教堂~），使用 `git remote add origin git@github.com:KuoWeiEli/DP_By_Pokemon.git`  指令（~發誓~）後，就完成（~結婚~）囉！事實上，本地檔案庫可以對應多個遠端檔案庫（~一夫多妻，一妻多夫？~）。可以使用 `git config --local -l` 來檢視剛剛是否有建立成功。可以看到最下面多了兩條參數 `remote.origin.url`、`remote.origin.fetch`。

> 如果您的專案尚未被 Git 接管（也就是沒有使用 Git 管理專案），那麼請先參考這篇─[讓 Git 接管你的專案](/1_Basic/Git/3_Git_Local_Repository.md "點擊去了解怎麼讓 Git 接管")，讓專案先成為 Git 本地檔案庫才行。

![LocalToGitHub4](/1_Basic/GitHub/Image/2_LocalToGitHub4.png "LocalToGitHub4")

5. 接著準備將本地檔案庫裡的檔案上傳到 GitHub，打上 `git push` 後出現了一個訊息。Git 告訴您，彼此之間的分支也要有對應關係！現在本地檔案庫的分支叫做 **master**（預設），而 GitHub 則是 **origin**（預設）。您可以使用 `git push --set-upstream origin master` 來設置分支的對應關係，其中，`--set-upstream` 可以更換短指令 `-u`。

![LocalToGitHub5](/1_Basic/GitHub/Image/2_LocalToGitHub5.png "LocalToGitHub5")

6. 於是您打上 `git push --set-upstream origin master` 後，按下去又出現了落落長的訊息。這是指 GitHub 現在的版本與您的本地端版本不同（通常是共同開發時，其他人提交新版本上去導致。這裡是因為我們在建立 GitHub 檔案庫時，有新增檔案 README or .gitignore or LICENSE），所以造成衝突了！

![LocalToGitHub6](/1_Basic/GitHub/Image/2_LocalToGitHub6.png "LocalToGitHub6")

7. 還記得要怎麼解決衝突嗎？忘記了也別擔心，這邊在一步一步教學，其實也就兩步...！
第一步：使用 `git fetch` 將 GitHub 的檔案庫的內容拉到本地檔案庫。
第二步：使用 `git rebase origin/master` 將兩者合併。如下圖：

![LocalToGitHub7](/1_Basic/GitHub/Image/2_LocalToGitHub7.png "LocalToGitHub7")

8. 解決衝突後，您可以放心的在打上一次 `git push -u origin master`，可以看到本地檔案庫的檔案成功上傳到 GitHub 剛剛建立的檔案庫：

![LocalToGitHub8](/1_Basic/GitHub/Image/2_LocalToGitHub8.png "LocalToGitHub8")

![LocalToGitHub9](/1_Basic/GitHub/Image/2_LocalToGitHub9.png "LocalToGitHub9")





