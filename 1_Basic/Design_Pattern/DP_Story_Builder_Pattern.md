# 建造者模式：寶可夢出戰順序

### Introduce：
由一個建造者介面利用抽象工廠的方法，建立一系列的類別，並使用特定順序邏輯將之組合起來。有以下特點：

* 各個物件組成的順序與相關邏輯皆被封裝起來。 
* 只要更換或是新建建造者類別即可添加新的順序邏輯。
* 客戶端不需要去考慮如何組成邏輯，只需要呼叫建造者方法即可。

### Main：

不知道各位玩家在跟 NPC 進行寶可夢對戰的時候，是否有比較傾向先派哪一隻寶可夢上場？最普遍的出戰順序，就是由弱至強，最強的最後才上場，通常稱為王牌。又或者是根據對方先出的寶可夢，來派出擁有優勢的寶可夢來對戰。像這種出場順序就可以來談談**建造者模式**。

### UML：

我們繼續使用上一篇 [抽象工廠模式-均衡的訓練家](DP_Story_Abstract_Factory_Pattern.md "抽象工廠模式") 的類別架構，只多了一個建造者的介面 BattleOrder（出戰順序）。實作 BattleOrder 該介面的有兩個，分別是 WaterFirst（水屬優先）與 FireFirst（火屬優先）。

![Builder UML](/1_Basic/Design_Pattern/Image/Builder_J.png "Builder UML")

### Code：

建造者介面 BattleOrder（出戰順序）。該介面有一個方法 `assignOrder`，其參數接收 `Trainer`（訓練者）介面，來安排寶可夢的出戰順序。

```Java
/** 出戰順序介面 **/
public interface BattleOrder {
    /** 決定出戰順序 **/
    void assignOrder(Trainer trainer);
}
```

實際建造者類別 WaterFirst（水屬優先）、FireFirst（火屬優先）。它們實作方法接受 `Trainer` 參數後，就可以依照各自類別的需求，來使用 `Trainer` 的方法。像 WaterFirst 就先讓 `Trainer` 使用 `assignWaterPokemon` 方法派出水屬性寶可夢，再來才使用 `assignFirePokemon` 方法派出火屬性寶可夢。

```Java
/** 水屬性先上場 **/
public class WaterFirst implements BattleOrder {
    @Override
    public void assignOrder(Trainer trainer) {
        /** 先派出水屬性 **/
        System.out.println(trainer.assignWaterPokemon().getClass().getSimpleName());
        /** 再派出火屬性 **/
        System.out.println(trainer.assignFirePokemon().getClass().getSimpleName());
    }
}
```
```Java
/** 火屬性先上場 **/
public class FireFirst implements BattleOrder {
    @Override
    public void assignOrder(Trainer trainer) {
        /** 先派出火屬性 **/
        System.out.println(trainer.assignFirePokemon().getClass().getSimpleName());
        /** 再派出水屬性 **/
        System.out.println(trainer.assignWaterPokemon().getClass().getSimpleName());
    }
}
```

### Sample：
玩家開始進行一連串的對戰。

從下面的程式碼可看出：出戰的順序被封裝了起來，不需要知道其順序及細節。想要什麼順序以及什麼樣類型的訓練家，只要更換其具體類別即可，這就是建造者模式的特色。

```Java
/** 建造者模式 - 示範 **/
public class BuilderDemo {
    public static void main(String[] args) {
        System.out.println("第一個訓練家將優先派出水屬性寶可夢");
        BattleOrder order = new WaterFirst();
        order.assignOrder(new AggressiveTrainer());
        System.out.println("第二個訓練家將優先派出火屬性寶可夢");
        order = new FireFirst();
        order.assignOrder(new DefensiveTrainer());
    }
}
```

![Builder Result](/1_Basic/Design_Pattern/Image/Builder_R.png "Builder Result")
