# 工廠模式：挑戰道館

### Introduce：
由一個介面去定義工廠如何產生類別。客戶端將需要創建實作此介面的工廠，並藉由該工廠得到需要的類別。有以下特點：

* 客戶端不再只是單純傳入參數就可以決定產出類別，需要額外判斷產生需要的工廠以產生所要的類別。
* 增加新類別時，不需要修改原有的工廠類別，只需要額外添加新的工廠類別即可。

### Main：
要如何成為世界第一的寶可夢訓練家？去參加寶可夢大賽，然後拿到冠軍吧。

參加大賽有個先決條件，那就是必須收集八個道館徽章。這些道館徽章可不是那麼輕易就讓你拿到手的，道館會派出他們拿手的寶可夢來與你對戰，你必須找到方法才能取得勝利！這裡使用了**工廠模式**來演示打到第三道館！

### UML：
Gym（道館）是個介面，他有個 `assignTrainer`（派出訓練家）的方法。所有道館均實作 Gym 介面，皆會派出 Trainer（訓練家）來與玩家對戰。每個道館主打的風格都不同，像是第一道館主打岩系寶可夢，就會派出岩系訓練家來對戰。所有屬性的訓練家都會繼承 Trainer 並實作方法。 

![Factory UML](/1_Basic/Design_Pattern/Image/Factory_J.png "Factory UML")

### Code：
Trainer（訓練家）是個抽象類別，擁有 `name`（訓練家名稱），`pokemons`（擁有的寶可夢）屬性。其子類別必須實作 `battleInfo` 方法來顯示戰鬥資訊。

```Java
/** 訓練家 **/
public abstract class Trainer {
    /** 訓練家名字 **/
    protected String name;
    protected String getName() { return this.name; }
    protected void setName(String name) { this.name = name; }
    /** 擁有的寶可夢 **/
    protected String[] pokemons;
    protected String[] getPokemons() { return this.pokemons; }
    protected void setPokemons(String[] pokemons) { this.pokemons = pokemons; }
    /** 戰鬥資訊 **/
    protected abstract void battleInfo();
}
```

不同屬性的訓練家都會繼承 Trainer 類別，並且實作抽象方法 `battleInfo`。

```Java
/** 岩系寶可夢訓練家 **/
public class RockTrainer extends Trainer {
    @Override
    protected void battleInfo() {
        System.out.println(this.getName() + "穿著登山裝自信登場!"
            + "對戰寶可夢為:" + Arrays.toString(this.getPokemons()));
    }
}
```
```Java
/** 水系寶可夢訓練家 **/
public class WaterTrainer extends Trainer {
    @Override
    protected void battleInfo() {
        System.out.println(this.getName() + "穿著比基尼亮麗登場 ❤❤"
                + " 對戰寶可夢為:" + Arrays.toString(this.getPokemons()));
    }
}
```
```Java
/** 電系寶可夢訓練家 **/
public class ElectricTrainer extends Trainer {
    @Override
    protected void battleInfo() {
        System.out.println(this.getName() + "穿著橡膠裝閃電登場 ⚡⚡⚡"
                + " 對戰寶可夢為:" + Arrays.toString(this.getPokemons()));
    }
}
```

Gym（道館）是個介面，其實例必須實作 `assignTrainer` 方法。

```Java
/** 道館 **/
public interface Gym {
    /** 派出訓練家 **/
    Trainer assignTrainer();
}
```

所有的道館均會實作 Gym 介面，實作其 `assignTrainer` 方法來派出他們得意的訓練家。

```Java
/** 深灰道館，對戰寶可夢其屬性為岩石屬性，道館館主為小剛，玩家獲勝之後可以獲得灰色徽章 **/
public class PewterGym implements Gym {
    @Override
    public Trainer assignTrainer() {
        return new RockTrainer();
    }
}
```
```Java
/** 華藍道館，對戰寶可夢其屬性為水屬性，道館館主為小霞，玩家獲勝之後可以獲得藍色徽章**/
public class CeruleanGym implements Gym {
    @Override
    public Trainer assignTrainer() {
        return new WaterTrainer();
    }
}
```
```Java
/** 枯葉道館，對戰寶可夢其屬性為電屬性，道館館主為馬志士，玩家獲勝之後可以獲得橙色徽章 **/
public class VermilionGym implements Gym {
    @Override
    public Trainer assignTrainer() {
        return new ElectricTrainer();
    }
}
```

### Sample：
玩家過關斬將，挑戰到第三道館！

從程式碼可觀察到，選擇要打哪個道館的邏輯，是在客戶端來判斷。之後如果要打其它的道館，只要再新增該道館類別以及其道館訓練家類別即可，不需修改原有的道館類別，這就是與簡單工廠模式的不同之處。

```Java
/** 工廠模式-示範 */
public class FactoryDemo {
    public static void main(String[] args) {
        /** 玩家來到了深灰市，挑戰深灰道館 **/
        Gym gym = new PewterGym();
        Trainer trainer = gym.assignTrainer();
        trainer.setName("小剛");
        trainer.setPokemons(new String[]{"小拳石", "大岩蛇"});
        trainer.battleInfo();
        /** 玩家來到了華藍市，挑戰華藍道館 **/
        gym = new CeruleanGym();
        trainer = gym.assignTrainer();
        trainer.setName("小霞");
        trainer.setPokemons(new String[]{"海星星", "蚊香蝌蚪", "可達鴉"});
        trainer.battleInfo();
        /** 玩家來到了枯葉市，挑戰枯葉道館 **/
        gym = new VermilionGym();
        trainer = gym.assignTrainer();
        trainer.setName("馬志士");
        trainer.setPokemons(new String[]{"小磁怪", "頑皮蛋", "雷丘", "三合一磁怪"});
        trainer.battleInfo();
    }
}
```

![Factory Result](/1_Basic/Design_Pattern/Image/Factory_R.png "Factory Result")