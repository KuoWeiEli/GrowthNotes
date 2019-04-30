# Discuss the copy method and Zero Copy

檔案複製是個常見的需求，最早在 Java 7 以前使用 java.io 類的串流來完成複製需求。到了 Java 7 後開始有了 java.nio 類，除了 java.nio.file.Files 直接提供 `copy` 方法複製檔案外，也有一個很潮的名詞出現：**Zero Copy（零複製）**。

### Zero Copy
什麼是 Zero Copy？先來看看[維基百科](https://zh.wikipedia.org/zh-tw/%E9%9B%B6%E5%A4%8D%E5%88%B6)怎麼說：

> 零複製（英語：Zero-copy，也譯零拷貝）技術是指電腦執行操作時，CPU 不需要先將資料從某處記憶體複製到另一個特定區域。這種技術通常用於通過網路傳輸檔案時節省 CPU 周期和記憶體頻寬。

用下圖來嘗試說明  java.io 可能的複製流程，當您用 Eclipse 開發寫了 java.io 串流方法實現檔案複製時，其步驟如下：
1. 透過 DMA 從 hardware 層讀取檔案資料到系統層的 Buffer。
2. 從系統層的 Buffer 寫到使用者層的 Buffer，像是 FileInputStream.`read` 方法。
3. 從使用者層的 Buffer 寫到系統層的 Buffer，像是 FileOutputStream.`write` 方法。
4. 透過 DMA 從系統層的 Buffer 將檔案資料寫入到 hardware 層 。

> 名詞解說：
> 
> DMA（**D**irect **M**emory **A**ccess）：直接記憶體存取。
> 
> NIC（**N**etwork **I**nterface **C**ontroller）：網路介面控制。

![零複製解說1](/3_BackEnd/Java/Image/discussCopy0.png "零複製解說1")

介於 User Context 與 System Context 之間，造成了兩次 CPU 執行讀取/寫入，並耗費了相關記憶體空間。

Zero Copy 免除了 CPU 運算與記憶體空間的浪費，直接在 System Context 層中處理資料並寫到 Hardware 層。如下圖所示，有下列步驟：
1. 使用 FileChannel 的 `transferTo`、`transferFrom` 方法。
2. 透過 DMA 從 hardware 層讀取檔案資料的位置和長度到系統層的 Buffer。
3. 剛剛紀錄檔案資料的資訊會寫到 Descriptor。
4. 透過 DMA 與 Descriptor 將 hardware 層的資料寫到 hardware 的另個位置 。

![零複製解說2](/3_BackEnd/Java/Image/discussCopy1.png "零複製解說2")

### 程式碼與測試紀錄
現在就來看看各個複製的方法與所花費的時間記錄！


```Java
/**
 * 檔案複製
 **/
public class CopyFile {
    public static void main(String[] args) throws IOException {
        System.out.println("檔案複製歐~");

        long start = System.currentTimeMillis();
        /** ============================== 測試的方法 ============================== **/
//        useFileInputStream("D:/test/from/source", "D:/test/to/result");
//        useFilesCopy("D:/test/from/source", "D:/test/to/result");
        zeroCopy("D:/test/from/source", "D:/test/to/result");
        /** ====================================================================== **/
        long end = System.currentTimeMillis();

        System.out.printf("總共費時：%sms", end-start);
    }

    /** 使用 IO 類的 FileInputStream  **/
    public static void useFileInputStream(String source, String target) throws IOException {
        byte[] buffer = new byte[1024];
        int length;

        try (FileInputStream fileIS = new FileInputStream(new File(source));
             FileOutputStream fileOS = new FileOutputStream(new File(target))) {
            while ((length = fileIS.read(buffer)) > 0)
                fileOS.write(buffer, 0, length);
        }
    }

    /** 使用 Java 7 的 Files 類 **/
    public static void useFilesCopy(String source, String target) throws IOException {
        /** 如果目的地有檔案的話，會出現下列錯誤訊息：java.nio.file.FileAlreadyExistsException: D:\test\to\result **/
        Files.copy(Paths.get(source), Paths.get(target));
    }

    /** 使用零複製 transferFrom 方法 **/
    public static void zeroCopy(String source, String target) throws IOException {
        /** mode 需使用 "rw" 才可以讀寫檔案，否則會出現下列錯誤訊息：java.nio.channels.NonWritableChannelException **/
        try(FileChannel srcChannel = new RandomAccessFile(source, "rw").getChannel();
            FileChannel tarChannel = new RandomAccessFile(target, "rw").getChannel()) {
            tarChannel.transferFrom(srcChannel, 0, srcChannel.size());
//            srcChannel.transferTo(0, srcChannel.size(), tarChannel);
        }
    }
}
```

* * *
* 測試資料大小為 13,134,738 位元組（約莫 12.5 MB）

method | spend time
:-: | :-:
useFileInputStream | 371ms
useFilesCopy | 43ms
zeroCopy | 41ms

* * *
* 測試資料大小為 183,886,293 位元組時（約莫 175 MB）

method | spend time
:-: | :-:
useFileInputStream | 5832ms
useFilesCopy | 470ms
zeroCopy | 435ms

### 結論
可以看見傳統的串流複製，因為擁有 User Context 與 System Context 這兩層的 CPU 運算，所以花費的時間是其他方法的好幾倍。Files 的 `copy` 方法，幾乎跟支援零複製 FileChannel 的 `transferTo` 、`transferFrom` 方法一樣快。

