# 樣板模式：
### Introduce：
父類別定義邏輯的架構，子類別負責實現邏輯的細節。有以下特色：

* 父類別實現架構可避免子類別有重複代碼出現。
* 架構的邏輯因為只呈現在父類別上，所以其輪廓清楚可見。

### Main：
寶可夢的世界裡，有各式各樣不同風貌的地形。遊戲設計在不同的地形所呈現的風景、音樂、及遇到的寶可夢皆不一樣。比如草原讓人想到生氣蓬勃的樣子，所以會有非常輕快活潑的背景音樂，而遇到的寶可夢就會是跟草原有相關的。

那麼，難道每個地形都要單獨設計嗎？有沒有什麼方法可以節省力氣？這時候**樣板模式**跳出來說：「交給我吧！」。

### UML：

我們將所有的地形抽象出一個 Landform（地形）類別。這裡地形就以 Grass（草原）、Cave（洞穴）為例子。

![Template UML](/1_Basic/Design_Pattern/Image/Template_J.png "Template UML")

### Code：

Landform（地形）是個抽象類別，他規範了其子類別必須實作 `loadScene`（載入地形）、`loadMusic`（載入背景音樂）、`loadPokemons`（載入當地寶可夢）等方法。

可以注意到的是，方法 `entry` （進入該地形）分別呼叫三個抽象方法，定義了基本的架構，而實作細節則交給子類別去完成，這就是樣板模式的特色。

```Java
/** 地形 **/
public abstract class Landform {
    /** 載入地形（如草原、洞穴...） **/
    protected abstract void loadScene();
    /** 載入背景音樂 **/
    protected abstract void loadMusic();
    /** 載入該地形會出現的野生寶可夢 **/
    protected abstract void loadPokemons();

    /** 進入該地形 **/
    public void entry() {
        loadScene();
        loadMusic();
        loadPokemons();
    }
}
```

Grass（草原）、Cave（洞穴）均繼承了 Landform，並且實作父類別的抽象方法。

```Java
/** 草原 **/
public class Grass extends Landform {
    @Override
    protected void loadScene() {
        System.out.println("載入草原地形");
    }

    @Override
    protected void loadMusic() {
        System.out.println("載入輕快悠然的背景音樂");
    }

    @Override
    protected void loadPokemons() {
        System.out.println("載入草原上的野生寶可夢");
    }
}
```
```Java
/** 洞穴 **/
public class Cave extends Landform {
    @Override
    protected void loadScene() {
        System.out.println("載入洞穴地形");
    }

    @Override
    protected void loadMusic() {
        System.out.println("載入沉悶詭譎的背景音樂");
    }

    @Override
    protected void loadPokemons() {
        System.out.println("載入洞穴中的野生寶可夢");
    }
}
```

### Sample：
玩家依序抵達了草原與洞穴：

```Java
/** 樣板模式 - 示範 **/
public class TemplatePattern {
    public static void main(String[] args) {
        System.out.println("==> 玩家準備進入草原");
        Landform landform = new Grass();
        landform.entry();

        System.out.println("==> 玩家看見了一個洞穴，準備進入探險");
        landform = new Cave();
        landform.entry();
    }
}
```
![Template Result](/1_Basic/Design_Pattern/Image/Template_R.png "Template Result")
