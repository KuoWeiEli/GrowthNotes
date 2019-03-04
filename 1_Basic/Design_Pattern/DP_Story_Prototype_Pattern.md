# 原型模式：喬伊小姐

### Introduce：

通過複製來取得新的物件。有以下特點：

* 直接複製初始化過後的物件，可提升效能外，也隱藏建立的細節。
* 可以動態獲得物件的執行狀態。

### Main：

相信大家玩寶可夢的時候，每抵達一個城鎮，第一個會去的地方就是寶可夢中心，去那邊治療因戰鬥而受傷的寶可夢。一進到寶可夢中心後，眼前看見的是一個長形櫃台，櫃台那站著一位穿著護士袍，紅色捲髮的女士─喬伊小姐。玩家只要詢問她，她就會幫忙把寶可夢送去治療。不過，玩家們一定很困惑，為什麼每個寶可夢中心的護士都叫喬伊小姐，而且長得一模一樣？如果喬伊小姐是複製人的話...，這裡就可以使用**原型模式**來解釋XD！

### UML：

Nurse（護士）擁有一個 Location（工作地點）類別，並且它們都實作 Cloneable 介面，需要實作 `clone( )`方法。

![Prototype UML](/1_Basic/Design_Pattern/Image/Prototype_J.png "Prototype UML")

### Code：

Location（地點）類別，紀錄工作地點的相關資訊，這裡只記錄工作地點的名稱。

```Java
/** 地點 **/
public class Location implements Cloneable {
    /** 名稱 **/
    private String name;
    public void setName(String name) {
        this.name = name;
    }
    public Location(String name) {
        this.name = name;
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    @Override
    public String toString() {
        return this.name;
    }
}
```

Nurse（護士）類別，擁有 `name`（姓名）、`features`（特徵）屬性及 `workLocation`（工作地點）類別。特別注意的是在 `clone( )`方法中，先使用 `super.clone( )`方法複製出一個 Nurse 類別，再複製一個 Location 類別，千萬不要使用 `this.clone( )`造成自身呼叫產生迴圈錯誤。

```Java
/** 護士 **/
public class Nurse implements Cloneable {
    /** 姓名 **/
    private String name;
    /** 特徵 **/
    private String features;
    /** 工作地點 **/
    private Location workLocation;
    public Nurse(String name, String features) {
        this.name = name;
        this.features = features;
    }
    /** 設置工作地點 **/
    public void setWorkLocation(Location location) { this.workLocation = location; }
    /** 設定工作地點名稱 **/
    public void setWorkLocationName(String name) {
        this.workLocation.setName(name);
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        /** 先複製nurse所擁有基本型別的資料 **/
        Nurse nurse = (Nurse) super.clone();
        /** 參考型別的資料必須額外複製 **/
        nurse.workLocation = (Location) workLocation.clone();
        return nurse;
    }
    @Override
    public String toString() {
        return String.format("我是%s的護士%s，請問需要幫助嗎?", this.workLocation, this.name);
    }
}
```

### Sample：

玩家依序抵達不同的城鎮，來到健康中心詢問喬伊小姐。

可以看見第二個城鎮以後的喬伊小姐，都是用 Nurse 的 `clone( )`方法來創造，並修改工作地點的名稱而已。不需要像第一個 Nurse 使用建構子產生相關的資訊。原型模式隱藏了物件建立的細節，且對性能來說也相對提高不少，這就是原型模式的特色。

```Java
/** 原型模式-示範 **/
public class PrototypeDemo {
    public static void main(String[] args) throws CloneNotSupportedException {
        /** 玩家每抵達一個城鎮，都會去寶可夢中心拜託喬伊治療寶可夢 **/
        Nurse nurse = new Nurse("喬伊", "紅色捲髮");
        /** 到了第一個城鎮 - 深灰市 **/
        nurse.setWorkLocation(new Location("深灰市"));
        /** 到了第二個城鎮 - 華藍市 **/
        Nurse nurse2 = (Nurse) nurse.clone();
        nurse2.setWorkLocationName("華蘭市");
        /** 到了第三個城鎮 - 枯葉市 **/
        Nurse nurse3 = (Nurse) nurse2.clone();
        nurse3.setWorkLocationName("枯葉市");
        /** 看看三個喬伊小姐的開場白 **/
        System.out.println(nurse);
        System.out.println(nurse2);
        System.out.println(nurse3);
    }
}
```

![Prototype Result](/1_Basic/Design_Pattern/Image/Prototype_R.png "Prototype Result")
