# 命令模式：寶可夢中心的治療機器
### Introduce：

將請求封裝成物件，使得能夠將請求客製化。有以下特色：
*  客製化功能如：
    1.  將請求排入佇列
    2.  能夠決定請求是否需要被執行、取消、或者再次執行
    3.  將請求操作等相關資訊紀錄成日誌
* 請求也是一種業務邏輯，它與達成需求的業務邏輯完美的分開

### Main：

相信玩家時常會遇到這種情況，在對戰的時候，寶可夢陷入了異常狀態，而這異常狀態到對戰結束後，還沒消失。讓筆者簡單介紹一下什麼是異常狀態，比如自己的寶可夢被火系寶可夢使用火系絕招後，容易進入〝灼傷〞狀態。遭受催眠攻擊，容易進入〝睡眠〞狀態。遭受毒藥攻擊，容易進入〝中毒〞狀態。還有很多狀態就不一一列舉。不過受了異常狀態的寶可夢，像是中毒之類的狀態就必須趕快給予解毒劑，或是到寶可夢中心治療。否則每走幾步，寶可夢就會越來越虛弱！到了寶可夢中心，馬上交給喬伊小姐。她就會放到一台機器裡面，沒過多久音樂響幾聲，你的寶可夢就全數恢復健康了。其實在這裡有著**命令模式**的蹤跡。

### UML：
我們可以先從 Nurse（喬伊小姐）來看，她負責接收玩家的請求，所以有個 HealRequest（治療請求）抽象類別集合。實作此類別的有 HealSleepRequest（治療睡眠請求）、HealBurnRequest（治療灼傷請求）、HealPoisonRequest（治療中毒請求）。這些實作分別使用 MachineHealer（治療機器）的方法來實作自己的邏輯。

![Command UML](/1_Basic/Design_Pattern/Image/Command_J.png "Command UML")

### Code：

Nurse（喬伊小姐），有個 HealRequest 抽象類別的集合 `request`，她提供了 `request`、`cancel` 方法處理玩家的請求。可以看到這邊對於請求多了一些控管邏輯，比如當玩家提供超過六個寶可夢就會出現提示「寶可夢最多只能六個！」，以及紀錄請求的時間與何種治療。當玩家請求完畢，喬伊小姐就可以使用 `putAllPokeball` 方法將所有寶貝球放入治療機器裡，交給治療機器治癒寶可夢。在這邊可以看到**額外處理請求的邏輯**，**請求邏輯與業務邏輯解耦合**，這都是命令模式的特色！

```Java
/** 喬伊小姐 **/
public class Nurse {
    private List<HealRequest> requests = new ArrayList();

    /** 請求治療 **/
    public void request(HealRequest request) {
        if (requests.size() >= 6) {
            System.out.println("寶可夢最多只能六個！");
            return;
        }
        requests.add(request);
        System.out.printf("玩家請求%s，紀錄時間：%s%n", request,
                new SimpleDateFormat("HH:mm:ss").format(new Date()));
    }

    /** 取消治療 **/
    public void cancel() {
        requests.clear();
        System.out.println("已取消所有寶可夢治療！");
    }

    /** 將玩家請求治療的寶貝球放入治療機器裡 **/
    public void putAllPokeball() {
        if (requests.size() == 0) {
            System.out.println("沒有選擇要治療的寶可夢！");
            return;
        }
        for (HealRequest request: requests)
            request.heal();
    }
}
```

HealRequest（治療請求）是個抽象類別，有個屬性 MachineHealer `healer`。目的是為了讓其子類別能夠利用 `healer` 實作 `heal`（治療）方法。 

```Java
/** 治療請求 **/
public abstract class HealRequest {
    protected MachineHealer healer;

    public HealRequest(MachineHealer healer) {
        this.healer = healer;
    }

    /** 治療 **/
    public abstract void heal();
}
```

HealSleepRequest（治療睡眠請求）、HealBurnRequest（治療灼傷請求）、HealPoisonRequest（治療中毒請求），都繼承 HealRequest 並利用 MachineHealer 來實作 `heal` 方法。

```Java
/** 治療灼傷請求 **/
public class HealBurnRequest extends HealRequest {
    public HealBurnRequest(MachineHealer healer) {
        super(healer);
    }

    @Override
    public void heal() {
        this.healer.healBurnStatus();
    }

    @Override
    public String toString() {
        return "治療灼傷";
    }
}
```
```Java
/** 治療中毒請求 **/
public class HealPoisonRequest extends HealRequest {
    public HealPoisonRequest(MachineHealer healer) {
        super(healer);
    }

    @Override
    public void heal() {
        this.healer.healPoisonStatus();
    }

    @Override
    public String toString() {
        return "治療中毒";
    }
}
```
```Java
/** 治療睡眠請求 **/
public class HealSleepRequest extends HealRequest {
    public HealSleepRequest(MachineHealer healer) {
        super(healer);
    }

    @Override
    public void heal() {
        this.healer.healSleepStatus();
    }

    @Override
    public String toString() {
        return "治療睡眠";
    }
}
```

MachineHealer（治療機器）則提供了各種治療方法：`healBurnStatus`、`healSleepStatus`、`healPoisonStatus`。

```Java
/** 寶可夢中心的治療機器 **/
public class MachineHealer {
    /** 治療灼傷狀態 **/
    public void healBurnStatus() {
        System.out.println("替寶可夢治療灼傷狀態！");
    }

    /** 治療睡眠狀態 **/
    public void healSleepStatus() {
        System.out.println("替寶可夢治療睡眠狀態！");
    }

    /** 治療中毒狀態 **/
    public void healPoisonStatus() {
        System.out.println("替寶可夢治療中毒狀態！");
    }
}
```

### Sample：
玩家在與 NPC 對戰後，將異常狀態的寶可夢送往寶可夢中心...。

```Java
/** 命令模式 - 示範 **/
public class CommandDemo {
    public static void main(String[] args) {
        System.out.println("玩家依序將有著異常狀態的寶可夢交給喬伊小姐");
        Nurse nurse = new Nurse();
        MachineHealer healer = new MachineHealer();
        /** 連續將七隻燒傷寶可夢交給喬伊小姐 **/
        nurse.request(new HealBurnRequest(healer));
        nurse.request(new HealBurnRequest(healer));
        nurse.request(new HealBurnRequest(healer));
        nurse.request(new HealBurnRequest(healer));
        nurse.request(new HealBurnRequest(healer));
        nurse.request(new HealBurnRequest(healer));
        /** 這一隻將會出現警告訊息：提醒最多只能治療六隻寶可夢 **/
        nurse.request(new HealBurnRequest(healer));
        /** 不治療了，想要另外治療其他隻寶可夢，所以取消所有請求 **/
        nurse.cancel();
        /** 再次請求治療其他隻寶可夢 **/
        nurse.request(new HealBurnRequest(healer));
        nurse.request(new HealSleepRequest(healer));
        nurse.request(new HealPoisonRequest(healer));
        nurse.putAllPokeball();
    }
}
```

![Command Result](/1_Basic/Design_Pattern/Image/Command_R.png "Command Result")

