# Oracle ─ Listagg Function：十大金釵

![Listagg Image](/3_BackEnd/Oracle/Image/Oracle_LISTAGG_Function.png "Listagg Image")

Eric 看著手上拿的密技，趴搭趴搭在電腦上輸入了 SQL，並將結果列印出來給了 Yuli 教授。

```SQL
--  系排名由高而低 (名單)
select '吸塵系' 系, LISTAGG(name, ', ') within group (order by SCORE desc nulls last) 系排名單  
from SCORE;
```

系 | 系排名單 | 
:- | :-
吸塵系 | Sharon, 小希, Amanda, Amy, CH, Cathy, David, Eric, Jessie, Johnson, Vivi, Eli, Ted, Mark, Nick, John

```SQL
-- 班排名由高而低 (名單)
select class 班級, LISTAGG(name, ', ') within group (order by SCORE desc nulls last) 班排名單 
from SCORE
group by class;
```

班級 | 班排名單 
:-: | :-
1 | Sharon, David, Vivi, Ted
2 | 小希, Amy, CH, Cathy, Johnson
3 | Amanda, Eric, Jessie, Eli, Mark, Nick, John

「ㄟ~給你吧！這是剛剛系主任想要的資料！」Eric 向 Yuli 教授說道。
「謝謝耶！你真是幫了我一個大忙，等等請你吃飯！」Yuli 教授才剛整理完面對督導時，凌亂不堪的儀態，再聽到 Eric 的話語後，容光煥發地回應著。

「ㄟ！Eric~我們剛剛還真有默契吼~。你也認識小希阿？」
「阿季，妳不知道嗎？現在小希可是校園裡的風雲人物耶！不但有著傾國傾城，美若天仙的顏值。而且在吸塵系學會裡大放異彩。學校內的其他系所都爭先恐後想要跟我們舉辦活動呢！還有...」。

當 Eric 正要滔滔不絕繼續講下去的時候，被一聲震耳欲聾，中氣十足的聲音給打斷！

「報！會長大人，末將已將大人所要的追蹤報告收集完畢，共計 1817 份！」一位理著平頭，全身肌肉繃著 Superman Logo 衣服的男生喊著！
「好！辛苦你了，退下吧！」Eric 一臉嚴肅的答道。
「遵命！」。

「會長...追蹤報告...？剛剛那個學生在說什麼？」，Yuli 教授一臉狐疑。
「Yuli 教授，妳的電腦再借我一下。」

只見 Eric 使用 Yuli 教授的電腦，打上一串網址後，打開網頁即看見擁有偌大精實肌肉的男生，正舉著啞鈴展現那唯美線條的封面。Eric 打上帳密登入後，便娓娓道來：

「剛剛那位學生是我健身幫的幹部。這一期的健身幫月刊與偵探社合作，請偵探社秘密調查我們大學內，有哪些女生被男生追求告白。被越多男生追求告白的女生，就代表越有吸引力。所以我們會利用這些調查報告，來選出校園內的十大金釵。這就是我們這一期的主題。我們健身幫會無所不用其極，極力爭取十大金釵的加入，如此一來，就會有很多人慕名而來，我幫就會日益壯大阿！哈哈哈哈哈!」
「你的鬼點子還真多啊~！」

Eric接著開啟了調查報告：

```SQL
CREATE TABLE TRAIL (
        "DEPT" VARCHAR2(20 BYTE),       --- 女孩所在科系
        "GIRL_NAME" VARCHAR2(20 BYTE),  --- 女孩名    
        "BOY_NAME" VARCHAR2(20 BYTE),   --- 男孩名
        "ACTIVE_DATE" VARCHAR2(20 BYTE) --- 男孩告白日期
);
```

然後輸入了一連串的語法，按下 `enter` 鍵：

```SQL
--將分析子句group by起來，並統計每個女生有多少追求者
select T.*, COUNT(*) 追求者數量 from ( 
    select DEPT 系所, GIRL_NAME 女生名,
    
    ---藉由 partition by 統計每個女生有多少男生追求，將追求者的名字用,來隔開。追求者的順序依照 ACTIVE_DATE 來升冪
    LISTAGG(BOY_NAME, ', ') within group(order by ACTIVE_DATE) 
    over (partition by GIRL_NAME) 追求者清單 from TRAIL 
) T
group by 系所, 女生名, 追求者清單
order by 追求者數量 desc;
```

系所 | 女生名 | 追求者清單 | 追求者數量 
:-: | :-: | :- | :-:
吸塵系 | 小希 | Ozzie, Johnson, Otto, CH, Dermot... | 424
吸塵系 | Sharon | Philander, Osmond, Albert, Felix, Justin... | 329
化妝品系 | Angela | Neddy, Rodney, Mark, Ted, Benjamin... | 232
吸塵系 | Jessie | Phil, Cedric, Zhenyi, Van, Jack... | 185
吸塵系 | Vivi | Nick, Jerry, Antony, Vic, Russle... | 157
休閒系 | Alice | Peter, James, Scott, Cook, Will... | 125
手環系 | Anastasia | John, David, Willis, Fairs, Bruce... | 115
冰箱系 | Dale | Pablo, Terence, Peter, Gabriel, Talon...| 109 
幼保系 | Elaine | Jacob, Vic, Haiden, Quincy, Salvatore... | 71
手鍊系 | Kama | Stewart, Maddox, Ralph, Lance, Xander... | 70

「果然跟我預想的一樣！各系最受矚目的女生都在前十名內！」

Eric 看著偵探社所給的調查結果，彷彿看到了未來健身幫充滿光輝的榮景，嘴角不禁微微抽動上揚。

過了幾天。數道叫喊劃破平靜的校園。

「號外！號外！十大金釵在此！歡迎索取最新一期健身幫雜誌！」。

好幾個學生在校園的各地大聲啷嚷，希望能夠受到注目拿取他們的雜誌。或許古今中外皆愛美人，一聽到十大金釵字眼，剎那間萬頭攢動，爭先恐後想要目睹其風采。先拿到雜誌的學生，無不歡聲雷動，喜氣雲騰。

「不好意思，請給我一本~！」
「好！拿去！」。

這位飢渴難耐的男學生也終於拿到了一本，迫不及待地翻開雜誌...。

![Top 10 Beauty Image](/3_BackEnd/Oracle/Image/Oracle_LISTAGG_Function_Beauty.png "Top 10 Beauty Image")

這位男學生看到一半，發現站在旁邊的人不太對勁，立刻拔腿就跑。顫抖的手捏爛了健身幫的雜誌，蒼白的嘴唇被使勁地咬到流出血絲，他們的主人站在垃圾桶旁邊，隨即將雜誌丟往垃圾桶裡...。





