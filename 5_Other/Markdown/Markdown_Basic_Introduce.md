Markdown
===========================
創始人 John Gruber 在 2004 年製作的 Markdown 是一個輕量級標記語言（LML：Lightweight Markup Language），能夠輕鬆使用純文字加上特定標點符號（markup）來書寫有條理結構的文章。

> 標記語言 （markup language）：在需要的地方添加標記（markup），以表示文字的結構與其他資訊的電腦文字編碼。標記最早用於出版業，現在工程師也很常用到標記語言，如下：
> * HTML（HyperText Markup Language）超文本標示語言
> * XML（eXtensible Markup Language）可延伸標示語言

## 標題
有六種標題大小可以選擇，皆使用 atx 形式，利用 `#` 符號加在文字前面，如果為了美觀效果，在文字的後面也可加上，如`#標題#`。
另外 H1、 H2（最高階、第二階）可以額外使用 Setext 型式，可在文字下一行加上任意個`=`或`-`來達成 H1、H2。
![1.png](/5_Other/Markdown/Image/1.png "1.png")


## 文字格式
名稱    | 格式                 | 顯示
:---:     | :---:                 | :---:
斜體     | `*字*或是 _字_`     | _字_
粗體    |`**字**或是 __字__` | **字**
刪除線 |`~~字~~`         | ~~字~~
下劃線 |`<u>字</u>`    | <u>字</u>

## 水平線
使用 `---`  、`***` 或者下標線 `___` 都可以 : 
***

## 引言
在註解或是引述一些文件（Email、文獻、別人的話...等等）皆適合使用這個來表達，其格式只要在最前面添加 `>` 符號即可 :
![2.png](/5_Other/Markdown/Image/2.png "2.png")

引言也可以使用階層方式呈現以及加入任何Markdown語法在裡面！
![3.png](/5_Other/Markdown/Image/3.png "3.png")

## 清單
清單可分為**有序**及**無序**，無序可以使用`*`、`+`、`-` 來標示。
![4.png](/5_Other/Markdown/Image/4.png "4.png")


## 待辦事項
![5.png](/5_Other/Markdown/Image/5.png "5.png")


## 超連結
![6.png](/5_Other/Markdown/Image/6.png "6.png")


## 圖片
![7.png](/5_Other/Markdown/Image/7.png "7.png")


## 表格
表格的組成至少會有兩列，欄與欄之間使用 `|` 分隔。第一列為 Header ，第二列為設定格式必須至少要有一個`-`，`-` 兩旁可以添加 `:` 來設定對齊方式。
![8.png](/5_Other/Markdown/Image/8.png "8.png")



## 程式區塊
程式可以使用行內或是區塊的方式來顯示，區塊方式可以支援幾十種程式語言高亮顯示。
![9.png](/5_Other/Markdown/Image/9.png "9.png")


## YouTube 影片
Markdown並沒有內嵌YouTube影片的功能，不過可以利用附上圖片再加上超連結的方式達到效果。
![10.png](/5_Other/Markdown/Image/10.png "10.png")



