# 橋梁模式：寶可夢進化系統

### Introduce：

分割類別自身的業務邏輯，並將其成為抽象。有以下特點：
* 業務邏輯能夠獨立出來，減少耦合。
* 類別依賴業務邏輯抽象類別，更換業務邏輯實作即可改變類別的實現。

### Main：

經過一段時間後，玩家收服的寶可夢達到一定的條件後，將會「進化」。進化將改變寶可夢的外觀，能力等等，通俗點就是寶可夢「登大人」了！不過登大人的方式有很多種，有些人用中藥，有些人...。寶可夢的進化也有很多種，如果寶可夢是個類別，而每個進化方式都繼承寶可夢類別，這樣的設計將會有過多的耦合。寶可夢也有分普通和稀有，這樣進化方式也要繼承稀有寶可夢類別，將使得繼承體系越來越笨重，並不是一個樂見的結果。這個時候，**橋梁模式**就可以在這邊派上用場了！

### UML：

NormalPokemon（普通寶可夢）繼承 Pokemon（寶可夢）。Pokemon 有一個 Evolution（進化方式）屬性，Evolution 是個介面，實例有 NaturalEvolution（自然進化）、StoneEvolution（石頭進化）、ExchangeEvolution（交換進化）

![Bridge UML](/1_Basic/Design_Pattern/Image/Bridge_J.png "Bridge UML")

### Code：

Pokemon（寶可夢）是個抽象類別，有著 `name`（寶可夢名稱）、以及 `evolution`（進化方式）屬性。除了基本的 `get`、`set` 方法設置屬性外，還可以注意到有一個 `evolve` 方法處理寶可夢的進化。其邏輯很簡單，當屬性 `evolution` 有設置時，就調用 `evolution.evolve` 方法，並將自己 `this` 作為參數傳遞。

```Java
/** 寶可夢 **/
public abstract class Pokemon {
    private String name;
    private Evolution evolution;

    public Pokemon(String name) {
        this.name = name;
    }

    /** 進化邏輯 **/
    public void evolve() {
        if (evolution == null) return;
        evolution.evolve(this);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Evolution getEvolution() {
        return evolution;
    }

    public void setEvolution(Evolution evolution) {
        this.evolution = evolution;
    }
}
```

NormalPokemon（普通寶可夢），繼承 Pokemon 類別。這裡可以實作一些普通寶可夢的邏輯。

```Java
/** 普通寶可夢 **/
public class NormalPokemon extends Pokemon {
    public NormalPokemon(String name) {
        super(name);
        // 設置一些普通寶可夢的邏輯
    }
}
```

Evolution（進化方式）是一個介面，規範其實例必須實作 `evolve` 寶可夢進化的邏輯。

```Java
/** 進化方式 **/
public interface Evolution {
    void evolve(Pokemon pokemon);
}
```

NaturalEvolution（自然進化）、StoneEvolution（石頭進化）、ExchangeEvolution（交換進化）皆是 Evolution 的實例。並且實作 `evolve` 方法。該方法接受一個 `Pokemon` 類別，並且使用其 `getName` 方法取得寶可夢的名稱。

```Java
/** 寶可夢達到一定等級後自然進化 **/
public class NaturalEvolution implements Evolution {
    @Override
    public void evolve(Pokemon pokemon) {
        System.out.printf("%s達到一定等級%n", pokemon.getName());
    }
}
```
```Java
/** 使用特殊石頭來進化寶可夢 **/
public class StoneEvolution implements Evolution {
    @Override
    public void evolve(Pokemon pokemon) {
        System.out.printf("對%s使用石頭%n", pokemon.getName());
    }
}
```
```Java
/** 透過與其他玩家交換寶可夢，以讓交換的寶可夢進化 **/
public class ExchangeEvolution implements Evolution {
    @Override
    public void evolve(Pokemon pokemon) {
        System.out.printf("與其他玩家交換%s%n", pokemon.getName());
    }
}
```

### Sample：
現在玩家的寶可夢隨著時間的過去，一一進化了。

從下面的程式碼可以看出來，寶可夢需要透過什麼樣的進化方式進化，只要使用 `Pokemon.setEvolution` 方法設置即可。進化方式可以說是寶可夢的業務邏輯，透過將進化方式抽象出來成為介面 Evolution 後，現在各個進化方式可以獨立發展，不再與 Pokemon 有過多的耦合，這就是橋梁模式的特色。


```Java
/** 橋梁模式 - 示範 **/
public class BridgeDemo {
    public static void main(String[] args) {
        Pokemon pokemon1 = new NormalPokemon("皮卡丘");
        pokemon1.setEvolution(new StoneEvolution());
        pokemon1.evolve();
        System.out.println("=> 皮卡丘透過電氣石進化成雷丘！");

        Pokemon pokemon2 = new NormalPokemon("傑尼龜");
        pokemon2.setEvolution(new NaturalEvolution());
        pokemon2.evolve();
        System.out.println("=> 傑尼龜自然進化成卡咪龜！");

        Pokemon pokemon3 = new NormalPokemon("豪力");
        pokemon3.setEvolution(new ExchangeEvolution());
        pokemon3.evolve();
        System.out.println("=> 豪力透過交換，進化成怪力！");
    }
}
```

![Bridge Result](/1_Basic/Design_Pattern/Image/Bridge_R.png "Bridge Result")

