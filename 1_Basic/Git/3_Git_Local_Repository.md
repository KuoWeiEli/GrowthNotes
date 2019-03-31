# 讓 Git 接管你的專案

經過前面兩篇 [Git 安裝筆記](1_Git_Install.md "還沒安裝嗎？快進去瞧瞧！")、[Git Config 基本設定](2_Git_Config_Basic.md "還沒完成基本設定嗎？快進去瞧瞧！")後，我們終於可以建立第一個 Git 專案囉！跟著筆者動一動，手指動一動！

### 建立 Git 檔案庫
首先我們找到需要被 Git 管理的資料夾，不論是新資料夾或是已有資料的舊資料夾，通通都可以使用。接著在該資料下開啟命令列工具（右鍵→Git Bash Here），使用 `git init` 指令會出現下圖訊息。

![init demo](/1_Basic/Git/Image/3_Git_Local_Repository1.png "init demo")

訊息說明 Git 已在本目錄建立了一個**空的**檔案庫。為什麼是空的呢？這可以用下圖來解釋！藍色框框就是剛剛 Git 幫你建立的檔案庫，目前是空的。最右邊綠色區塊為工作目錄，可以看到該資料夾下的檔案，我們新增、編輯、刪除檔案都是在工作目錄下完成。

![Git Basic1](/1_Basic/Git/Image/Git_Basic1.png "Git Basic1")


等等，您會說那中間的黃色的索引區塊是幹嘛？好問題！索引區塊代表的是，準備提交到檔案庫的檔案。我們必須先透過 Git 提供的指令 `add`，將工作目錄下的檔案加入到索引後。這些在索引區塊的檔案才可以再次用指令 `commit` 提交到檔案庫。如下圖：

![Git Basic2](/1_Basic/Git/Image/Git_Basic2.png "Git Basic2")

### 檔案狀態
簡單了解 Git 怎麼運作後，再來要解說的是**檔案狀態**。我們打上指令：`git status` 可以看到目前分支下的狀況。

![status before ignore setting demo](/1_Basic/Git/Image/3_Git_Local_Repository2.png "status before ignore setting demo")

檔案狀態分為三種：
1. tracked（被追蹤的）：已加入檔案庫的檔案將被標記此狀態。
2. ignored（被忽略的）：符合 .gitignore 規則的檔案將被忽略。
3. untracked（不被追蹤的）：還沒有受到處理的檔案。

因為我們還沒有開始處理任何檔案，所以可以看到上圖中，目錄下的所有檔案全部都被標註成 **untracked**。

正常情況下，我們不會將所有的檔案都上傳至 GitHub。像是圖中 `.idea/`、附檔名`.iml`，這些是由 IDE 所產生的檔案。`out/` 是存放編譯 java 後產生的 class 檔案。這些可以透過設置 .gitignore 檔來請求 Git 忽略這些檔案。

### .gitignore 設置
我們可以打上指令 `touch .gitignore` 建立檔案，接著使用 Notepad++ 編輯如下：

![ignore setting demo](/1_Basic/Git/Image/3_Git_Local_Repository3.png "ignore setting demo")

備註：
* 通常不會忽略 .gitignore 檔案，因為其他人使用的時候，也需要這些忽略規則。
* 每一層目錄都可以建立 .gitignore 檔。影響範圍為該目錄與其子目錄，並且最上層的 .gitignore 檔案有最大的優先權。

設置完成後，我們再次打上 `git status`，可以看見符合該忽略規則的檔案，都不再顯示於命令列工具上。

![status after ignore setting demo](/1_Basic/Git/Image/3_Git_Local_Repository4.png "status after ignore setting demo")

### 添加到索引
接下來準備將工作目錄的檔案添加到索引囉！我們可以使用 `add .` 將所有該目錄下**新增/修改**的檔案加入到索引。打完指令再次使用 `git status` 查看：

![status after add demo](/1_Basic/Git/Image/3_Git_Local_Repository5.png "status after add demo")

`add` 指令後面不只可以添加 `.`，還可以添加別的參數，茲舉例幾個參數如下：

參數 | 功能
:-: | :-:
. | 將**新增/修改**的檔案加入到索引
-u | 將**修改/刪除**的檔案加入到索引
-A | 將**新增/修改/刪除**的檔案加入到索引
-f | 無視忽略規則並加入到索引

那我們可以將已加入索引的檔案移除嗎？只要使用指令 `git rm --cached 檔案名稱` 即可辦到。下圖展示了使用 `git add -f` 忽視 .gitignore 的規則將檔案加入到索引，再利用 `git rm --cached 檔案名稱` 將檔案從索引中移除。

![add -f and rm --cached demo](/1_Basic/Git/Image/3_Git_Local_Repository8.png "add -f and rm --cached demo")

### 提交到檔案庫
添加完檔案到索引後，就可以執行 `git commit`  指令將索引區的檔案提交到檔案庫。後面可以接上參數 `--author 'yourName'` 、`--message [or -m] 'comments'`。如果沒有打上 `--message` 參數，會自動開啟 Notepad++如下圖所示，並且附有註解的規範寫法。

![commit demo](/1_Basic/Git/Image/3_Git_Local_Repository6.png "commit demo")

打完註解後關掉 Notepad++，命令列工具就會執行提交到檔案庫的動作。

![commit demo1](/1_Basic/Git/Image/3_Git_Local_Repository7.png "commit demo1")

