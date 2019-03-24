# 策略模式：戰鬥介面
### Introduce：
封裝一系列的物件（實作同個介面或繼承同樣類別），客戶端只需傳入參數即可建構特定物件，並擁有統一的介面來操作這一系列的物件，這裡以 Context 類別代稱。

* 客戶端只要知道 Context 類別的操作方法即可統一操作物件的方法，不需要知道其實作細節。
* 一系列物件的變化不影響客戶端，彼此不會相互耦合。

### Main：
相信熱愛寶可夢電玩的玩家一定不會忘記寶可夢的戰鬥介面。戰鬥介面指得是，在遇到野生寶可夢或是與 NPC 對戰的時候，進入後其畫面右下角有四個操作選項，分別為戰鬥、選擇道具、更換寶可夢、逃跑。這裡就可以使用**策略模式**來模擬。

### UML：
下圖中可以看見中間有個最大的矩形，那是 BattleContext（戰鬥介面），也就是在對戰中提供一些操作的介面。玩家可以選擇下列操作 `CombatAction` 戰鬥、`UseItemAction` 選擇道具、`ChangePokemonAction` 更換寶可夢、`EscapeAction` 逃跑。這些操作其實就是 BattleAction（戰鬥介面操作）的子類別。 

![Strategy UML](/1_Basic/Design_Pattern/Image/Strategy_J.png "Strategy UML")

### Code：

BattleAction（戰鬥介面操作）是個抽象類別，有著抽象方法 `action`（操作）。

```Java
/** 戰鬥介面操作 **/
public abstract class BattleAction {
    /** 操作 **/
    public abstract void action();
}
```

CombatAction（戰鬥操作）、UseItemAction（選擇道具操作）、ChangePokemonAction（更換寶可夢操作）、EscapeAction（逃跑操作）皆繼承 BattleAction 實作 `action` 方法。

```Java
/** 戰鬥操作 **/
public class CombatAction extends BattleAction {
    @Override
    public void action() {
        System.out.println("選擇戰鬥");
    }
}
```
```Java
/** 選擇道具操作 **/
public class UseItemAction extends BattleAction {
    @Override
    public void action() {
        System.out.println("選擇使用道具");
    }
}
```
```Java
/** 更換寶可夢操作 **/
public class ChangePokemonAction extends BattleAction {
    @Override
    public void action() {
        System.out.println("選擇更換寶可夢");
    }
}
```
```Java
/** 逃跑操作 **/
public class EscapeAction extends BattleAction {
    @Override
    public void action() {
        System.out.println("選擇逃跑");
    }
}
```

BattleContext（戰鬥介面）有個 BattleAction 的屬性其變數名為 `action`，透過方法 `setAction` 所傳入的參數（有四種靜態屬性可選擇： `COMBAT`、`USE_ITEM`、`CHANGE_POKEMON`、`ESCAPE`）決定建構哪種 BattleAction 的子類別。而使用方法 `operation` 即可使用所有 BattleAction 的 `action` 方法。客戶端只需要記住 `operation` 方法，不需要知道怎麼使用 BattleAction 等相關子類別。這就是策略模式的好處！  

```Java
/**
 * 戰鬥介面
 **/
public class BattleContext {
    /**
     * 操作參數
     **/
    public static int COMBAT = 1;           // 戰鬥
    public static int USE_ITEM = 2;         // 使用道具
    public static int CHANGE_POKEMON = 3;   // 更換寶可夢
    public static int ESCAPE = 4;           // 逃跑

    private BattleAction action;

    /**
     * 玩家選擇特定操作
     **/
    public BattleContext setAction(int battleCode) throws Exception {
        switch (battleCode) {
            case 1:
                this.action = new CombatAction();
                break;
            case 2:
                this.action = new UseItemAction();
                break;
            case 3:
                this.action = new ChangePokemonAction();
                break;
            case 4:
                this.action = new EscapeAction();
                break;
            default:
                throw new Exception("未選擇操作！");
        }
        return this;
    }

    /**
     * 戰鬥介面執行操作
     **/
    public void operation() {
        if (this.action == null) return;
        this.action.action();
    }
}
```

### Sample：
玩家準備要進入戰鬥畫面了！

```Java
/** 策略模式 - 示範 **/
public class StrategyDemo {
    public static void main(String[] args) throws Exception {
        /** 玩家遇到野生的超夢，進入戰鬥介面 **/
        BattleContext context = new BattleContext();
        /** 玩家選擇寶可夢進行戰鬥 **/
        context.setAction(BattleContext.COMBAT).operation();
        /** 寶可夢快沒血了，換下一隻 **/
        context.setAction(BattleContext.CHANGE_POKEMON).operation();
        /** 完了，沒寶可夢可換了，趕快道具補血 **/
        context.setAction(BattleContext.USE_ITEM).operation();
        /** 糟糕，道具也沒了，趕快逃跑 **/
        context.setAction(BattleContext.ESCAPE).operation();
        /** 竟然不給我逃，他X&#*$(，激動得不小心讓電動玩具從手中滑落掉到地上 **/
        context.setAction(58398902).operation();
    }
}
```

![Strategy Result](/1_Basic/Design_Pattern/Image/Strategy_R.png "Strategy Result")

