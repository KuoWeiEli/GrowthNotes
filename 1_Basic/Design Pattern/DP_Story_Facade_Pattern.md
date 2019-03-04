# 表象(外觀)模式：智能圖鑑

### Introduce：

提供一個介面來操作一系列的子系統。有以下特點：

* 藉由使用介面簡單的操作方法，來避免子系統的複雜邏輯。
* 因為使用介面來操作，所以只耦合介面，減少了子系統在各個地方的耦合。
* 在維護舊系統時，利用此模式可以簡單擴展和維護。

### Main：

還記得當初在大木博士研究所選擇御三家的時候嗎？在玩家選擇完寶可夢後，大木博士會給予一台寶可夢圖鑑。這個圖鑑可以幫玩家在遇到寶可夢的時候，提供寶可夢的詳細資訊，並記錄遇到寶可夢的位置。它不需要玩家在遇到寶可夢的時候，還需要自己拿起地圖，向聯盟尋求寶可夢資訊後，在地圖上畫上標記。也就是圖鑑統合了寶可夢、地圖、聯盟並且提供簡單的介面供玩家操作，這裡就是**表象模式**的應用！

### UML：

Pokedex（圖鑑）是一個介面，規範了圖鑑應該為玩家提供的功能。而實作這個介面的為 BasicPokedex（基本圖鑑），在一開始就擁有了KantoMap（關都地圖）、Pokemon（寶可夢）、LeagueCenter（聯盟中心）這些類別，並使用這些類別的方法來實作 Pokedex。Map（地圖）也是個介面，規範地圖應該提供的功能，實作的子介面有 KantoMap、JohtoMap（城都地圖）。

![Facade UML](/1_Basic/Design Pattern/Image/Facade_J.png "Facade UML")

### Code：

Map（地圖）是個介面，而實作的子介面有兩個，分別為 KantoMap（關都地圖）、JohtoMap（城都地圖）。均提供了 `showMap`、`markPosition`、`getRegionInfo` 等方法來操作地圖。

```Java
/** 地圖 **/
public interface Map {
    /** 顯示地圖 **/
    void showMap();
    /** 標記位置 **/
    void markPosition();
    /** 取得地區資訊 **/
    void getRegionInfo();
}
```
```Java
/** 關都地區的地圖 **/
public class KantoMap implements Map {
    @Override
    public void showMap() { System.out.println("顯示關都地區的地圖"); }
    @Override
    public void markPosition() { System.out.println("在關都地區標註位置"); }
    @Override
    public void getRegionInfo() { System.out.println("取得關都地區資料"); }
}
```
```Java
/** 城都地區地圖 **/
public class JohtoMap implements Map {
    @Override
    public void showMap() { System.out.println("顯示城都地區的地圖"); }
    @Override
    public void markPosition() { System.out.println("在城都地區標註位置"); }
    @Override
    public void getRegionInfo() { System.out.println("取得城都地區資料"); }
}
```

Pokemon（寶可夢）類別可以藉由 `getPokemon`  方法取得特定的寶可夢，也提供了 `showDetail` 方法來顯示寶可夢詳細資料。
```Java
/** 寶可夢 **/
public class Pokemon {
    public void getPokemon() { System.out.println("取得寶可夢"); }
    public void showDetail() { System.out.println("顯示寶可夢的詳細資料"); }
}
```

LeagueCenter（聯盟中心）先使用 `queryFromLibrary`方法來送出查詢的要求，再使用 `getPokemonDetail` 取得寶可夢詳細資料。也提供了 `getPokemonLocation` 方法取得寶可夢出現地區的資料。

```Java
/** 聯盟中心 **/
public class LeagueCenter {
    public void queryFromLibrary() { System.out.println("尋找寶可夢詳細資料"); }
    public void getPokemonDetail() { System.out.println("取得寶可夢詳細資料"); }
    public void getPokemonLocation() { System.out.println("取得寶可夢的出現的地區資料"); }
}
```

Pokedex（圖鑑介面）是個介面，規範圖鑑應有的操作功能 `showPokemonOnMap`、`requirePokemonDetail`。

```Java
/** 圖鑑介面，定義一些使用者可操作的動作 **/
public interface Pokedex {
    /** 在地圖上顯示寶可夢位置 **/
    void showPokemonOnMap();
    /** 查詢寶可夢詳細資訊 **/
    void requirePokemonDetail();
}
```

BasicPokedex（基本圖鑑）實作 Pokedex 介面。在一開始就初始化所擁有的 Map、Pokemon、LeagueCenter 類別，並且 Map 被指派為關都地區（玩家一開始所在的區域）類別。

```Java
/** 基本圖鑑 **/
public class BasicPokedex implements Pokedex {
    /** 地圖 **/
    private Map map = new KantoMap();
    /** 寶可夢 **/
    private Pokemon pokemon = new Pokemon();
    /** 聯盟中心 **/
    private LeagueCenter center = new LeagueCenter();
    public void setMap(Map map) { this.map = map; }
    public void setPokemon(Pokemon pokemon) { this.pokemon = pokemon; }
    public void setCenter(LeagueCenter center) { this.center = center; }
    @Override
    public void showPokemonOnMap() {
        /** 顯示地圖 **/
        map.showMap();
        /** 取得寶可夢 **/
        pokemon.getPokemon();
        /** 取得寶可夢的位置 **/
        center.getPokemonLocation();
        /** 標記在地圖上 **/
        map.markPosition();
    }
    @Override
    public void requirePokemonDetail() {
        /** 取得寶可夢 **/
        pokemon.getPokemon();
        /** 聯盟中心尋找資料，並返回查到的詳細資料 **/
        center.queryFromLibrary();
        center.getPokemonDetail();
        /** 查詢寶可夢的出現地方相關資訊 **/
        map.getRegionInfo();
        /** 最後顯示寶可夢的詳細資料 **/
        pokemon.showDetail();
    }
}
```

### Sample：

玩家從大木博士拿到圖鑑（BasicPokedex）後，就可以開始操作圖鑑。而破關後來到第二代地區-城都地區，只要設定圖鑑的地圖為城都地區的地圖（JohtoMap），就可以在城都地區繼續使用圖鑑功能。除了可以統合各個子系統（地圖、寶可夢、聯盟中心）並提供簡單的介面操作，如果有新的需求（到了新的地區需要新的地圖）只要替換掉子系統就可以了，這就是表象（外觀）模式的特色。

```Java
/** 表象 (外觀) 模式 - 示範 **/
public class FacadeDemo {
    public static void main(String[] args) {
        /** 玩家擁有一個圖鑑 **/
        Pokedex pokedex = new BasicPokedex();
        System.out.println("玩家打開地圖");
        pokedex.showPokemonOnMap();
        System.out.println();
        System.out.println("玩家想要看寶可夢詳細資料");
        pokedex.requirePokemonDetail();
        System.out.println();
        /** 玩家到了城都地區，將圖鑑的地圖替換為城都地區的地圖 **/
        ((BasicPokedex) pokedex).setMap(new JohtoMap());
        System.out.println("玩家打開地圖");
        pokedex.showPokemonOnMap();
    }
}
```

![Facade Result](/1_Basic/Design Pattern/Image/Facade_R.png "Facade Result")
