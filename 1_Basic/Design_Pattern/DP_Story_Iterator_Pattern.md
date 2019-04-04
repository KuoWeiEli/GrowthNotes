# 走訪器模式：寶可夢圖鑑
### Introduce：
提供一介面可遍歷聚合物件的任何成員。有以下特色：

* 遍歷相關邏輯與聚合物件是分離的。
* 聚合物件能夠被使用者操作介面簡單遍歷，又不會暴露本身的資訊。

### Main：
在遊玩寶可夢的過程中，筆者偶爾無聊的時候會開啟寶可夢圖鑑，來觀賞自己收服的寶可夢。這個圖鑑是一個列表，一開始指針會在列表最上面，指到第一個寶可夢並顯示其相關資訊。玩家可使用 `↑`、` ↓` 鍵來移動指針，查看別的寶可夢。這裡就可以使用**走訪器模式**來設計！

### UML：

玩家可以將寶可夢託付給大木博士來照顧，這些寶可夢將會被安置在大木博士研究所的花園 DrGarden 中。所以 DrGarden 屬於聚合物件，可以保存很多寶可夢，並繼承了 Aggregate（聚合物件）。要如何查看收服那些寶可夢呢？可以使用實作 Iterator（走訪器介面）的 Pokedex（寶可夢圖鑑）來查看。

![Iterator UML](/1_Basic/Design_Pattern/Image/Iterator_J.png "Iterator UML")

### Code：

我們先來看看聚合物件的父類別 Aggregate（聚合物件）。因為有「**聚合**」兩字，代表著此物件可以保存許多個體，所以有個 `add` 方法去規範子類別須實作物體可以加入到本身的方法。另外子類別也必須實作 `createIterator` 方法，取得一個可以遍歷本身的走訪器。

```Java
/** 聚合物件 **/
public abstract class Aggregate {
    /** 取得一個可遍歷此聚集物件的走訪器 **/
    public abstract Iterator createIterator();
    /** 寄存 **/
    public abstract void add(Object obj);
}
```

走訪器的相關邏輯會使用一個介面 Iterator（走訪器介面）去規範。這邊提供了三個方法在遍歷聚合物件時可以使用：
* `prev`：取上一個物件。
* `next`：取下一個物件。
* `isEnd`：是否已經是最後一個物件。

```Java
/** 走訪器介面 **/
public interface Iterator {
     Object prev();
     Object next();
     boolean isEnd();
}
```

剛剛說到託管的寶可夢會在哪裡？沒錯，就是在大木博士的花園裡 DrGarden。它繼承了 Aggregate，所以須實作會返回一個 Iterator 實例的 `createIterator` 方法。也須實作 `add` 方法，寶可夢將可以透過此方法保存在成員 `pokemons` 中。透過變數 `position` 可以知道目前花園中有多少寶可夢在活動。如果位置不夠了，就會增大花園。

```Java
/** 大木博士的花園 **/
public class DrGarden extends Aggregate {
    /** 託管的寶可夢們 **/
    private Object[] pokemons;
    /** 寶可夢活動的位置 **/
    private int position = 0;

    public DrGarden() {
        /** 大木博士的花園裡目前可以讓六個寶可夢活動 **/
        pokemons = new Object[6];
    }

    @Override
    public Iterator createIterator() {
        return new Pokedex(pokemons);
    }

    @Override
    public void add(Object pokemon) {
        /** 如果花園裡已塞滿了寶可夢，那就要擴建花園以擁有更多的位置 **/
        if (position >= pokemons.length) {
            pokemons = Arrays.copyOf(pokemons, pokemons.length + 1);
        }
        pokemons[position++] = pokemon;
    }
}
```

最後，Pokedex（寶可夢圖鑑）實作 Iterator 介面的方法 `prev`、`next`、`isEnd`，以操作 Object 陣列 `pokemons`。它是由建構子傳進來的參數 `pokemonsInGarden` 給設定的。 

```Java
/** 寶可夢圖鑑 **/
public class Pokedex implements Iterator {
    /** 在大木博士花園裡快活的寶可夢們 **/
    private Object[] pokemons;
    /** 使用者目前所瀏覽的位置 **/
    private int index = 0;

    public Pokedex(Object[] pokemonsInGarden) {
        pokemons = pokemonsInGarden;
    }

    @Override
    public Object prev() {
        try {
            return pokemons[--index];
        } catch (IndexOutOfBoundsException e) {
            System.out.println("到頂了！無寶可夢可選擇查看！");
            ++index;
        }
        return null;
    }

    @Override
    public Object next() {
        try {
            return pokemons[index++];
        } catch (IndexOutOfBoundsException e) {
            System.out.println("到底了！無寶可夢可選擇查看！");
            --index;
        }
        return null;
    }

    @Override
    public boolean isEnd() {
        return index >= pokemons.length-1;
    }
}
```

### Sample：
接下來就來演示走訪器模式的應用─操作寶可夢圖鑑！

在下列程式碼中可以看見，想要知道花園中有那些寶可夢，不需要親自操作存放那些寶可夢的陣列。只需要藉由走訪器 pokedex 提供的操作，就可以知道陣列裡有哪些寶可夢。這就是走訪器模式的特色！

```Java
/** 走訪器模式 - 示範 **/
public class IteratorDemo {
    public static void main(String[] args) {
        /** 玩家將收服的寶可夢交給大木博士照顧 **/
        Aggregate drGarden = new DrGarden();
        drGarden.add("綠毛蟲");
        drGarden.add("傑尼龜");
        drGarden.add("皮卡丘");
        drGarden.add("小拳石");
        drGarden.add("走路草");
        drGarden.add("鯉魚王");
        drGarden.add("小拉達");

        /** 開啟寶可夢圖鑑，查看被照顧的寶可夢 **/
        Iterator pokedex = drGarden.createIterator();
        while (!pokedex.isEnd()) {
            System.out.println(pokedex.next());
        }
        System.out.println("圖鑑已預覽到底！再往上預覽寶可夢");
        System.out.println(pokedex.prev());
        System.out.println(pokedex.prev());
        System.out.println(pokedex.prev());
        System.out.println(pokedex.prev());
    }
}
```

![Iterator Result](/1_Basic/Design_Pattern/Image/Iterator_R.png "Iterator Result")
