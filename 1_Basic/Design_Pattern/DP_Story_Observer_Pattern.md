# 觀察者（發佈-訂閱）模式：玩家福利
### Introduce：
當一個物件改變其狀態時（又稱發佈者），與此物件相關的其他物件（又稱訂閱者）會視需要自動更新自己。有以下特色：

* 發佈者與訂閱者彼此解除耦合，各自獨立
* 發佈者不需要知道訂閱者任何細節。

### Main：

以前的寶可夢遊戲因為是單機版，所以沒有〝玩家福利〞的概念。現在大部分的手遊、線上遊戲，都會定期發放福利，以拉攏玩家繼續支持。玩家們又分了好幾個級別，免費遊玩的都是普通玩家，有包月花錢的稱為 VIP 玩家，還有更多的級別就不枚舉。因為級別的不同，發放福利當然也不會一樣。大部分 VIP 玩家會有額外的福利。寶可夢的其中一款 Pokemon GO 不是單機遊戲，但因為筆者遊玩時間不長，所以不知道是否有發放福利。但如果有發放福利，這裡就可以使用**觀察者模式**。 

### UML：
Center（中心）是個抽象類別，其子類別為 LeagueCenter（寶可夢聯盟中心），他們負責處理一些有關 Member（會員）的邏輯。Member 也是個抽象類別，其子類別有兩個，分別為 NormalPokemonMember（普通寶可夢玩家）、VipPokemonMember（Vip 寶可夢玩家），他們處理一些玩家的業務邏輯，以及關注  Center 的動態。

![Observer UML](/1_Basic/Design_Pattern/Image/Observer_J.png "Observer UML")

### Code：

Center（中心）是個抽象類別，除了提供招收 `add` 與剔除 `remove` 會員的方法外，也提供了發放予取得福利的相關方法 `setBonus`、`getBonus`。以及發放福利就要通知會員來領取的方法 `notifyAllMembers`;
```Java
/** 中心 **/
public abstract class Center {
    /** 招收會員 **/
    public abstract void add(Member m);
    /** 剔除會員 **/
    public abstract void remove(Member m);
    /** 發放/取得福利 **/
    public abstract void setBonus(int count);
    public abstract int getBonus();
    /** 公告所有會員 **/
    public abstract void notifyAllMembers();
}
```

LeagueCenter（寶可夢聯盟中心）繼承 Center 並實作其方法。可以看見多了一個 `members` 的集合，負責管理加入的會員。其屬性 `bonus` 為發放福利的數量。其中 `setBonus` 方法代表發放福利後，即使用 `notifyAllMembers` 方法通知所有會員來領取福利。

```Java
/** 寶可夢聯盟中心 **/
public class LeagueCenter extends Center {
    private List<Member> members = new ArrayList();
    private int bonus;

    @Override
    public void add(Member m) {
        members.add(m);
    }

    @Override
    public void remove(Member m) {
        members.remove(m);
    }

    @Override
    public void setBonus(int count) {
        this.bonus = count;
        notifyAllMembers();
    }

    @Override
    public int getBonus() {
        return this.bonus;
    }

    @Override
    public void notifyAllMembers() {
        for (Member m: members)
            m.receive();
    }
}
```

Member（會員）是個抽象類別，有個屬性 `center` 代表會員隸屬的中心，以及 `name` 代表會員的名稱。可以注意到，建構子在設置 `center`、`name` 的屬性後，及調用設置的 `center` 其 `add` 方法，以自己 `this` 作為參數傳入。

`receive` 方法是個抽象方法，其子類別必須實作此方法來處理接受中心公告的動作。

```Java
/** 會員 **/
public abstract class Member {
    /** 會員所屬中心 **/
    protected Center center;
    protected String name;

    public Member(Center center, String name) {
        this.name = name;
        this.center = center;
        this.center.add(this);
    }

    /** 接受公告 **/
    public abstract void receive();
}
```

NormalPokemonMember（普通寶可夢玩家）與 VipPokemonMember（VIP 寶可夢玩家）都繼承了 Member 並實作 `receive` 方法。其中 VipPokemonMember 是 VIP 玩家，所以領取的福利是兩倍，可以在 `receive` 方法中看見 bonus 得到兩倍。

```Java
/** 普通寶可夢玩家 **/
public class NormalPokemonMember extends Member {
    public NormalPokemonMember(Center center, String name) {
        super(center, name);
    }

    @Override
    public void receive() {
        System.out.printf("普通玩家%s取得中心發放的%s個寶貝球%n", this.name, this.center.getBonus());
    }
}
```
```Java
/** VIP 寶可夢玩家，取得中心的資源是普通寶可夢玩家的兩倍 **/
public class VipPokemonMember extends Member {
    public VipPokemonMember(Center center, String name) {
        super(center, name);
    }
    @Override
    public void receive() {
        System.out.printf("VIP玩家%s取得中心發放的%s個寶貝球%n", this.name, this.center.getBonus() * 2);
    }
}

```

從上述的程式碼可見，發佈者（Center）只提供加入與剔除訂閱者（Member）的功能，訂閱者怎麼實作邏輯完全不甘發佈者的事。發佈者只要更新狀態，訂閱者隨之就會接受並更新自己，這就是觀察者模式的特色。

### Sample：
PokemonGO 要發放福利給玩家了！

```Java
/** 觀察者模式 - 示範  **/
public class ObserverDemo {
    public static void main(String[] args) {
        Center center = new LeagueCenter();

        /** 一個普通玩家，一個VIP玩家 **/
        new NormalPokemonMember(center, "小明");
        new VipPokemonMember(center, "小華");

        /** 中心發福利給每位會員 **/
        center.setBonus(10);
    }
}
```
![Observer Result](/1_Basic/Design_Pattern/Image/Observer_R.png "Observer Result")
