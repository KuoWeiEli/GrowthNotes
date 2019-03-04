# 代理模式：健康中心的電腦

### Introduce：

通過保有目標物件屬性，代理執行該物件。有以下特點：

* 隔離不相干的邏輯，使目標物件專注自己本身。
* 隱藏目標物件細節（空間位置資訊、業務邏輯等等）。
* 提供目標物件加載時間（代理物件先讓 Html 出現縮圖，目標物件仍加載實際圖片）。
* 對物件進行授權驗證控制。

### Main：

玩家告別大木博士後，歷經了一連串的冒險，也收服了一隻隻的寶可夢。不過玩家被規定手上只能擁有六個寶可夢，那剩下的寶可夢該怎麼辦？超過六個寶可夢就無法再收服新的嗎？玩家不需要擔心這些問題，當身上擁有六個寶可夢後，在收服新的寶可夢時，牠將會被傳送到健康中心的電腦裡。玩家只要到健康中心的電腦前，就可以操作此電腦管理自己的寶可夢。

「蛤？電腦可以管理寶可夢？」

玩家心中一定會有這樣的疑問。其實幫玩家管理寶可夢的是大木博士，電腦只是負責處理你的請求，將你的請求傳送給大木博士，並記錄這些動作。可以說電腦代理了大木博士處理你的請求，而不是玩家直接接觸大木博士。這裡可以用**代理模式**來解釋。

### UML：

這邊將大木博士以保管員代稱。一個保管員必須能夠處理**領取**、**保管**、**放生**寶可夢等請求，因此有了 Caretaker（保管員）介面來規範處理操作。而　PokemonCaretaker（寶可夢保管員）、Computer（電腦）均實作了 Caretaker 介面，擁有可以管理 Pokemon（寶可夢）的能力。

![Proxy UML](/1_Basic/Design_Pattern/Image/Proxy_J.png "Proxy UML")

### Code：

Pokemon（寶可夢），擁有 `name` 屬性記錄寶可夢名稱。

```Java
/** 寶可夢 **/
public class Pokemon {
    /** 名稱 **/
    private String name;
    public Pokemon(String name) { this.name = name; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

Caretaker（保管人）介面，規範一個保管員須要有 `get` 、`put`、`remove` 處理操作。

```Java
/** 保管人 **/
public interface Caretaker {
    /** 領回寶可夢 **/
    Pokemon get(Pokemon pokemon);
    /** 保管寶可夢 **/
    void put(Pokemon pokemon);
    /** 放生寶可夢 **/
    void remove(Pokemon pokemon);
}
```

PokemonCaretaker（寶可夢保管員）實作 Caretaker 介面。

```Java
/** 寶可夢保管員 **/
public class PokemonCaretaker implements Caretaker {
    private List<Pokemon> pokemons = new ArrayList();
    @Override
    public Pokemon get(Pokemon pokemon) {
        if (pokemons.contains(pokemon))
            return pokemons.get(pokemons.indexOf(pokemon));
        else return null;
    }
    @Override
    public void put(Pokemon pokemon) { pokemons.add(pokemon); }
    @Override
    public void remove(Pokemon pokemon) { pokemons.remove(pokemon); }
}
```

Computer（健康中心的電腦）也實作了 Caretaker，並且擁有 `PokemonCaretaker` 屬性。在實作 Caretaker 介面的方法時，除了操作 PokemonCaretaker 同名方法外，還加上了一些檢核判斷，並記錄相關訊息。如此一來可以讓 PokemonCaretaker 專心自己的職責，這些檢核與紀錄就交給 Computer 就行，達成了業務邏輯的隔離，這就是代理模式的特色。

```Java
/** 健康中心的電腦 **/
public class Computer implements Caretaker {
    /** 代理寶可夢管理員 **/
    private PokemonCaretaker caretaker = new PokemonCaretaker();
    @Override
    public Pokemon get(Pokemon pokemon) {
        Pokemon checkPokemon = caretaker.get(pokemon);
        if (checkPokemon != null) {
            System.out.printf("已成功取得%s寶可夢!%n", checkPokemon.getName());
            return checkPokemon;
        } else {
            System.out.printf("您並沒有將%s寶可夢交給管理員保管!%n", pokemon.getName());
            return null;
        }
    }
    @Override
    public void put(Pokemon pokemon) {
        if (pokemon == null) System.out.println("請選擇需要寄放的寶可夢!");
        else {
            caretaker.put(pokemon);
            System.out.printf("%s寶可夢已順利交給管理員!%n", pokemon.getName());
        }
    }
    @Override
    public void remove(Pokemon pokemon) {
        if (pokemon == null) System.out.println("請選擇需要放生的寶可夢!");
        else {
            caretaker.remove(pokemon);
            System.out.printf("%s寶可夢已順利放生!%n", pokemon.getName());
        }
    }
}
```

### Sample：

現在玩家到了健康中心，想要操作一下電腦管理自己的寶可夢。玩家只需要知道如何操作 Computer 就行，不需要與 PokemonCaretaker 類別打交道，隱藏了被代理的物件（PokemonCaretaker）。

```Java
/** 代理模式-示範 **/
public class ProxyDemo {
    public static void main(String[] args) {
        /** 到健康中心電腦前操作電腦 **/
        Computer computer = new Computer();
        /** 將綠毛蟲送去保管 **/
        Pokemon caterpie = new Pokemon("綠毛蟲");
        computer.put(caterpie);
        /** 操作不正確，沒有選擇寶可夢 **/
        computer.put(null);
        /** 將手上寶可夢都送去保管 **/
        Pokemon pikachu = new Pokemon("皮卡丘");
        computer.put(pikachu);
        computer.put(new Pokemon("傑尼龜"));
        computer.put(new Pokemon("小火龍"));
        computer.put(new Pokemon("妙蛙種子"));
        /** 綠毛蟲決定放生 **/
        computer.remove(caterpie);
        /** 將皮卡丘領出 **/
        computer.get(pikachu);
    }
}
```

![Proxy Result](/1_Basic/Design_Pattern/Image/Proxy_R.png "Proxy Result")
