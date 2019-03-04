# 抽象工廠模式：均衡的訓練家

### Introduce：
由一個介面規範建立一系列的類別，客戶端只需建立實作該介面的工廠類別即可使用一系列的類別。有以下特點：

* 客戶端只要更換工廠類別，即可替換一系列物件。
* 如果有新的類別加入，介面及每個實作工廠都必須修改以增加創建該類別的方法。

### Main：
在這個寶可夢的世界裡，有各式各樣不同屬性的寶可夢。如果都只訓練同一種屬性，那對戰的時候容易遭遇到逆風。如小霞為水系寶可夢訓練家，她的寶可夢海星星、蚊香蝌蚪、可達鴨均是水屬性寶可夢。所以玩家只要把皮卡丘或者電系寶可夢練強一點，一隻就可以打敗小霞，挑戰成功。小霞並沒有可以剋制皮卡丘屬性的寶可夢（如岩系寶可夢-小拳石）。這就是單一屬性寶可夢訓練家會遇到的危機。為了避免這種情況，訓練的寶可夢必須多種屬性，避免單一屬性被剋制。

寶可夢每隻的能力也不相同，攻擊力高的通常防禦力較低，反之亦然。劇情玩到後面會發現，擁有多種屬性寶可夢的訓練家越來越多，更可以發現，有些訓練家比較傾向攻擊，會選擇攻擊型的寶可夢來擊潰對手。有些訓練家比較保守，則會選擇防禦型的寶可夢來採取穩紮穩打。這邊就可以用**抽象工廠模式**來表達。

### UML：

最上面為 Pokemon（寶可夢）類別，是個抽象類別，用以保存一些寶可夢共同屬性以及規範一些方法。其子類別為 FirePokemon（火屬寶可夢）、WaterPokemon（水屬寶可夢），而各自的子類別代表著每個屬性都有攻擊型與防禦型。

介面 Trainer（訓練家），是個抽象工廠，規範派出不同屬性的寶可夢來對戰。實作這個介面的有 AggressiveTrainer（攻擊型訓練家）、DefensiveTrainer（防禦型訓練家）。他們是具體工廠類別，實作的每個方法將會返回不同屬性不同類型的寶可夢。

![Abstract Factory UML](/1_Basic/Design Pattern/Image/Abstract_Factory_J.png "Abstract Factory UML")

### Code：

Pokemon（寶可夢），抽象類別，有 `name` （寶可夢名稱）屬性。其子類別必須實作 `getProperty` 與 `getType` 方法。
```Java
/** 寶可夢 **/
public abstract class Pokemon {
    /** 寶可夢名稱 **/
    protected String name;
    protected String getName() { return this.name; }
    protected void setName(String name) { this.name = name; }
    /** 寶可夢屬性 **/
    protected abstract String getProperty();
    /** 寶可夢類型 **/
    protected abstract String getType();
}
```

Pokemon 有兩個子類別，代表水屬性與火屬性寶可夢。
```Java
/** 水屬性寶可夢 **/
public abstract class WaterPokemon extends Pokemon {
    @Override
    protected String getProperty() { return "水屬性"; }
}
```
```Java
/** 火屬性寶可夢 **/
public abstract class FirePokemon extends Pokemon {
    @Override
    protected String getProperty() { return "火屬性"; }
}
```

每一種屬性寶可夢又可以分為兩類：攻擊型、防禦型。
```Java
/** 水屬性攻擊型寶可夢 **/
public class WaterAggressivePokemon extends WaterPokemon {
    @Override
    protected String getType() { return "攻擊型"; }
}
```
```Java
/** 水屬性防禦型寶可夢 **/
public class WaterDefensivePokemon extends WaterPokemon {
    @Override
    protected String getType() { return "防禦型"; }
}
```
```Java
/** 火屬性攻擊型寶可夢 **/
public class FireAggressivePokemon extends FirePokemon {
    @Override
    protected String getType() { return "攻擊型"; }
}
```
```Java
/** 火屬性防禦型寶可夢 **/
public class FireDefensivePokemon extends FirePokemon {
    @Override
    protected String getType() { return "防禦型"; }
}
```

Trainer（訓練家）是個抽象工廠，規範訓練家必須派出水屬寶可夢與火屬寶可夢。

```Java
/** 訓練家 **/
public interface Trainer {
    /** 派出水系寶可夢 **/
    WaterPokemon assignWaterPokemon();
    /** 派出火系寶可夢 **/
    FirePokemon assignFirePokemon();
}
```

實作 Trainer 介面的的兩個實例工廠，代表著攻擊型的訓練家與防禦型的訓練家。他們必須實作訓練家的方法派出水屬性與火屬性寶可夢，這些寶可夢將會符合訓練家的類型。

```Java
/** 攻擊型訓練家 **/
public class AggressiveTrainer implements Trainer {
    @Override // 返回水屬性攻擊型的寶可夢
    public WaterPokemon assignWaterPokemon() { return new WaterAggressivePokemon(); }
    @Override // 返回火屬性攻擊型的寶可夢
    public FirePokemon assignFirePokemon() { return new FireAggressivePokemon(); }
}
```
```Java
/** 防禦型訓練家 **/
public class DefensiveTrainer implements Trainer {
    @Override // 返回水屬性防禦型的寶可夢
    public WaterPokemon assignWaterPokemon() { return new WaterDefensivePokemon(); }
    @Override // 返回火屬性防禦型的寶可夢
    public FirePokemon assignFirePokemon() { return new FireDefensivePokemon(); }
}
```

### Sample：
到後期玩家遇到的對手將會很棘手，因為其對手的寶可夢均衡且強大。

從下面的程式碼可發現，攻擊型訓練家要切換到防禦型訓練家，只要改變其訓練家具體工廠即可，並且封裝了攻擊型與防禦型寶可夢的資訊。這就是抽象工廠模式的特色。

```Java
/** 抽象工廠模式-示範 **/
public class AbstractFactoryDemo {
    public static void main(String[] args) {
        /** 遇到攻擊型的訓練家要求對戰 **/
        Trainer trainer = new AggressiveTrainer();
        System.out.println("Round1 - 攻擊型訓練家派出寶可夢");
        battle(trainer.assignFirePokemon(), "噴火龍");
        battle(trainer.assignWaterPokemon(), "毒刺水母");
        /** 遇到防禦型的訓練家要求對戰 **/
        trainer = new DefensiveTrainer();
        System.out.println("Round2 - 防禦型訓練家派出寶可夢");
        battle(trainer.assignFirePokemon(), "煤炭龜");
        battle(trainer.assignWaterPokemon(), "水箭龜");
    }
    /** 戰鬥 **/
    private static void battle(Pokemon pokemon, String name) {
        pokemon.setName(name);
        System.out.println(pokemon.getName() + "-" + pokemon.getProperty() + pokemon.getType());
    }
}
```
![Abstract Factory Result](/1_Basic/Design Pattern/Image/Abstract_Factory_R.png "Abstract Factory Result")
