# Oracle Sql Loader

命令列語法：
```SQL
sqlldr dbuser/dbpass@dbservice control=ctlName.ctl 
```

## 參數詳解
OPTIONS 內填入需要的命令參數，彼此使用`,`分隔，可使用的參數如下：
* direct：開啟 Direct Path Load，以提升導入效能。如 `direct=true`。
> **Direct Path Load** 是利用 Direct Path API 來達到異步 I/O 的效果。其 Direct Path Load Engine 會將資料塞進 Column Array 並使用異步 I/O（如果 Platform Host 有支援）。這取代了 Bind array buffer 使用 insert sql 導入 database 的方法，以提升效能。（如果主鍵重複將使索引狀態變為 UNUSABLE！）
* skip：忽略的行數，資料檔前面幾行可能是 Header 或是 說明。如 `skip=1`。
* rows：多少筆 commit 一次，默認 64 筆。
* log：紀錄導入的日誌，默認 ctrlName.log
* bad：壞資料，默認 ctrlName.bad
* data：指定要導入資料的來源檔
* errors：設定可允許的錯誤筆數。如 `errors=0` 代表一筆都不能錯。

## 實例介紹
```SQL
OPTIONS (skip=1)    -- OPTIONS 參數
LOAD DATA             -- 宣告開始

CHARACTERSET AL32UTF8  -- 需告編碼方式

INFILE "data.csv"   -- 指定資料檔，可以寫多個 INFILE 指定多個資料檔。
-- 另外還可指定 BADFILE（壞資料）、DISCARDFILE (丟棄資料)

---------------------------------操作類型---------------------------------
-- INSERT：默認值，導入表格必須為空。
-- APPEND：保留舊資料，接續導入新資料。
-- REPLACE：刪除舊資料，替換成導入的資料。
-- TRUNCATE：在導入資料前清空所有資料
-----------------------------------END-----------------------------------
操作類型
INTO TABLE_NAME -- TABLE_NAME 為要導入資料的表格
WHEN 子句            -- 使用條件篩選資料
FIELDS TERMIATED BY 符號 -- 使用符號來分隔，如 "," 代表使用逗號分隔資料
OPTIONALLY ENCLOSED BY 字符  -- 資料兩側有字符。如 """ 代表資料是 "1","2"
TRAILING NULLCOLS -- 允許對應欄位其數值為空
( -- 開始進行資料欄位對應設置，常用方法如下：
    VIRTUAL_COLUMN FILLER,   -- 虛擬欄位，用來跳過自動產號
    ID NUMBER "seq.nextval", -- 指定類型，否則默認為 CHARACTER，並取用序列下一值作為 ID 
    USER_NAME "upper(:USER_NAME)" -- 可使用函數處理欄位數值
    GENDER POSITION(start:end), -- 使用 POSITION 從 start 擷取到 end
    VERSION "0", -- 設定數字型態數值
    TIME sysdate "YYYY-MM-DD HH24:MI:SS", -- 使用 sysdate 並 format
    MEMO constant "這是DEMO", -- 設定字串常數要用 constant 參數
)
```

## 特殊應用
當導入的資料檔其大小超過 10GB 以上，就需要使用平行導入，並且關閉該表格的產生日誌。

#### 1、關閉該表格日誌
```SQL
ALTER   TABLE   TABLE_NAME   nologging; 
```

#### 2、加上參數指令
在 CONTROL 檔案中，其 `LOAD DATA` 上面加上 `UNRECOVERABLE` 以關閉數據庫的日誌。（注意，此指令必須與 `DIRECT` 共同應用）。  

#### 3、命令列
```SQL
sqlldr dbuser/dbpass@dbservice  control=ctlName.ctl direct=true parallel=true
```






