# Oracle ─ Connect By：小希的大學冒險

分層查詢（Hierarchical Query）是 Oracle 提供的一個好用功能。執行速度會比一般使用 `left join` 語法來實作查詢快上很多，在資料具有階級的情況下，是個不二選擇。現在筆者嘗試使用故事來介紹怎麼使用，Let's go！
* * *

小希在今年大學指考的時候考了 75 級分錄取了伊萊大學吸塵系。身為大一新生的她興致勃勃地馬上跑到系學會，想要加入並為吸塵系服務。

吸塵系學會底下有幾個 Team，每個 Team 各司其職完成他們的任務，如下：

```SQL
CREATE TABLE TEAM
( MEMBER_TITLE VARCHAR2(20),--成員頭銜
 MEMBER_NAME VARCHAR2(20), --成員姓名
 LEADER_NAME VARCHAR2(20), --隊長姓名
  STATUS VARCHAR2(2)        --成員狀態 (0: 沒空、1: 有空)
);
```

小希的活潑個性與天生麗質的外表，馬上就被會長 David 熱烈歡迎，一一介紹該系學會的所有成員給小希認識：
```SQL
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('會長','David',null,'1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('副會長','Johnson','David','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('組長','Eric','David','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('組長','CH','David','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('組長','Amanda','Johnson','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Ted','Eric','0');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Nick','CH','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Amy','CH','0');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Jessie','Amanda','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Eli','Eric','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Vivi','CH','1');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Cathy','Amanda','0');
Insert into TEAM (MEMBER_TITLE,MEMBER_NAME,LEADER_NAME,STATUS) values ('隊員','Mark','Eric','1');
```

小希如願以償地加入了吸塵系學會。過了一段時間，由於小希出色的能力，馬上就被指派了任務。她必須找到以副會長 Johnson 為首，以及他底下優秀的成員，來籌辦這次的**吸力大競賽**活動。所以小希馬上就使用下列語法找出成員：

```SQL
---------從上層到下層--------- 
select T.*  from TEAM T
--以Johnson為首，所以起始節點設成
start with MEMBER_NAME = 'Johnson'  
--此行代表現在的節點MEMBER_NAME，會等於下一層的LEADER_NAME
connect by prior MEMBER_NAME = LEADER_NAME
--然後以 MEMBER_NAME 做降冪排序
order siblings by MEMBER_NAME desc;
```

MEMBER_TITLE | MEMBER_NAME | LEADER_NAME | STATUS
:-: | :-: | :-: | :-: 
副會長 | Johnson | David | 1
組長 | Amanda | Johnson | 1
隊員 | Jessie | Amanda | 1 
隊員 | Cathy | Amanda | 0

關鍵字 | 說明 
:-: | :-
start with  | 指定起始節點，該節點為查詢結果的根節點（最高節點）。
connect by prior | 指定層次結構的父節點和子節點之間的關係。`prior` 為一元運算子，等號兩邊可選一側擺放。
siblings | 在 `connect by` 需要排序的時候必須使用。此參數夾在 `order by` 中間指名在 `connect by` 的時候，以某個欄位做排序。

雖然找到了 Johnson 團隊所有成員，不過大學課業繁忙，小希必須詢問成員是否有空（Status：1 代表有空），因此再添加了一個 `where STATUS = 1`，找出有空的成員。

```SQL
select T.*  from TEAM T
where STATUS = 1 -- 找出有空的成員
--以Johnson為首，所以起始節點設成
start with MEMBER_NAME = 'Johnson'  
--此行代表現在的節點MEMBER_NAME，會等於下一層的LEADER_NAME
connect by prior MEMBER_NAME = LEADER_NAME
--然後以 MEMBER_NAME 做降冪排序
order siblings by MEMBER_NAME desc;
```

活動就在小希的高執行力下，召集了相關成員，並順利完成了吸力大競賽。在小希令人驚豔的能力下，慢慢地有越來越多的人崇拜她。系學會的 Eli 就是一個例子，他懇求小希幫忙查詢他的長官們是誰（天啊，因為他忘記了）。小希馬上就使用了下列語法，幫助 Eli 從下往上找到他的長官們是誰：

```SQL
---------從下層到上層---------
select * from TEAM
--起始節點設為 Eli
start with MEMBER_NAME = 'Eli'
--找出現在節點LEADER_NAME 等於下一層的MEMBER_NAME
connect by prior LEADER_NAME = MEMBER_NAME;
```

MEMBER_TITLE | MEMBER_NAME | LEADER_NAME | STATUS
:-: | :-: | :-: | :-: 
隊員 | Eli | Eric | 1
組長 | Eric | David | 1
會長 | David | (null) | 1

小希不僅在系學會表現非常突出，在課業上也是絕頂厲害，她幫助了組長 Amanda 解決數學問題。譬如有一題是這樣的，有一方程式 y = 1 + x * 2 ，求出 x 為 1~10 的所有數。小希用下列語法來求出：

```SQL
SELECT 1 + (LEVEL * 2) NUM 
FROM DUAL
CONNECT BY ROWNUM <= 10;
```
NUM | 
:-:
3|
5|
7|
9|
11|
13|
15|
17|
19|
21|

小希的應變能力也是一流。在前些日子，系學會的電腦突然出現了一個錯誤訊息：`ORA-01436: CONNECT BY loop in user data`，導致系統無法使用。這對於系學會來說是非常嚴重的事情，因為不久之後就要使用到電腦裡的重要文件。在這種情況下，系學會的大家每個都是哭天喊地，不知道怎麼辦。只有小希處變不驚，使用了下列語法：

```SQL
---------找出重複的循環---------
select MEMBER_NAME, LEADER_NAME,
case when CONNECT_BY_ISCYCLE = 1 then '是' else '否' end 錯誤,
LEVEL 層數,
CONNECT_BY_ROOT MEMBER_TITLE 直屬最大長官, 
case when CONNECT_BY_ISLEAF = 1 then '是' else '否' end 是否為隊員,
SYS_CONNECT_BY_PATH(MEMBER_TITLE, '/') 組織路徑 
from TEAM T
start with MEMBER_NAME = 'David'
connect by nocycle prior MEMBER_NAME = LEADER_NAME;
```

MEMBER_NAME | LEADER_NAME | 錯誤 | 層數 | 直屬最大長官 | 是否為隊員 | 組織路徑
:-: | :-: | :-: | :-: | :-: | :-: | :-
David | John | 否 | 1 | 會長 | 否 | /會長 
CH | David | 否 | 2 | 會長 | 否 | /會長/組長 
Amy | CH | 否 | 3 | 會長 | 是 | /會長/組長/隊員
Nick | CH | 否 | 3 | 會長 | 是 | /會長/組長/隊員
Vivi | CH | 否 | 3 | 會長 | 是 | /會長/組長/隊員
Eric | David | 否 | 2 | 會長 | 否 | /會長/組長
Eli | Eric | 否 | 3 | 會長 | 是 | /會長/組長/隊員 
Mark | Eric | 否 | 3 | 會長 | 是 | /會長/組長/隊員
Ted | Eric | 否 | 3 | 會長 | 是 | /會長/組長/隊員
John | David | 是 | 2 | 會長 | 是 | /會長/組長
Johnson | David | 否 | 2 | 會長 | 否 | /會長/副會長
Amanda | Johnson | 否 | 3 | 會長 | 否 | /會長/副會長/組長
Cathy | Amanda | 否 | 4 | 會長 | 是 | /會長/副會長/組長/隊員
Jessie | Amanda | 否 | 4 | 會長 | 是 | /會長/副會長/組長/隊員

**偽列（Pseudocolumns） 說明：**

偽列參數 | 說明 
:- | :-
LEVEL | 表示節點階級
CONNECT_BY_ROOT | 返回當前節點的根節點欄位名稱 
CONNECT_BY_ISLEAF | 判斷是否為葉子節點。葉子節點代表下面不會有任何的子節點。
SYS_CONNECT_BY_PATH（COLUMN, SEPARATOR）| 節點詳細路徑。`COLUMN` 為顯示路徑的名稱，`SEPARATOR` 指定用什麼字元來隔開。
CONNECT_BY_ISCYCLE | 是否為父子循環的具體節點，必須搭配 `NOCYCLE` 使用。
NOCYCLE | 可避免父子循環所造成的錯誤（ORA-01436）。通常搭配 `CONNECT_BY_ISCYCLE` 來使用。

隨著查詢結果出來，答案似乎就寫在每個人的臉上。原來是不知道哪一個粗心的人，把David 的直屬長官，KEY 成新進來的人員，導致 David 和 John 互為長官，因此才會出現這個錯誤 **ORA-01436**。  

隨後系學會馬上修正這個錯誤，化解了這次的危機。系學會的眾人見識到小希的厲害，小希的表現，更傳播到了伊萊大學的每個角落。不過樹大便招風，花香便招蜂，正在有人忌妒小希的表現，正準備暗算小希呢！小希未來會遭遇什麼樣的挑戰呢？