# 合成模式：火箭隊

### Introduce：

樹狀結構的物件提供一致性的方法來供客戶端使用。有以下特點：

* 一致性的方法讓客戶端不需要考慮物件是單體物件還是組合物件。
* 定義了清晰的類別層次結構，並可以使用單體物件任意組合成更複雜的物件結構。
* 可在父類別宣告組合的相關方法（`add`、`remove`），又或者只在組合類別宣告。各有優劣，視情況使用哪一方法。

### Main：

> 既然你誠心誠意的發問了
> 我們就大發慈悲的告訴你 
> 為了防止世界被破壞 
> 為了守護世界的和平 
> 貫徹愛與真實的邪惡 
> 可愛又迷人的反派角色 
> 武藏！ 
> 小次郎！ 
> 我們是穿梭在銀河中的火箭隊 
> 白洞、白色的明天正等著我們 
> 就是這樣喵！

相信看過寶可夢的人，對上面火箭隊三人組的開場白並不陌生！火箭隊是一個龐大組織，他們的 Boss-坂木老大，一心一意想要捕捉最強寶可夢-超夢，以及捕捉所有強力寶可夢來支配寶可夢世界。所以火箭隊三人組想出各種計畫，希望能抓走主角的皮卡丘，送給火箭隊的老大，然後受到組織的重用。主角碰到他們會以「火箭隊」來稱呼他們，這裡的火箭隊指得就是火箭隊三人組。而警察君莎小姐常常說火箭隊胡作非為，這裡的火箭隊則是代表整個火箭隊組織。火箭隊這個詞有時代表單體（火箭隊三人組），有時代表全體（火箭隊組織）。不管是單體還是全體，當他們的老大下令行動的時候，他們就必須設法達成老大的期望。在這裡就可以使用**合成模式**來解釋整個火箭隊組織的運作。

### UML：

Team（小隊）、Organization（組織）都繼承 Rocket（火箭隊）這個抽象類別，它們都必須實作父類別的抽象方法。唯一不同的是，Organization 有個 List 集合 `group`，能夠招募小隊，或是汰除不適任小隊。而 Team 是最小單位，無法再招募或汰除。

![Composite UML](/1_Basic/Design Pattern/Image/Composite_J.png "Composite UML")

### Code：

Rocket（火箭隊）是個抽象類別。身為火箭隊一員必須要有一個目標，所以擁有 `name`（名稱）以及 `target`（目標）屬性。並且規範一些方法（`add`、`remove`、`action`、`display`），讓其子類別必須實作。

```Java
/** 火箭隊 **/
public abstract class Rocket {
    /** 名稱 **/
    protected String name;
    /** 目標 **/
    protected String target;
    public Rocket(String name, String target) {
        this.name = name;
        this.target = target;
    }
    /** 加入組織 **/
    public abstract void add(Rocket rocket);
    /** 退出組織 **/
    public abstract void remove(Rocket rocket);
    /** 行動 **/
    public abstract void action();
    /** 顯示組織架構圖 **/
    public abstract void display(int level);
}
```

Organization（組織）繼承 Rocket 類別。Organization 可以招募以及汰除成員，所以擁有一個 List 集合。可以看到 `action` 方法，先完成組織必須要做的事情後，其組織下的所有小隊也使用迴圈來完成行動。

```Java
/** 組織 **/
public class Organization extends Rocket {
    private List<Rocket> group = new ArrayList();
    public Organization(String name, String target) { super(name, target); }
    @Override
    public void add(Rocket rocket) { group.add(rocket); }
    @Override
    public void remove(Rocket rocket) { group.remove(rocket); }
    @Override
    public void action() {
        System.out.printf("%s準備大型機具捕捉%s!%n", name, target);
        for(Rocket rocket: group) rocket.action();
    }
    @Override
    public void display(int level) {
        for (int i = 0; i < level; i++)
            System.out.printf("-");
        System.out.println(name);
        level++;
        for (Rocket rocket: group) rocket.display(level);
    }
}
```

Team（小隊）也繼承了 Rocket 類別，是火箭隊裡的最小單位。因為他們沒辦法招募與汰除其他成員，所以實作 `add`、`remove` 方法將會顯示請聯絡組織等字樣。

```Java
/** 小隊 **/
public class Team extends Rocket {
    public Team(String name, String target) { super(name, target); }
    @Override
    public void add(Rocket rocket) { System.out.println("請聯絡組織"); }
    @Override
    public void remove(Rocket rocket) { System.out.println("請聯絡組織"); }
    @Override
    public void action() { System.out.printf("%s出動抓捕%s!%n", name, target); }
    @Override
    public void display(int level) {
        for (int i = 0; i < level; i++)
            System.out.printf("-");
        System.out.println(name);
    }
}
```

### Sample：

在這裡有五個角色，boss（火箭隊老大）其底下有個得力助手 assistant，並且在各地擁有組織分部，像是 pewter（深灰分部）。每個分部都可以招募成員，深灰分部也招募到了菜鳥隊與三人小組這兩個小隊。從程式碼中可看出，boss 只要下令行動，就可以讓所有角色都行動。不需要考慮是單體還是全體，擁有一個一致性的操作介面，這就是合成模式的特色。

```Java
/** 合成模式 - 示範 **/
public class CompositeDemo {
    public static void main(String[] args) {
        /***************************角色設定並組織成員***************************/
        /** 火箭隊老大 **/
        Rocket boss = new Organization("坂木", "超夢");
        /** 老大旁的左右手 **/
        Rocket assistant = new Team("真島", "火焰鳥");
        /** 火箭隊深灰分部 **/
        Rocket pewter = new Organization("深灰分部", "急凍鳥");
        /** 深灰分部-菜鳥隊 **/
        Rocket newcomer = new Team("菜鳥隊", "綠毛蟲");
        /** 三人小組-武藏、小次郎、喵喵 **/
        Rocket triple = new Team("武藏、小次郎、喵喵", "皮卡丘");
        boss.add(assistant);
        boss.add(pewter);
        pewter.add(newcomer);
        pewter.add(triple);
        System.out.println("\n開始行動");
        boss.action();
        System.out.println("\n顯示組織架構");
        boss.display(0);
        System.out.println("\n菜鳥隊考績不合格，被深灰分部汰除");
        pewter.remove(newcomer);
        System.out.println("\n老大再次下令行動");
        boss.action();
    }
}
```

![Composite Result](/1_Basic/Design Pattern/Image/Composite_R.png "Composite Result")
