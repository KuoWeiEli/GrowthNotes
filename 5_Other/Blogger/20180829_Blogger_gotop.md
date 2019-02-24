# Blogger 筆記（5）：如何新增回到頂端的按鈕
相信不少人在瀏覽網頁的時候，常常會看到右下角有個按鈕。這個按鈕按下去，頁面將會回到網頁最上方。在瀏覽很長的文章的時候特別實用，你就不用費時費力的使用滑鼠滾輪一直往上滾。你心想： 天啊！如果自己的 Blogger 能夠有這種按鈕就好了！辦得到嗎？答案是：可以！

這種增加使用者體驗的設計，其實在現代的網頁上，有越來越多的趨勢，而且增加到你的 Blogger 很簡單的，讓我們開始吧！

## 教學說明
#### 1、選擇圖示
大部分的都會選擇有上升意涵的圖示，你可以到 [ICONFIDER](www.iconfinder.com "ICONFIDER") 尋找你想要的圖示。而這個範例將使用火箭圖示做為按鈕。

#### 2、上傳圖片
在編輯文章（HTML 模式）下，上傳剛剛下載好的火箭圖示。上傳過後應該可以見到如下網址 :
```HTML
<a href="https://3.bp.blogspot.com/-CkjA4yzVja0/W4tyK5jdaxI/AAAAAAAACP8/GpI7gn1BkeEpwyWbOOJ5S0FEEu2hZJOmgCLcBGAs/s1600/if_back_on_top_328014.png" imageanchor="1" ><img border="0" src="https://3.bp.blogspot.com/-CkjA4yzVja0/W4tyK5jdaxI/AAAAAAAACP8/GpI7gn1BkeEpwyWbOOJ5S0FEEu2hZJOmgCLcBGAs/s1600/if_back_on_top_328014.png" data-original-width="512" data-original-height="512" /></a>
```
可看到 `img` 標籤內的 `src` 有一段網址，`https://3.bp.blogspot.com/-CkjA4yzVja0/W4tyK5jdaxI/AAAAAAAACP8/GpI7gn1BkeEpwyWbOOJ5S0FEEu2hZJOmgCLcBGAs/s1600/if_back_on_top_328014.png` ，那就是火箭圖示的網址。將網址複製起來，待會需要用到！

## 插入程式碼
到 `Blogger 後台管理` → `主題` → `編輯 HTML` ，準備插入接下來的程式碼 !
> **Warning！**
>
> 在插入程式碼之前，記得備份你的主題！  

#### 1、插入顯示圖式的程式碼
;尋找`<body>`標籤，並在其上方新增這一段 code：
```HTML
<div id="jumper">
    <img width="80"
         src='https://2.bp.blogspot.com/-JamkMHnf2ps/W4tmQEZCw8I/AAAAAAAACPw/HlvgmetNuzI8Aprr2IPjpu1-_a6jaAN_ACLcBGAs/s1600/rocket.png'/> 
   <!-- 上面img標籤內的src 替換成剛剛的火箭圖示網址 -->
</div>
<style>
    #jumper {
        display: none;
        position: fixed;
        right: 40px;
        bottom: 80px;
        cursor: pointer;
    }
</style>
```

#### 2、插入按鈕動作</div>
尋找`</head>`標籤並在其上方新增這一段 code：
```HTML
<!--Start:插入TOP套件-->
<script type='text/javascript'>
    $(function () {
        $("#jumper").click(function () {
            jQuery("html,body").animate({
                scrollTop: 0
            }, 1000);
        });
        $(window).scroll(function () {
            if ($(this).scrollTop() > 450) {
                $('#jumper').fadeIn('fast');
            } else {
                $('#jumper').stop().fadeOut('fast');
            }
        });
    });
</script>
<!--END:插入TOP套件-->
```

#### 3、貼完上述程式碼後，儲存設定，就可以檢視你剛剛的成果囉！

