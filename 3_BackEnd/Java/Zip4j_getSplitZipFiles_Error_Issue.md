# Zip4j 套件其方法 `getSplitZipFiles()` 取得分割檔名列表與實際產出檔名不一致問題

取自 [Zip4j](http://www.lingala.net/zip4j/download.php "Zip4j 官網") 官網：
> 關於 Zip4jJava 具有內置的 Zip 支持，但缺少多個 Zip 功能（例如：支持加密的 Zip 文件等）。我的想法是在 Java 的 inbuit Zip 功能之上構建一個開源 java 庫，它可以支持大多數 Zip 格式規範。Zip4j 是一個簡單易用的開源 java 庫，支持創建，提取，更新 Zip 文件。它支持 Zip 文件，Zip64 格式等的標準 Zip 加密和 AES 加密。有關詳細信息，請參閱 Zip4j 主頁上的主要功能部分。

筆者在很久之前有壓縮加解密的需求，所以有使用這個套件。目前最新的版本為 
`1.3.2`。最近筆者在實作壓縮檔分割寄送的需求時，一直碰到這個錯誤：

```
java.nio.file.NoSuchFileException: C:\XXX\XXX\XXXX\ZipFile.z010
```

明明目的地有產出分割檔，但一直找不到此檔案（眼睛業障重...）！後來仔細瞧瞧發現了一些蛛絲馬跡。產出分割檔的檔名叫做 `ZipFile.z10`，但是使用 ZipFile 這個類別的 `getSplitZipFiles( )` 來取得分割檔的檔名列表時，唯獨第十個分割檔檔名為 `ZipFile.z010`，其他分割檔名沒有這個問題。為什麼會這樣子？擷取這個 `getSplitZipFiles( )` 的其中一段來瞧瞧如下：

```Java
for (int i = 0; i <= numberOfThisDisk; i++) {
	if (i == numberOfThisDisk) {
		retList.add(zipModel.getZipFile());
	} else {
		String fileExt = ".z0";
		if (i > 9) {            // 這裡出現了問題
			fileExt = ".z";
		}
		partFile = (zipFileName.indexOf(".") >= 0) ? currZipFile.substring(0, currZipFile.lastIndexOf(".")) : currZipFile;
		partFile = partFile + fileExt + (i + 1); // 當 i=9 時，根據程式會產出附檔名為 .z010
		retList.add(partFile);
	}
}
```

可以發現到這是 Zip4j 的 bug，目前官網並未修復此 bug。如果要解決此問題：可以直接修改此 Bug，客製你的 Zip4j。不然就加上一些判斷，讓分割檔名為 `.z010` 時，修改成 `.z10`：

```Java
for (Object p : zipFile.getSplitZipFiles()) {
	// 當副檔名為 .z010 時則修改成 .z10
	if (p.toString().matches(".+\\.z010$")) {
		System.out.println("原分割檔名 => " + p);
		System.out.println("改造 => " + p.toString().replaceAll(".z010$", ".z10"));
	} else {
		System.out.println(p);
	}
}
```
輸出結果：

```
C:\XXX\XXX\XXXX\ZipFile.z01
C:\XXX\XXX\XXXX\ZipFile.z02
C:\XXX\XXX\XXXX\ZipFile.z03
C:\XXX\XXX\XXXX\ZipFile.z04
C:\XXX\XXX\XXXX\ZipFile.z05
C:\XXX\XXX\XXXX\ZipFile.z06
C:\XXX\XXX\XXXX\ZipFile.z07
C:\XXX\XXX\XXXX\ZipFile.z08
C:\XXX\XXX\XXXX\ZipFile.z09
原分割檔名 => C:\XXX\XXX\XXXX\ZipFile.z010
改造 => C:\XXX\XXX\XXXX\ZipFile.z10
C:\XXX\XXX\XXXX\ZipFile.z11
C:\XXX\XXX\XXXX\ZipFile.z12
C:\XXX\XXX\XXXX\ZipFile.z13
C:\XXX\XXX\XXXX\ZipFile.z14
C:\XXX\XXX\XXXX\ZipFile.z15
C:\XXX\XXX\XXXX\ZipFile.z16
C:\XXX\XXX\XXXX\ZipFile.z17
C:\XXX\XXX\XXXX\ZipFile.z18
C:\XXX\XXX\XXXX\ZipFile.z19
C:\XXX\XXX\XXXX\ZipFile.z20
```
