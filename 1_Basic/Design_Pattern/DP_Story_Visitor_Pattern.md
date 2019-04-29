# 拜訪者模式：劇情故事
### Introduce：
使穩定的資料結構，其易於變化的業務邏輯給封裝成拜訪者物件。有以下特色：
* 業務邏輯與資料結構沒有耦合，增加業務邏輯只要擴展（增加）拜訪者物件即可。
* 易於增加新的業務邏輯，但難於增加資料結構。

### Main：
在較新版的寶可夢遊戲中，玩家可選擇遊玩角色的性別。除了角色外觀不同外，遊戲劇情也不截然相同。這邊在資料結顧穩定的情況下（只有男角色、女角色，兩種類別），再加上易變的業務邏輯（來個 DLC 資料片或是未來版本隨時會增加劇情），最適合使用**訪問者模式**。

### UML：
遊戲角色使用 Player 類別來代表，可決定角色性別是 BoyPlayer（男玩家）還是 GirlPlayer（女玩家），它們都繼承 Player。男女玩家各會經歷不同的劇情故事 Story，所以需要實作 Story 以增加男女角色的劇情。在這個例子中，分別使用 OpponentStory（對手劇情）、MatchStory（比賽劇情）來演示。

![Visitor UML](/1_Basic/Design_Pattern/Image/Visitor_J.png "Visitor UML")

### Code：
Player（玩家）是個抽象類別，除了擁有可以設置名字的 `name` 屬性外，也定義了一個抽象方法 `entryStory`（進入劇情）。子類別將實作這個方法達到不同姓別的角色擁有不同的劇情。

```Java
/** 玩家 **/
public abstract class Player {
    private String name;
    public Player(String name) {
        this.name = name;
    }
    /** 進入劇情 **/
    public abstract void entryStroy(Story story);

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

Story（劇情）是個介面，它定義了實例實作男女角色劇情的方法：ofBoy（男生劇情）、ofGirl（女生劇情）。

```Java
/** 劇情 **/
public interface Story {
    /** 男生劇情 **/
    void ofBoy(BoyPlayer boyPlayer);
    /** 女生劇情 **/
    void ofGirl(GirlPlayer girlPlayer);
}
```

BoyPlayer（男玩家）、GirlPlayer（女玩家）實作 Player 的 `entryStory` 方法，將自己設為參數，分別呼叫參數 `story` 的 `ofBoy`、`ofGirl` 方法，將劇情的職責委派給 Story 類別。

```Java
/** 男玩家 **/
public class BoyPlayer extends Player {
    public BoyPlayer(String name) {
        super(name);
    }

    @Override
    public void entryStroy(Story story) {
        story.ofBoy(this);
    }
}
```
```Java
/** 女玩家 **/
public class GirlPlayer extends Player {
    public GirlPlayer(String name) {
        super(name);
    }

    @Override
    public void entryStroy(Story story) {
        story.ofGirl(this);
    }
}
```

OpponentStory（對手劇情）、MatchStory（比賽劇情）實作 Story 的 `ofBoy`、`ofGirl` 方法，已達成男女角色劇情的需求。

```Java
/** 對手劇情 **/
public class OpponentStory implements Story {
    @Override
    public void ofBoy(BoyPlayer boyPlayer) {
        System.out.printf("%s玩家在前往深灰市的途中遇到勁敵小茂！%n", boyPlayer.getName());
    }

    @Override
    public void ofGirl(GirlPlayer girlPlayer) {
        System.out.printf("%s玩家在華藍市的噴水池前遇到閨密小玲！%n", girlPlayer.getName());
    }
}
```
```Java
/** 比賽劇情 **/
public class MatchStory implements Story {
    @Override
    public void ofBoy(BoyPlayer boyPlayer) {
        System.out.printf("%s玩家將參加格鬥大賽！%n", boyPlayer.getName());
    }

    @Override
    public void ofGirl(GirlPlayer girlPlayer) {
        System.out.printf("%s玩家將參加華麗大賽！%n", girlPlayer.getName());
    }
}
```

### Sample：

接下來就來演示怎麼載入男女角色的劇情：

```Java
/** 訪問者模式 - 示範 **/
public class VisitorDemo {
    public static void main(String[] args) {
        Player boy = new BoyPlayer("小明");
        Player girl = new GirlPlayer("小美");
        /** 勁敵劇情 **/
        OpponentStory oStory = new OpponentStory();
        boy.entryStroy(oStory);
        girl.entryStroy(oStory);
        /** 比賽劇情 **/
        MatchStory mStory = new MatchStory();
        boy.entryStroy(mStory);
        girl.entryStroy(mStory);
    }
}
```

![Visitor Result](/1_Basic/Design_Pattern/Image/Visitor_R.png "Visitor Result")
