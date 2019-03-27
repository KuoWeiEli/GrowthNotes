# 'Join' error on angular datepicker
在開發的時候，會用到 angular 實作的 bootstrap 套件。 其中使用 `uib-datepicker` 日曆的相關套件時，Chrome 瀏覽器主控台就會出現 `TypeError: Cannot read property 'join' of undefined`  的相關錯誤，導致頁面顯示不出來。

GitHub 也有人發起了這個 Issue：[GitHub Issue](https://github.com/angular-ui/bootstrap/issues/5439)。其中裡面有人回覆：

> In my case, i was using angular-locale en-us, same name module , I uninstalled it and downloaded the angular-i18n and everything worked

不過筆者試了好幾次較新的版本，依舊存在這個錯誤。

筆者還重新安裝 Chrome 瀏覽器，想說**重新安裝**這個大絕招可以奏效，但還是失敗了。而且奇怪的是，在 Edge 瀏覽器就不會出現這個錯誤？

目前筆者找到了一個解決辦法，因為筆者有登入 Chrome 瀏覽器，所以開啟了**訪客模式**就不會出現這個錯誤了（備註：開啟無痕式仍會出現這個錯誤！）。

點擊登入者的頭像，將會出現如下圖的下拉式選單，就可以選擇**開啟訪客視窗**了！
![open_visitor_mode](/2_FrontEnd/Angular/Image/open_visitor_mode.png "開啟訪客模式說明圖")

> 你在這個視窗瀏覽的網頁不會顯示在瀏覽器記錄中，而且在你關閉所有開啟的訪客視窗後，電腦也不會留下 Cookie 等其他記錄。不過，系統將會保留你下載的所有檔案。

從訪客視窗上的提醒文字推測，可能因為瀏覽器的設定值，又或是 Cookie 設定導致了這個錯誤 `TypeError: Cannot read property 'join' of undefined` ！






