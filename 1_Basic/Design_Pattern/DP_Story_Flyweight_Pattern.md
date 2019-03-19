# 享元模式：NPC
### Introduce：
某一類別其屬性內容需要被大量地方引用，在屬性內容不變的情況下，保存此類別並重複使用。有以下特點：
* 避免創建多個重複內容的類別，導致消耗大量記憶體。
* 類別其內部參數是不會隨環境改變的，改變的是透過方法傳入進來的外部參數。

### Main：
NPC 全名為 Non-Player Character，意思是指非玩家角色或非操控角色。在寶可夢的遊戲世界中，從一開始的大木博士，到路上隨處可見的訓練家，都是 NPC。可以從這些 NPC 中發現**享元模式**的存在。

### UML：

BattleNPC（對戰 NPC）與 TalkNPC（對話 NPC）都繼承了 NPC（非玩家角色）這個類別，他們實作的方法會使用到參數 Player（玩家）。而 NpcFactory（NPC 工廠）負責創建並保存玩家遇到的 NPC。

![Flyweight UML](/1_Basic/Design_Pattern/Image/Flyweight_J.png "Flyweight UML")

### Code：
Player（玩家），有個 `name` 屬性保存玩家的名稱。
```Java
/** 玩家 **/
public class Player {
    /** 名稱 **/
    private String name;
    public Player(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
NPC（非玩家角色）是個抽象類別，有著 `name` 屬性保存 NPC 的名稱，並且有個抽象方法 `interact` 需要子類別實作。

```Java
/** 非玩家角色 **/
public abstract class NPC {
    /** 名字 **/
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    /** 互動邏輯 **/
    public abstract void interact(Player player);
}
```

BattleNPC（對戰 NPC）與 TalkNPC（對話 NPC）均繼承了 NPC 並且實作 `interact` 方法。

```Java
/** 對戰 NPC **/
public class BattleNPC extends NPC {
    public BattleNPC(String name) {
        setName(name);
    }
    @Override
    public void interact(Player player) {
        System.out.printf("NPC %s 要求與玩家 %s 進行戰鬥！%n", getName(), player.getName());
    }
}
```
```Java
/** 對話 NPC **/
public class TalkNPC extends NPC {
    public TalkNPC(String name) {
        setName(name);
    }
    @Override
    public void interact(Player player) {
        System.out.printf("NPC %s 與玩家 %s 進行對話！%n", getName(), player.getName());
    }
}
```

NpcFactory（NPC 工廠）負責產生各式各樣的 NPC。`getBattleNPC`、`getTalkNPC` 分別提供 BattleNPC、TalkNPC。而 `getNpc` 是上述這兩個方法的核心方法，可用工廠的靜態參數 `BATTLE`、`TALK` 來指派給參數 `type`，藉此創建指定類型的 NPC。從核心方法邏輯可看到，先從自身屬性 `table` 中查看是否已存在此 NPC。若有則從 `table` 中拿，反之存進 `table` 中並返回此 NPC。

```Java
import java.util.Hashtable;
/** NPC 工廠 **/
public class NpcFactory {
    private static Hashtable<String, NPC> table = new Hashtable();
    public static String BATTLE = "B";
    public static String TALK = "T";

    /** 取得戰鬥 NPC **/
    public static NPC getBattleNPC(String name) {
        return getNpc(name, BATTLE);
    }
    /** 取得對話 NPC **/
    public static NPC getTalkNPC(String name) {
        return getNpc(name, TALK);
    }

    /** 依照給定種類 type 取得該 type 的 NPC **/
    private static NPC getNpc(String name, String type) {
        if (!table.containsKey(name))
            table.put(name, BATTLE.equals(type)? new BattleNPC(name): new TalkNPC(name));
        return table.get(name);
    }
}
```

### Sample：

玩家開始陸續遇到各式各樣的 NPC...。

從下面的程式碼與結果圖片可發現，多個玩家與 NPC 明美進行對話的時候，都是與同個物件互動（記憶體位置一樣）。在沒有使用享元模式的情況下，如果明美本身的邏輯非常複雜（比如關鍵人物），就會花費大量的創建時間與記憶體去保存明美這個 NPC 物件。因此使用享元模式克服上述的缺點，這就是此模式的特色！

```Java
/** 享元模式 - 示範 **/
public class FlyweightDemo {
    public static void main(String[] args) {
        Player player = new Player("Red");

        /** 玩家遇到了對戰 NPC **/
        NPC battleNpc = NpcFactory.getBattleNPC("江川");
        battleNpc.interact(player);

        /** 玩家遇到了對話 NPC **/
        NPC talkNpc = NpcFactory.getTalkNPC("明美");
        talkNpc.interact(player);

        /** 其他玩家在不同的時間點遇到了明美 **/
        String[] players = new String[]{"Blue", "Yello", "Black", "White"};
        for (String each: players) {
            NPC anotherTalkNpc = NpcFactory.getTalkNPC("明美");
            anotherTalkNpc.interact(new Player(each));
            System.out.println("明美的 ID：" + anotherTalkNpc.toString());
        }
    }
}
```
![Flyweight Result](/1_Basic/Design_Pattern/Image/Flyweight_R.png "Flyweight Result")


