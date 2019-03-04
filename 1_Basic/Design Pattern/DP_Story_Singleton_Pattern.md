# 單例模式：四大天王

### Introduce：

一個類別只有一個實體。有以下特點：

* 類別其成員有一自身類別，在初始化實體後使用全域方法來存取此一實體。
* 在初始自身唯一實體時，如果馬上在自身類別被載入就初始稱為餓漢式獨體類別。如果只在參考到才初始稱為懶漢式獨體類別。
* 通常使用 Double Check 來進行初始邏輯，可避免多執行緒的環境下，初始多個實例。

### Main：

四大天王是寶可夢聯盟的菁英，必須擁有八顆徽章才有資格挑戰四大天王。四大天王每一位各有特色，擁有的寶可夢與攻擊策略也都不一樣。每一位都是獨一無二的存在，如果在多人連線的寶可夢世界裡，就要確保四大天王每一位都只有一個類別，以免系統在四大天王上消耗大量資源。在這裡就可以談到**單例模式**。

### UML：

Lance 是一個單例類別，由 SingletonDemo 來存取測試。

![Singleton UML](/1_Basic/Design Pattern/Image/Singleton_J.png "Singleton UML")

### Code：

Lance （阿渡）是四大天王的王牌。為龍系寶可夢訓練家，所訓練的龍系寶可夢其能力值都是一流的。

它擁有一個自身類別。其建構子存取修飾子為 **private**，使得其他地方不能使用 `new` 來實體化阿渡。想要與阿渡對戰，就必須藉由 Lance 的靜態類別方法 `meetLance( )`來達成。這個方法使用到 Double Check 來確保此類別是唯一實例，我們來講解一下這個方法。

首先，第一個 `if (null == lance)` 會先看看成員 `lance` 是否已實體化（去看看阿渡是否已經睡醒，來到戰鬥擂台邊準備戰鬥）。接下來的 `synchronized (Lance.class)`是為了防止多個執行緒會實體化多個 Lance （想像一下每一位訓練家就是一個執行緒，因此需要管制訓練家。想要見阿渡，請一個一個排隊）。第二個 `if (null == lance)` 也是用來檢查成員 `lance` 是否已實體化。以上這些就可以用來確保 Lance 會有唯一的實體，並且只能透過靜態全域的 `meetLance( )` 方法來獲得 Lance，這就是單例模式的特色。

```Java
/** 四大天王最後一位-龍系寶可夢的支配者 阿渡 **/
public class Lance {
    /** 擁有自身類別 **/
    private static Lance lance;
    /** 封閉建構子 **/
    private Lance() {}
    /** 與阿渡對戰 **/
    public static Lance meetLance() {
        /** Double check **/
        if (null == lance) {
            synchronized (Lance.class) {
                if (null == lance) {
                    lance = new Lance();
                }
            }
        }
        return lance;
    }
    /** 顯示戰鬥資訊 **/
    public void showBattleInfo() {
        System.out.println("阿渡，龍系寶可夢訓練家 ! 最強寶可夢為快龍! ");
    }
}
```

為什麼要有兩個 `if (null == lance)` ? 因為 `synchronized(Lance.class)` 會耗費一些時間與資源去鎖定這個類別。每當存取這個方法的時候都要去這樣耗費實在不值得，所以第一個 `if (null == lance)` 判斷如果有實體的話，就直接跳過 `synchronized` 並返回Lance，避免消耗資源。


### Sample：
現在有一大票訓練家準備挑戰四大天王最強者─阿渡！

第一個範例為存取 Lance 的測試。第二個範例為多執行緒的情況下，是否仍可讓 Lance 保持單一實體。

```Java
/** 單例模式 - 示範 **/
public class SingletonDemo {
    public static void main(String[] args) {
        /** 與阿渡對決 **/
        Lance lance = Lance.meetLance();
        lance.showBattleInfo();
        /** 多執行緒測試 **/
        System.out.println("\n多名玩家要求跟阿渡對戰!");
        askBattle();
        askBattle();
        askBattle();
        askBattle();
        askBattle();
    }
    /** 要求對戰 **/
    private static void askBattle() {
        new Thread() {
            @Override
            public void run() {
                System.out.println(Lance.meetLance());
            }
        }.start();
    }
}
```

可發現多執行緒的情況下，Lance 仍保持單一實體。

![Singleton Result](/1_Basic/Design Pattern/Image/Singleton_R.png "Singleton Result")
