# Oracle ─ Analytic Functions：大考 X 督導 X 優良學生的尋找

「噹~噹~噹...」。

隨著下課鈴聲響起，每個學生的紙筆便紛紛放下，將考試卷交到站在講台上的教授面前。這是伊萊大學一年一度的品質鑑測，此鑑測將會攸關到每個學生未來畢業後，是否能夠順利找到工作。雖說鑑測應該是嚴肅的，但在這裡卻看不出來。當腳步一踏出那教室門口的門檻後，大家便歡樂地談論等等要去哪玩。

「ㄟ～David，等等一起去衝山阿？」
「Amy、Vivi，今天有奇奇蒂蒂特展耶！」
「Eli，你又要去辦聯誼囉？什麼？你說要不要一起去跟化妝品系公關洽談？才不要咧~」

隨著討論聲的慢慢消落，教室終於恢復了平靜，只留下了教授改卷的聲音與咒罵聲...。

![Analytic_Functions Image](/3_後端 Front-end/Oracle/Image/Oracle_Analytic_Function.png "Analytic_Functions Image")

小希停下寫了一陣子的手，眼睛掃視剛剛做的筆記。

「解析函數...」。
「這不是小希嗎！你怎麼在圖書館？」，Yuli 教授滿臉疑問，納悶地問到。
「教授好！我想要先預習一下之後上課的內容阿~」。
「教授，你怎麼看起來有點焦急？」。

小希發現 Yuli 教授衣衫有點凌亂外，飄逸的散髮雖有花香，但卻凋謝在肩膀上。一顆顆汗珠在嫩白的臉頰上滑動，互相競速看誰最快抵達香肩大道。

「真不好意思讓小希你看見我的糗樣。」Yuli教授調整了一下呼吸，接著說：
「考試才剛結束，督導馬上就來視察，並想要看這次的大家考得如何。所以教授們馬上改考卷，並回報學生的成績，你看！」

班級 | 學生姓名 | 分數 || 班級 | 學生姓名 | 分數
:-: | :-: | :-: |:-:| :-: | :-: | :-: 
1|David|76||2|Johnson|70
3|Eric|72||2|CH|80
3|Amanda|82||1|Ted|65
3|Nick|40||2|Amy|80
3|Jessie|70||3|Eli|65
1|Vivi|68||2|Cathy|77
3|Mark|55||3|John|缺考
1|Sharon|99||2|小希|98

「咦？怎麼沒有依照班級排列呢？」。
「沒辦法呀～時間太緊迫了，教授們聚集在一起，改好一個就馬上紀錄起來。完成後就要馬上交給督導看了，哪有什麼時間整理阿！」Yuli 教授又接著說：
「督導看了我們給的成績單，皺著眉頭說：『就這樣？這成績單可以在更詳細點嗎？沒有班平均？系平均？你們也沒有幫學生的成績做排名？這樣子我怎麼看阿！』」

Yuli 教授忿忿不平地闡述督導的要求：

「『馬上去給我重用，一個小時內我要看到！對了，再加上一個累計平均紀錄，我想要看到系平均是怎麼被拉下來的！』。這就是我為什麼會這麼著急的原因。」
「教授，我來幫你吧！我能夠馬上幫你產出督導所要的報表！」小希若由所思的想了一下，便如此回答道 。
「真的嗎？那就拜託小希了！」，Yuli 教授凋謝的頭髮似乎微微上揚，找到了希望的曙光。

「真的能夠辦到嗎？時間只剩下十分鐘而已！」
「可以的，放心吧！教授！」

小希一邊從容不迫地走向圖書館的電腦，一邊安撫著 Yuli 教授的情緒。小希啪啦啪啦打著鍵盤，先建立了成績的表格：

```SQL
CREATE TABLE SCORE  (
     "CLASS" VARCHAR2(10 BYTE), 
     "NAME" VARCHAR2(20 BYTE), 
     "SCORE" VARCHAR2(20 BYTE)
 );
```

再將學生的成績紀錄，依序塞進表格內。

```SQL
Insert into SCORE (CLASS,NAME,SCORE) values ('1','David','76');
Insert into SCORE (CLASS,NAME,SCORE) values ('2','Johnson','70');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','Eric','72');
Insert into SCORE (CLASS,NAME,SCORE) values ('2','CH','80');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','Amanda','82');
Insert into SCORE (CLASS,NAME,SCORE) values ('1','Ted','65');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','Nick','40');
Insert into SCORE (CLASS,NAME,SCORE) values ('2','Amy','80');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','Jessie','70');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','Eli','65');
Insert into SCORE (CLASS,NAME,SCORE) values ('1','Vivi','68');
Insert into SCORE (CLASS,NAME,SCORE) values ('2','Cathy','77');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','Mark','55');
Insert into SCORE (CLASS,NAME,SCORE) values ('3','John',null);
Insert into SCORE (CLASS,NAME,SCORE) values ('1','Sharon','99');
Insert into SCORE (CLASS,NAME,SCORE) values ('2','小希','98');
```

「來不及了拉！我完蛋了，只剩下五分鐘而已！」Yuli 教授白皙的臉頰變得通紅，汗珠比剛剛又更多了。
「別著急！」，小希用迅雷不及掩耳的速度，咖搭咖搭完成了一段 SQL。

```SQL
select CLASS 班級, NAME 姓名, SCORE 分數,
--平均方面，四捨五入。班平均以班為單位去做計算。
--系平均是所有班級都一起去計算，所以不須特別指定數值，使用null
round(avg(SCORE) over (partition by "CLASS"),2) 班平均,
round(avg(SCORE) over (partition by null),2) 系平均,
--排名方面，分數越高，名次越高。所以order by SCORE desc。
--但是缺考沒成績者應該名次要最低，所以再指定nulls last
--班排名使用跳躍排序 (兩個第二名，接下來是第四名)；系排名使用連續排序 (兩個第二名，接下來仍是第三名)
rank() over (partition by "CLASS" order by SCORE desc nulls last) 班排名,
dense_rank() over (partition by null order by SCORE desc nulls last) 系排名,
--這邊的累計平均就是從分數最高開始，後者與前面所有人去平均。比如第二名會與第一名去平均分數，第三名會與一、二名去平均分數。
round(avg(SCORE) over  (partition by null order by SCORE desc nulls last
--所以使用unbounded當作起始點(起點從第一名開始)，current row 當作終點 (該名學生)
rows between unbounded preceding and current row), 2) 累計平均
from SCORE S ;
```

小希將執行結果印出來，拿給了 Yuli 教授。

「太快了吧！沒問題嗎？」

儘管Yuli教授內心忐忑不安，但實在沒辦法，只剩下三分鐘。便利用僅剩下的三分鐘，跑到了督導所在的辦公室，將方才列印出來的熱騰騰成績單，給督導過目：

班級 | 姓名 | 分數 | 班平均 | 系平均 | 班排名 | 系排名 | 累計平均
:-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: 
1 | Sharon | 99 | 77 | 73.13 | 1 | 1 | 99 
2 | 小希 | 98 | 81 |73.13|1|2|98.5
3 | Amanda | 82 | 64 | 73.13 | 1 | 3 | 93
2 | Amy | 80 | 81 | 73.13 | 2 | 4 | 89.75
2 | CH | 80 | 81 | 73.13 | 2 | 4 |  87.8  
2 | Cathy | 77 | 81 | 73.13 | 4 | 5 | 86 
1 | David | 76 | 77 | 73.13 | 2 | 6 | 84.57
3 | Eric | 72 | 64 | 73.13 | 2 | 7 | 83
2 | Johnson | 70 | 81 | 73.13 | 5 | 8 | 81.56
3 | Jessie | 70 | 64 | 73.13 | 3 | 8 | 80.4 
1 | Vivi | 68 | 77 |  73.13 | 3 | 9 | 79.27 
1 | Ted | 65 | 77 | 73.13 | 4 | 10 | 78.08 
3 | Eli | 65 | 64 | 73.13 | 4 | 10 | 77.08 
3 | Mark | 55 | 64 | 73.13 | 5 | 11 | 75.5
3 | Nick | 40 | 64 | 73.13 | 6 | 12 | 73.13
3 | John | (null) | 64 | 73.13 | 7 | 13 | 73.13

Yuli 教授 90 度鞠躬，頭低的不能再低，深怕一抬頭便看到督導怒視的臉。過了一陣子，督導龍心大悅，輕輕地用食指抬起了 Yuli 教授的下巴，並單膝跪在地板上。

「ㄟ！你們在幹嘛！」Eric 撞見後，大聲喝斥道：「你要對阿季幹嘛？」。

督導站起身來說道：

「我正要跟教授講：『做得很好，這就是我想要的！』，請問你是...？」

Yuli 教授挽起袖子，連忙將 Eric 的頸子挽再自己的手肘內，並說：

「不好意思，年輕人血氣方剛，你看，他的臉都紅了！這是我的學生，叫做 Eric。」
「還不是你勒我這麼緊，別靠這麼近拉！&$@%！」，Eric 心中 OS。

督導在審核完剛出爐的成績單後，與系主任相談甚歡一段，便驅車離開伊萊大學。

「Yuli 教授，你這次真的幫了我們吸塵系大忙阿。督導對我們讚譽有佳，謝謝你幫助吸塵系提升名聲！」系主任滿臉笑容地說著。
「不不不，主任，這次是因為我們系裡，有個很優秀的學生。她在我慌忙之際，不但緩和我的情緒，更解決了這個問題！她真的很厲害，我希望推薦她為這次吸塵系的優良學生候選人，可以嗎？」
「喔？我們系裡竟然有這號人物，請問她的名字是...？」。

「小希 !」Eric 與 Yuli 教授異口同聲說道。



