# 轉接器模式：橫渡海洋

### Introduce：

使用一介面讓兩不相干的介面相容，能夠一起使用。有以下特點：

* 能夠解決既有類別需要複用但是環境不符合的問題。
* 可使用在兩端邏輯需要介接的時候，減少彼此兩端的耦合。

### Main：

在船尚未發明以前，人們只能依靠寶可夢來橫渡海洋抵達下個城鎮。並不是所有寶可夢都可以橫渡海洋，只有水系寶可夢在學會衝浪這個技能後，才能夠在海洋上移動。玩家一開始很難遇到水系寶可夢，就算御三家選擇傑尼龜，如果你沒有秘傳機器也沒辦法讓傑尼龜學會衝浪技能。這時候玩家如果要橫渡海洋抵達下個城鎮，只能夠買貴森森的船票了。還好在劇情中，只要達成NPC的任務，就可以得到免費船票。但如果沒有遇到這個機會呢？就只能自己想辦法製作木筏囉！其實船和木筏就是**轉接器模式**的一種應用。它們都是透過觀察寶可夢在海上的一舉一動，將現有的材料改造並且不斷的實驗，進而完成這些航海工具。

### UML：

Surf（衝浪）是個介面，只要實作這個介面就可以自由自在地在海面上移動。現在可以看到，野生的 Squirtle（傑尼龜）實作這個介面，牠本身就可以在海面上衝浪。那沒有水系寶可夢也沒有船票怎麼辦？現在玩家就只有一個 Box（箱子），雖然人可以在箱子裡面，但箱子沒辦法在海面上浮起來。這時候就只好用轉接器 Raft（木筏）包裝一下 Box，因為 Raft 有實作 Surf 介面，所以它就可以在海面上移動了！

![Adapter UML](/1_Basic/Design_Pattern/Image/Adapter_J.png "Adapter UML")

### Code：

Surf（衝浪）介面，想要在海面上移動就得先實作這個介面！

```Java
/** 衝浪技能 **/
public interface Surf {
    /** 衝浪 **/
    void surf();
}
```

Squirtle（傑尼龜）實作 Surf 介面，所以牠可以在海面上移動。

```Java
/** 傑尼龜可以透過秘傳機器學習到衝浪技能 **/
public class Squirtle implements Surf {
    @Override
    public void surf() {
        System.out.printf("%s悠游在海面上~~%n", this.getClass().getSimpleName());
    }
}
```

Box（箱子），現在玩家所擁有的東西，它提供 `load` 方法讓玩家可以坐在裡面。

```Java
/** 箱子 **/
public class Box {
    /** 擁有裝載功能，可以讓人坐在裡面 **/
    public void load(String name) {
        System.out.printf("%s坐在箱子裡面!%n", name);
    }
}
```

Raft（木筏），可以把箱子放置在上面。因為有實作 Surf 介面，所以它能夠在海面上移動！可看見在 `surf` 方法上，經過一些特殊的處理後，現在箱子已經被裝在木筏上。玩家可以坐在箱子裡面，操作划槳在海面上航行。

```Java
/** 木筏 **/
public class Raft implements Surf {
    /** 箱子 **/
    private Box box;
    /** 乘客 **/
    private String name;
    /** 將箱子準備好，乘客準備坐上去 **/
    public Raft(Box box, String name) {this.box = box; this.name = name; }
    @Override
    public void surf() {
        System.out.println("Step1: 將木頭排在一起並用繩子固定住~");
        System.out.println("Step2: 將箱子固定在木頭上面!");
        System.out.println("Step3: 準備兩隻划槳!");
        box.load(name);
        System.out.println("準備出發，使用划槳在海面上移動 !!!!");
    }
}
```

### Sample：

現在玩家就可以拿著剛做好的木筏，在海面上航行了！ 從這邊可看出，原本的物件（Box）無法在現有的介面（Surf）中使用。但可以藉由轉接器（Raft）來達成相容的目的，這就是轉接器模式的特性。

```Java
/** 轉接器模式 - 示範 **/
public class AdapterDemo {
    public static void main(String[] args) {
        System.out.println("玩家要到下一個城鎮必須橫渡這片海過去!");
        System.out.println("玩家開始製作木筏");
        Raft raft = new Raft(new Box(), "小智");
        raft.surf();
        System.out.println("在海面上看到傑尼龜一起同行!");
        Squirtle squirtle = new Squirtle();
        squirtle.surf();
    }
}
```

![Adapter Result](/1_Basic/Design_Pattern/Image/Adapter_R.png "Adapter Result")
