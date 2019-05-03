# How to download file from URL

常常會有這個需求：從某個伺服器下載檔案到本地端。如何利用 Java 來達成這個功能呢？來看看下列的程式碼吧！

## 範例解說
通常開放下載的檔案有個對應的 URL（用來對應伺服器端的某個位置），筆者這個範例就以下列網址：<https://www.sitca.org.tw/ROC/Download/K0000.aspx> 為例！這個連結提供各式表單供人下載，每個表單其實都對應著一個 URL。這邊就以**DP.基金應負擔費用率資訊、費用率及報酬率資訊**的**5.投信基金各級別近五年度費用率及報酬率資訊**電子表單為下載目標。

點擊下載圖示後，瀏覽器會自動呼叫這個圖式所設定的 URL，並下載檔案下來。可以對圖式右鍵→檢查，即可以看到它所呼叫的 URL 是什麼！所以可以看到程式的範例 `url` 變數的數值為 `"https://members.sitca.org.tw/OPF/K0000/files/P/05/投信基金各級別近五年度費用率及報酬率資訊.csv"`。

從 URL 下載檔案的概念就是，欲下載檔案的 URL 透過 java.net.URL 類別包裝後，藉由它的 `openConnection( )` 方法建立連線物件 HttpURLConnection 來連線（使用 `connect` 方法），之後再透過連線物件的 `getInputStream()` 方法取得輸入流（也就是欲下載的檔案流），寫入本地檔案的輸出流即完成下載！來看看以下程式碼如何實作！

```Java
public class UrlHelper {
    public static void main(String[] args) throws IOException {
        System.out.println("Start");
        String url = "https://members.sitca.org.tw/OPF/K0000/files/P/05/投信基金各級別近五年度費用率及報酬率資訊.csv";
        UrlHelper.downloadFileFromUrl(url, "D:\\test",null);
    }
    
    /** 從 Url 下載檔案 **/
    public static void downloadFileFromUrl(String url, String targetDir, String targetName) throws IOException {
        String fileName = getChildUrl(url);
        String encUrl = url.replace(fileName, "") + getEncodeUrl(fileName);

        /** 取得指定網址的連線物件並連線 **/
        URL fileUrl = new URL(encUrl);
        HttpURLConnection conn = (HttpURLConnection) fileUrl.openConnection();
        conn.setDoInput(true);
        conn.setDoOutput(true);
        conn.connect();

        /** 設定輸出檔案 **/
        Path target = Paths.get(targetDir).resolve(null == targetName? fileName: targetName);
        download(conn.getInputStream(), Files.newOutputStream(target));
        conn.disconnect();
        System.out.println("Finish！");
    }

    /** 下載 **/
    public static void download(InputStream inputStream, OutputStream outputStream) throws IOException {
        byte[] buf = new byte[1024];
        int length;
        try (BufferedOutputStream bos = new BufferedOutputStream(outputStream);
             BufferedInputStream bis = new BufferedInputStream(inputStream)) {
            while ((length = bis.read(buf)) != -1)
                bos.write(buf, 0, length);
        }
    }

    /** 取 Url 最後一段 **/
    public static String getChildUrl(String url) {
        return url.replaceAll(".+/", "");
    }

    /** percent-encode URL **/
    public static String getEncodeUrl(String url) throws UnsupportedEncodingException {
        return URLEncoder.encode(url, "UTF-8");
    }
}
```

## Trouble Shoot
### 版本不支援伺服器的安全協定

這個錯誤會出現下列訊息：主要是在講對象伺服器的安全協定為 TLSv1.2，而本地的 JDK 版本不支援。

`javax.net.ssl.SSLHandshakeException: Server chose TLSv1.2, but that protocol version is not enabled or not supported by the client.`

TLSv1.2 為傳輸層安全協定的一種，我們來看看[維基百科](https://zh.wikipedia.org/zh-tw/%E5%82%B3%E8%BC%B8%E5%B1%A4%E5%AE%89%E5%85%A8%E6%80%A7%E5%8D%94%E5%AE%9A "TLS")怎麼說：

>傳輸層安全性協定（英語：Transport Layer Security，縮寫作 TLS），及其前身安全通訊協定（Secure Sockets Layer，縮寫作 SSL）是一種安全協定，目的是為網際網路通訊提供安全及資料完整性保障。

要解決這個問題可以提升 JDK 版本到 1.8。

### 無法找到有效憑證
這個錯誤會出現下列訊息：

```
sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

主要是 Java 在憑證檔中找不到該伺服器的憑證序號。解決方法有兩種：
1. 匯入該網站憑證於 Java 憑證檔
2. 使用程式自動忽略所有憑證

#### 1、匯入該網站憑證於 Java 憑證檔

Java 的憑證檔路徑為：

```
../yourJDK/jre/lib/security/cacerts
../yourJRE/lib/security/cacerts

```

每個版本的 JDK 或是 JRE 其憑證檔均是獨立分開的。我們先看看您使用的 JDK 版本中其憑證檔是不是真的沒有該網站憑證。先談談網站憑證如何查看，瀏覽器呼叫該伺服器網址後，應該可以在網址列旁邊看到鎖頭圖式，每個瀏覽器顯示的方式不一樣，但都可以查看該憑證資訊與匯出憑證。先將憑證序號給記起來，之後會需要用到。

在剛剛憑證檔的路徑下使用命令列提示字元，打上下列指令來匯出憑證文字檔。`-keystore` 後面是憑證檔的檔名，而 `>` 後面則是輸出憑證文字檔的檔名。 它會提示需要**輸入金鑰儲存庫密碼**，預設密碼是 `changeit`。

```
keytool -list -v -keystore cacerts  > java_cacerts.txt
```

打開憑證文字檔，在檔案內搜尋一下是否有剛剛記起來的伺服器憑證序號。如果沒有序號，代表是這個錯誤的發生原因。我們可以下載伺服器的憑證到憑證檔目錄，並使用下列指令。`-alias` 後面的 `aliasName` 為別名名稱；`-keystore` 後面接著是憑證檔名稱 `cacerts`；`-file` 後面則是剛剛下載憑證檔的檔名

```
keytool -import -alias aliasName -keystore  cacerts -file certificateFile
```

所以此範例可能會這樣下指令：

```
keytool -import -alias wwwsitcaorgtw -keystore  cacerts -file wwwsitcaorgtw.crt
```

一樣會輸入憑證密碼以及確認選項（打上 `yes` or `是`），伺服器憑證檔的資訊就匯入憑證檔中了！如此一來這個錯誤應該可以解決！

參考網址：<http://magicmonster.com/kb/prg/java/ssl/pkix_path_building_failed.html>

#### 2、使用程式自動忽略所有憑證

忽略憑證邏輯如下：

```Java
public class SslUtils {
    /**
     * 忽略 HTTPS 請求的 SSL 憑證，需在開啟連線時調用此方法
     */
    public static void ignoreVerify() throws Exception {
        ignoreVerifyHttpsTrustManager();
        ignoreVerifyHttpsHostName();
    }

    /**
     * 忽略驗證 https
     */
    private static void ignoreVerifyHttpsHostName() {
        HostnameVerifier hv = new HostnameVerifier() {
            public boolean verify(String urlHostName, SSLSession session) {
                System.out.println("Warning: URL Host: " + urlHostName + " vs. " + session.getPeerHost());
                return true;
            }
        };
        HttpsURLConnection.setDefaultHostnameVerifier(hv);
    }

    /**
     * 忽略驗證 https
     */
    private static void ignoreVerifyHttpsTrustManager() throws Exception {
        TrustManager[] trustAllCerts = new TrustManager[]{
                new X509TrustManager() {
                    public java.security.cert.X509Certificate[] getAcceptedIssuers() {
                        return null;
                    }

                    public void checkClientTrusted(java.security.cert.X509Certificate[] certs, String authType) {
                    }

                    public void checkServerTrusted(java.security.cert.X509Certificate[] certs, String authType) {
                    }
                }
        };

        SSLContext sc = SSLContext.getInstance("SSL");
        sc.init(null, trustAllCerts, new java.security.SecureRandom());
        HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
    }
}
```

`SslUtils.ignoreVerify()` 方法需要在開啟連線前呼叫，**且只要呼叫一次就行**。

```Java
	SslUtils.ignoreVerify();
	HttpURLConnection conn = (HttpURLConnection) fileUrl.openConnection();
	/** 連線 **/
	conn.connect();
```
### 400 回應

會出現下列錯誤訊息：

```
java.io.IOException: Server returned HTTP response code: 400 for URL: https://members.sitca.org.tw/OPF/K0000/files/P/05/投信基金各級別近五年度費用率及報酬率資訊.csv
```

因為在發送 URL 請求的時候，會 percent encode 該 URL。可參考[維基百科](https://zh.wikipedia.org/zh-tw/%E7%99%BE%E5%88%86%E5%8F%B7%E7%BC%96%E7%A0%81 "百分號編碼")對此的解釋。所以必須用 java.net.URLEncoder 來編碼請求的 URL 後再送出，可以參考上述範例的做法。
