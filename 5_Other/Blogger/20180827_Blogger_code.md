# Blogger 筆記（3）：Blogger 中的程式碼

要如何在 Blogger 中加入程式碼，其實這是筆者寫 Blogger 的最重要的需求。客官們一定不想要直接面對平白樸素的 code 吧！（真的能夠面對，應該是神人等級了😂） 所以為了這個最重要的需求，也足足花了我很多時間來尋找答案。

目前可在 GitHub 找到最新的外掛：[Google Code Prettify](github.com/google/code-prettify "Google Code Prettify")。進去後有詳細的教學，要如何使用語法等等。在這裡簡單介紹一下！ 

> **Warning！**
>
> * 接下來的教學也許過了一段時間後，就不再有其效果! 所以最好追蹤 [Google Code Prettify](github.com/google/code-prettify "Google Code Prettify") 在GitHub的近況。
>
> * 有些主題無法使用此功能，如官方的**動態檢視**主題！

## 代碼說明
#### 核心JS
```HTML
<script src="https://google-code-prettify.googlecode.com/svn/loader/run_prettify.js"></script>
```

上面就是 **Google Code Prettify** 的核心代碼，將會搜尋 HTML 裡的
`<pre>`、 `<code>`、`<xmp>`元素，並呈現 **Code Block** 的樣子，其中可引入一些參數，添加效果：

參數      |   默認值     |   說明
:-         |       :-:     |       :-
autorun=(true, false)   |   true    |   在頁面加載時自動運行。
lang=...    |  none |   加載指定語言，其參數名為**文件擴展名**，請參照支援的語言。可以指定多個語言（如：?lang=css&lang=ml）。不過預設將會自動判別語言，不須特別設定。
skin=...    |   none    |   設定Code Block的CSS樣式，請參照官方的 [Skin Gallery](cdn.rawgit.com/google/code-prettify/master/styles/index.html "Skin Gallery")。
callback=js_ident   |   |   `window.exports["js_ident"]` 將在精美打印完成時調用如果多次指定，則全部被調用。

#### CSS 說明與使用
除了可以在引入核心中設置參數 `skin` 來取得 **Code Block** 的 CSS。也可以另外加入額外的 CSS 參數。官方的 CSS 參數 Class 最主要有兩個，分別為 `prettyprint`、`linenums`。其參數說明如下：

Class   |   效果
:-       |  :-
prettyprint | 標註此 Class 的元素，其內容樣式將會變成 Code Block Style。
linenums:num    |   是否標註行號，`[:num]` 指定從第幾行開始顯示（可選）。

筆者引入的 CSS 樣式如下 : 

```HTML
<!--Google Code Prettify-->
<style>
    /*! Color themes for Google Code Prettify | MIT License | github.com/jmblog/color-themes-for-google-code-prettify */
    .prettyprint {
        background: #2d2d2d;
        font-family: Menlo, "Bitstream Vera Sans Mono", "DejaVu Sans Mono", Monaco, Consolas, monospace;
        border: 0 !important;
        overflow: auto;
        min-height: 30px;
        max-height:600px;
    }
    .pln {
        color: #cccccc;
    }
    /* Specify class=linenums on a pre to get line numbering */
    ol.linenums {
        margin-top: 0;
        margin-bottom: 0;
        color: #999999;
    }
    li.L0,
    li.L1,
    li.L2,
    li.L3,
    li.L4,
    li.L5,
    li.L6,
    li.L7,
    li.L8,
    li.L9 {
        padding-left: 1em;
        background-color: #2d2d2d;
        list-style-type: decimal;
    }
    @media screen {
        /* string content */
        .str {
            color: #99cc99;
        }
        /* keyword */
        .kwd {
            color: #cc99cc;
        }
        /* comment */
        .com {
            color: #999999;
        }
        /* type name */
        .typ {
            color: #6699cc;
        }
        /* literal value */
        .lit {
            color: #f99157;
        }
        /* punctuation */
        .pun {
            color: #cccccc;
        }
        /* lisp open bracket */
        .opn {
            color: #cccccc;
        }
        /* lisp close bracket */
        .clo {
            color: #cccccc;
        }
        /* markup tag name */
        .tag {
            color: #f2777a;
        }
        /* markup attribute name */
        .atn {
            color: #f99157;
        }
        /* markup attribute value */
        .atv {
            color: #66cccc;
        }
        /* declaration */
        .dec {
            color: #f99157;
        }
        /* variable name */
        .var {
            color: #f2777a;
        }
        /* function name */
        .fun {
            color: #6699cc;
        }
    }
</style>
```

## 外掛設置步驟
#### 1、到 Blogger 管理後臺，選擇版面配置→新增小工具
![3_blogger_code1.png](/5_Other/Blogger/Image/3_blogger_code1.png "3_blogger_code1.png")

#### 2、點選 HTML/JavaScript
![3_blogger_code2.png](/5_Other/Blogger/Image/3_blogger_code2.png "3_blogger_code2.png")

#### 3、貼上剛剛的核心 JS 與 CSS 代碼後儲存
> Hint！
>
> 標題可以不打（這樣的話就不會出現在 Blogger 頁面上）！
![3_blogger_code3.png](/5_Other/Blogger/Image/3_blogger_code3.png "3_blogger_code3.png")

## 實戰練習
#### 1、準備代碼
我們就 HTML sample code 來示範要如何將代碼變成剛剛設定的**Code Block**樣式。代碼如下：

```HTML
<!DOCTYPE html>
<html>
	<head>
		<title>示範 Code Block By Eli</title>
	</head>
	<body>
		<h1>Coding My Life</h1>
		<p>人生就如一段程式碼...</p>
	</body>
</html>
```

#### 2、轉換代碼
此時代碼還沒有辦法直接使用，我們必須先將代碼轉變格式（如`<`、`>`轉換為`&lt;`、`&gt;`，可到 [HTML TO XML PARSER](demo.smarttutorials.net/html-xml-adsense-parser "HTML TO XML PARSER") 進行轉換）。
![3_blogger_code4.png](/5_Other/Blogger/Image/3_blogger_code4.png "3_blogger_code4.png")

#### 3、置入代碼
將轉換過的代碼，與下列代碼中的其中一段**將程式碼貼入這裡**做置換。並在 **HTML模式**下（注意，不是在**撰寫模式**），貼到想要的位置。
```HTML
<pre class="prettyprint linenums">
    <code>將程式碼貼入這裡!</code>
</pre>
```

#### 4、檢視結果
接下來完成編輯後，在檢視文章時，你就會看到 Code Block 現身拉！😎 
![3_blogger_code5.png](/5_Other/Blogger/Image/3_blogger_code5.png "3_blogger_code5.png")




