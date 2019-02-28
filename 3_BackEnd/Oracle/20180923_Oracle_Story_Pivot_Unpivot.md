# Oracle - Pivot & Unpivot : 心動

![Pivot_Unpivot Image](/3_BackEnd/Oracle/Image/Oracle_Pivot_Unpivot.png "Pivot_Unpivot Image")

仿柚木質感的長桌上面有一張疑似小希筆記的影本，旁邊則擺放著筆盒與水壺，還有一大疊厚厚的書。它們的主人公手裡拿著一本雜誌正在翻閱，嘴巴正念念有詞。

「哇~！小希真的好美歐！」

窗外的綠繡眼佇立在枝頭上，正在凝視窗內的一舉一動。

「齁~！Angela 這個 Pose 也太撩人，真令人血脈賁張。可惜上次聯誼沒談成功！」

窗外的楓樹已被名為秋天的藝術師染成鮮紅色彩。

「真想要跟 Jessie 一起登山阿！這樣子沒水就可以...」

「就可以怎樣啊~？」

這位男學生被小希突如其來地問道嚇得大叫，像極了在窗外也被嚇到飛走的綠繡眼那模樣，臉都綠了！

「Eli！你還在這裡悠閒的看雜誌！Yuli 教授請你做的事做好了嗎？」

小希一臉不悅地問 Eli。

「當然都做好了阿！你怎麼在這裡？」

「我要忙一些事情，你可以幫我整理一下我們班級的請假單嗎？」

「歐歐！當然可以啊，交給我吧！」，Eli 一臉胸有成竹的說著。

「畢竟，你常常幫我的忙阿！此恩不報非君子！」。

小希聽著這一句話，臉上的不悅漸漸消失。
經過一段時間後。

「小希，我整理好了！」

只見 Eli 拿著一張表，紀錄了小希班上的請假紀錄。

```SQL
-- 請假表
create table HOLIDAY (
        NAME varchar(20) not null,  --學生姓名
        H_DATE varchar(8) not null, --請假時數
        KIND varchar(2) not null    --請假種類
);
```

```SQL
-- 請假資料  ('事假' : 1, '病假' : 2, '公假' : 3)
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Johnson','2','1');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('CH','5','2');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Amy','3','1');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Johnson','8','3');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('CH','2','1');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Amy','1','3');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Cathy','4','2');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('小希','7','3');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Johnson','2','2');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Johnson','3','3');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('CH','3','2');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Amy','4','1');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Cathy','4','3');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('小希','4','1');
Insert into HOLIDAY (NAME,H_DATE,KIND) values ('Cathy','2','3');
```

「然後，我也整理出了每個人請了多少時間的假！」，Eli 一臉神氣昂昂的展示他手上的成果：

```SQL
--- 使用Group by 子句得出結果
select NAME 學生姓名,
       decode(KIND, 1, '事假', 2,  '病假', 3, '公假') 請假種類, 
       sum(H_DATE) 請假時數 
from HOLIDAY 
group by NAME, KIND
order by NAME;
```

學生姓名 | 請假種類 | 請假時數
:-: | :-: | :-:
Amy | 事假 | 7
Amy | 公假 | 1
CH | 事假 | 2 
CH | 病假 | 8 
Cathy | 病假 | 4 
Cathy | 公假 | 6
Johnson | 事假 | 2 
Johnson | 病假 | 2 
Johnson | 公假 | 11 
小希 | 事假 | 4 
小希 | 公假 | 7

「恩，勉強可以拉！不過阿~統計表能不能幫我調整一下，將事假、病假、公假變成欄位 header 呢？像這樣！」小希邊說邊在一張白紙上畫出想要的統計圖表樣式。

![Oracle_Pivot_Sample Image](/3_BackEnd/Oracle/Image/Oracle_Pivot_Sample.png "Oracle_Pivot_Sample Image")

想要在小希面前耍帥的 Eli，看著這張統計圖表樣式，原本的神氣貌漸漸消失。

「痾...好，等一，等一會我給你！」Eli 有點結巴。

又過了一會，小希忙完自己身邊的工作，然後看到 Eli 面有難色地低頭苦思。

「Eli~！ 好了嗎？」

「痾...那個...還沒...我試了很多次，但是始終無法呈現你要的樣子！感覺很複雜！」Eli 愁眉苦臉地說著。

「真遜耶！這很簡單耶！來！我來給你看怎麼辦到這樣子的圖表！」這次換小希神氣昂昂。

小希迅速地 Key 完指令，並按下 Enter，圖表隨之出現在兩人的眼前：

```SQL
--- 使用 PIVOT 去彙整得出結果
select * from (
        select NAME, KIND, H_DATE 
        from HOLIDAY
)
PIVOT (
        sum(H_DATE) for KIND in (1 事假, 2 病假, 3 公假)
);
```

NAME | 事假 | 病假 | 公假 
:-: | :-: | :-: | :-:  
Johnson | 2 | 2 | 11 
CH | 2 | 8 | (null)
Amy | 7 | (null) | 1 
Cathy | (null) | 4 | 6
小希 | 4 | (null) | 7

「歐歐！這樣看起來好像挺簡單的！」，Eli 恍然大悟地接著說：

「在 `PIVOT`  裡 `for` 後面指定 `KIND 請假種類` 去做時數的加總 `sum(H_DATE)`，最後在 `in` 後面選擇所有的種類並命名別名。如此一來就可以將結果以不同的面向顯示出來！那如果我想要翻轉回去，也就是將結果倒回去的話，也可以辦得到嗎？」。

「當然可以囉！我們先把這個結果存進這個表 PIVOT_HOLIDAY」，小希接著又打了一段，按下 Enter：

```SQL
--- 使用 UNPIVOT 再彙整
select NAME 學生姓名,
--將請假種類轉換成代碼
        decode(KIND, '事假', 1, '病假', 2, '公假', 3) 請假種類,
        H_DATE 請假時數
from PIVOT_HOLIDAY
UNPIVOT 
-- INCLUDE NULLS  -- 顯示NULL的資料，預設是排除的
( 
        H_DATE for KIND in (事假 , 病假, 公假)
);
```

學生姓名 | 請假種類 | 請假時數
:-: | :-: | :-: 
Johnson | 1 | 2
Johnson | 2 | 2
Johnson | 3 | 11
CH | 1 | 2 
CH | 2 | 8
Amy | 1 | 7
Amy | 3 | 1
Cathy | 2 | 4
Cathy | 3 | 6
小希 | 1 | 4
小希 | 3 |7

「不過因為轉換前的資料已經是彙整過了，所以轉換後就無法再細分加總前的資料囉！只能單純轉向，回到原來的面向！」小希說道。

「喔喔！原來如此！不愧是品學兼優的正妹小希！」

只見小希脹紅著臉，那微笑像極了秋天的下弦月，那麼如此絕美且令人心醉神迷。但沒過多久，卻又消失得無影無蹤，並冒出了一句話 : 

「所以你跟 Jessie 登山，沒水就可以怎樣？」。