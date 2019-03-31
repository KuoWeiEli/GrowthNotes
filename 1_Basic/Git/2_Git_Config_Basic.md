# Git Config 基本設定

在安裝完 Git 後，還有一項重要的任務必須達成，那就是使用 Git 提供的 Config 工具來完成 Git 相關的設定。這些設定都會儲存在檔案裡，而且也有階級之分呢！每個階級，身處在不同的資料夾，擁有不同優先權！我們趕快來瞧瞧吧！

在隨便一個地方點選右鍵，應該可以看到如下的圖示：

![右鍵選單](/1_Basic/Git/Image/2_GitConfig1.png "快選功能列")

點選 **Git Bash Here**，就會開啟命令列工具。

![命令列工具](/1_Basic/Git/Image/2_GitConfig2.png "Command line tool")

呼叫 Git Config 工具很簡單，打上 `git config` 即可，它會列出相關資訊。其中可以看見這一段文字：

![Git 環境](/1_Basic/Git/Image/2_GitConfig3.png "Git 環境")

這就是一開始提到的，設定也有階級之分。說文解字如下：
* `--global`：儲存在使用者目錄中的 gitconfig 檔。
* `--system`：儲存在 Git 程式安裝目錄下的 etc/gitconfig 檔。
* `--local`：儲存在專案下的 .git/config 檔。

其優先權的意思是說，Git 執行相關的動作時，會先參考哪個設定。優先權的順序為 `local`>`global`>`system`。

### 使用者資訊

Git 提交時，會附上相關的使用者資訊，在 commit 動作完成不能更改此資訊。我們可以使用下列指令來設定該資訊：
* 設定使用者名稱：`git config --global user.name 'yourName'`
* 設定使用者 Email：`git config --global user.email 'yourEmail'` 

設定完成後可以使用 `git config --global -l` 或者 `git config --global --list` 來查看剛剛的參數是否設定正確。其中 `-l` 為 `--list` 的短指令。或者可以直接查詢某個數值是否正確，比如查詢使用者名稱：`git config --global user.name`。如果不小心打錯，只要重新輸入一次指令，即可覆蓋舊的資料。當然也可以使用 `git config --unset user.name` 直接移除該參數。

因為使用者資訊通常不會更改，所以可以看見指令中有 `--global` 指令，使每個專案都參考這個設定。如果您要在不同專案使用不同的使用者資訊，那就只要在該專案下，設定使用者資訊時，將 `--global` 更換成 `--local` 即可。

### 使用別名

如果覺得指令太難記或者要打很多字，也可以使用自己的命名來取代。比如 `git config --global -l`  要改成 `git al` 只要這樣設定即可：`git config --global alias.al 'config --global -l'`。

### 預設編輯器

在上一篇 [Git 安裝筆記](1_Git_Install.md "再看一次 Git 安裝筆記？")中有提到，其中第五個步驟是選擇預設的編輯器，筆者那時候選擇的是 Notepad++。使用 `git config --system core.editor` 指令會得到 notepad++.exe 所在的路徑（Notepad 的安裝路徑下可找到 notepad++.exe）。如果想要更改預設的編輯器，可以打上如下指令：`git config --system core.editor "'編輯器執行檔的路徑'"`。或者想要反璞歸真使用 Windows 的記事本，打上下面指令即可：`git config --system core.editor notepad`。

### 檔案比對程式

檔案比對是個很常用到的功能，我們可以借助其他的軟體，來幫助我們比對程式。這裡使用 KDiff3 來當作我們的檔案比對程式（可以在這裡下載 [KDiff3](https://sourceforge.net/projects/kdiff3/files/latest/download "點擊自動下載 KDiff3")）。相關設定步驟如下：
* 自訂檔案比對參數：`git config --global diff.tool kdiff3`。
* 設定比對程式路徑：`git config --global difftool.kdiff3.cmd "'kdiff3.exe 的路徑' \"\$LOCAL\" \"\$REMOTE\""`。
* 啟動比對程式時，是否需要詢問：`git config --global difftool.prompt false`。

這樣子就設定完成了，以後如果要比對程式，只要用以下指令即可：`git difftool HEAD^ HEAD fileName`（HEAD 為檔案庫中最新版本，HEAD^為前一版）。

### 檔案合併工具
在多人開發的環境裡，檔案衝突是難免的，這時候我們就要再次抱 KDiff3 的大腿了！相關設定步驟如下：
* 自訂檔案合併參數：`git config --global merge.tool kdiff3`。
* 設定合併程式路徑：`git config --global mergetool.kdiff3.cmd "'kdiff3.exe 的路徑' \"\$BASE\" \"\$LOCAL\" \"\$REMOTE\" -o \"\$MERGED\""`。
* 啟動合併程式時，是否需要詢問：`git config --global mergetool.promp false`。
* 是否用傳回值來判斷合併是否成功：`git config --global mergetool.kdiff3.trustExitCode true`。
* 是否要備份衝突檔案：`git config --global mergetool.keepBakup false`。

### 說明文件
Git 設定百百種，詳細的資料可以參考說明文件。以下指令可以叫出文件：`git help config`、`git config --help`。





