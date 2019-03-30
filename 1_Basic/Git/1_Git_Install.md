# Git 安裝筆記
### 去哪安裝
如果要在 Windows 作業系統使用 Git，我們可以到 [Git For Windows](http://git-scm.com/download/win '進去後，下載即自動開始') 專案中下載，詳細可查閱 <http://git-for-windows.github.io/>。另外相關**版本控制程式碼代管服務**如 [GitHub](https://github.com/ "前去 GitHub")，也有提供客製化的 Git ─ [GitHub for Window](https://desktop.github.com/ "前去 GitHub for Window" ) 可供下載。

>  備註：耳熟能詳的 VCS 神器─[小烏龜](http://code.google.com/p/tortoisegit/ "前去小烏龜下載")，必須要先下載 [Git For Windows](http://git-scm.com/download/win '進去後，下載即自動開始') 歐！

兩者皆有提供命令列以及 GUI 圖形化工具。不過 GitHub For Window 的 GUI，筆者私心覺得比較美觀且易用。不過初學者還是從最基本的命令列工具來學習，一方面可以了解 Git 的指令，一方面也可以加深對於 Git 的了解。

### 安裝步驟
安裝步驟除了位置等相關基本資訊是自己想要的設定，其他皆是使用預設選項。

1. 合約條款。

![Setup 1](/1_Basic/Git/Image/1_GitSetup1.png "Setup 1")

2. 選擇安裝位置。

![Setup 2](/1_Basic/Git/Image/1_GitSetup2.png "Setup 2")

3. 選擇欲安裝的元件。
    * Additional Icons： 選擇是否要在桌面上建立捷徑。
    * 紅色框框所標註的元件，可在右鍵選單中選擇。`Git Bash Here` 在當前目錄執行命令列工具。`Git GUI Here` 在當前目錄執行圖形化工具。
    * Git LFS： 可以讓 Git 處理大型檔案（如圖片、影音檔等）。
    * Associate .git* configuration files with the default text editor：使用預設編輯器開啟 `.git` 為前綴（開頭）的檔案。
    * Associate .sh files to be run with Bash：以命令列工具執行副檔名為 `.sh` 的文件。
    * Use a TrueType font in all console windows：是否所有的命令列工具（包含命令式提示字元等）都使用 TrueType 字體。
    * Check daily for Git for Windows updates：是否需要每天檢查有無更新。

![Setup 3](/1_Basic/Git/Image/1_GitSetup3.png "Setup 3")

4. 設置 Git 在**開始選單**裡的位置。

![Setup 4](/1_Basic/Git/Image/1_GitSetup4.png "Setup 4")


5. 選擇預設的編輯器，這裡筆者使用熟悉的 Notepad++（之後可以再設定預設的編輯器）。

![Setup 5](/1_Basic/Git/Image/1_GitSetup5.png "Setup 5")

6. 這邊是設置 Git 相關套件至環境變數中。第三個選項須注意，加上 Unix 工具有可能會衝突到 Windows 系統。

![Setup 6](/1_Basic/Git/Image/1_GitSetup6.png "Setup 6")

7. 使用 OpenSSL Library 來設定 HTTPS 傳輸後端。

> OpenSSL 是一個被廣泛應用在網路伺服器上的函式庫，實作了基本加密功能、SSL、TLS 協定，能夠協助伺服器進行安全的傳輸。

![Setup 7](/1_Basic/Git/Image/1_GitSetup7.png "Setup 7")

8. 設定斷行字元的處理方式，預設是第一個選項。在將文字檔案提交到檔案庫的時候，Git 會將所有的 CRLF 轉換為 LF；反之從檔案庫取出文字檔案，將會把所有的 LF 轉換為 CRLF。

> 因為 Git 本身是為 Linux 設計的，Linux 這種類 UNIX 作業系統，斷行字元是 **LF** 字元（`\n`、ASCII 為 `0x0A` ）。在 Windows 作業系統中，斷行字元則是 **CRLF** 字元（`\r\n`、ASCII 為 `0x0D 0x0A`）。

![Setup 8](/1_Basic/Git/Image/1_GitSetup8.png "Setup 8")

9. 命令列工具要使用何種介面。第一個選項，MinTTY 是一種模擬 Unix 系統下的命令列工具，能夠紀錄命令，設定字體，調整視窗大小，但是只能執行 Git 命令。而第二個選項則是使用 Windows的命令提示字元。

![Setup 9](/1_Basic/Git/Image/1_GitSetup9.png "Setup 9")

10. 額外設定。
    * Enable file system caching：啟動文件緩存。此選項能加快載入最近使用的文件。
    * Enable Git Credential Manager：啟動憑證管理。這個選項能夠幫助管理憑證密碼。
    * Enable symbolic links：開啟符號連結。在 [Stack Overflow](https://stackoverflow.com/questions/5917249/git-symlinks-in-windows/16754068#16754068) 上有個相關的問題。不過這邊就暫時先不開啟。

![Setup 10](/1_Basic/Git/Image/1_GitSetup1.png "Setup 10")

11. 安裝完成畫面，恭喜囉！已經成功地把 Git 安裝在本機上了。

![Setup 11](/1_Basic/Git/Image/1_GitSetup11.png "Setup 11")







