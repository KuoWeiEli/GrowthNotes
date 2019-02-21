# Markdown on Evernote
目前國際板的 Evenote 並沒有支援 Markdown 語法，如果執意要用的話，有很多熱心網友有發布一些教學方法，教你如何搭配一些軟體、插件達到成果！

不過日前聽到一則令人振奮的消息，那就是**可以在 Evernote 上直接使用 Markdown 語法**！這著實讓懶散的筆者彷彿看到救贖的聖光，立馬來瞧瞧怎麼使用這個玩意。了解之後，原來是中國版的 Evernote 有這項功能可以使用。

> Evernote 在中國建立本土公司名為印象筆記，在2018年12月率先推出 Markdown 語法給使用者使用。
> 目前（2019／02／20）在印象筆記中下載的版本號為 6.17.9，[連結附上](https://www.yinxiang.com/download/?offer=www_menu "印象筆記")。
> 國際板的 Evernote 版本號為 6.17.6 [連結附上](https://evernote.com/download "國際版 Evernote")。

國際版的版號略舊，也代表之後不久也會呈現在國際版 Evernote？不過先前就在使用國際版的網友也不用擔心，可以直接銜接使用印象筆記，看起來就只是版本不同而已，不是另外創造一個軟體的樣子。

![11.png](/5_Other/Markdown/11.png "11.png")
上圖就是印象筆記使用 Markdown 寫作時的樣子。 在左列直接多了一個 `New Markdown Note` 來新增 Markdown 筆記。工具列也與原來的完全不同，如果不熟 Markdown 語法直接使用工具列也很方便。而編輯區分為兩側，左側編輯書寫，右側負責顯示。歐！真是太迷人了！接下來繼續講述 Evernote，你準備好了嗎？

## Evernote's Markdown VS Markdown
Evernote 上的 Markdown 語法新增了一些方便的語法，來瞧瞧與原來的有那些不一樣！

#### 圖片上的用法
* Evernote 上可以直接複製或拖曳圖片到編輯區，在圖片名稱後面塞入參數調整其長度與寬度：
```
![5bde914ec0616cb10d11ff0f024b478d.png](en-resource://database/3581:1)@w=800
![5bde914ec0616cb10d11ff0f024b478d.png](en-resource://database/3581:1)@h=600
![5bde914ec0616cb10d11ff0f024b478d.png](en-resource://database/3581:1)@w=800h=600
![5bde914ec0616cb10d11ff0f024b478d.png](en-resource://database/3581:1)@h=600w=800
```
* 引用圖片的時候，提供更簡便的語法：
```
● Defulat：![Alt文字][參考名稱]
● Evernote：![Alt文字][參考名稱]   、  ![參考名稱]
```

#### HTML 用法
Markdown 上面可以使用 HTML 元素來書寫，且元素裡面的 Markdown 語法不起作用，Evernote 則是不支援 HTML 語法。

> Evernote 無法使用 HTML，下列 YouTube 影片 HTML 方法無法使用。
> > <a href="https://www.youtube.com/watch?v=JfnlPH3-bwg" target="_blank">
> >     <img src="https://img.youtube.com/vi/JfnlPH3-bwg/0.jpg"
> >             alt="圖片 Alt" width="240" height="180" border="10" />
> > </a>

#### TOC
`[TOC]`是一個依照文章內的標題（H1～H6）自動建立一個具有階層架構目錄的語法。原本的 Markdown 並沒有此語法，Evernote 有支援。使用上也很簡單，只要在欲安插目錄的文章位置打上 `[TOC]`，就會依照文章內的標題去建構目錄。
> 目前 GitHub 沒有支援，必須透過安裝插件才能夠使用。

## Extra feature in Evernote's Markdown
#### 支持圖表（餅圖、柱狀圖、條狀圖、折線圖）
其軸項還有一些變數可以使用 : 
變數名  | 功用
 :-:      | :-:
 x.title | x軸的標題
 x.suffix | x軸後綴 (常用於單位)
 x.prefix | x軸前綴 
 
![12.png](/5_Other/Markdown/12.png "12.png")
![13.png](/5_Other/Markdown/13.png "13.png")


#### 支持流程圖
![14.png](/5_Other/Markdown/14.png "14.png")

#### 支持時序圖
![15.png](/5_Other/Markdown/15.png "15.png")

#### 支持甘特圖
![16.png](/5_Other/Markdown/16.png "16.png")

#### 支持數學公式
![17.png](/5_Other/Markdown/17.png "17.png")


