# 簡單工廠模式：選擇御三家
### Introduce：

所有創建的邏輯都封裝在工廠類別裡，客戶端只要傳入參數即可取得需要的類別。有以下特點：

* 客戶端沒有創建類別的責任，所有責任都在工廠類別裡。
* 欲創建新類別必須修改工廠類別，而且如果類別過多將造成難以維護及擴展。

### Main：
在一開始遊戲開始時，玩家從自己的住家離開後，必須到研究所找大木博士。大木博士跟你寒暄一番後，在他旁邊的桌子上有三顆精靈球，他會請你從這三顆精靈球中挑選一顆當作自己的寶可夢。這裡就存在著**簡單工廠模式**。

### UML：
Pokemon（寶可夢）為抽象類別。Bulbasaur（妙蛙種子）、Charmander（小火龍）、Squirtle（傑尼龜）均是 Pokemon 的子類別。而 ProfessorSamuelOak（大木博士）則是簡單工廠類別，負責提供玩家指定的寶可夢。

![Simple Factory UML](/1_Basic/Design Pattern/Image/SimpleFactory_J.png "Simple Factory UML")

### Code：

Pokemon 是個抽象類別，所有寶可夢都會繼承此類別。它擁有 `name`（寶可夢名稱）屬性，並且有個 `showInfo` 抽象方法，可以顯示寶可夢資訊。
```Java
/** 寶可夢 */
public abstract class Pokemon {
    protected String name;
    protected String getName() { return this.name; }
    protected void setName(String name) { this.name = name;}
    protected abstract void showInfo();
}
```

Bulbasaur（妙蛙種子）、Charmander（小火龍）、Squirtle（傑尼龜）均是寶可夢，所以繼承 Pokemon，並實作 `showInfo` 方法。
```Java
/** 妙蛙種子 **/
public class Bulbasaur extends Pokemon {
    @Override
    protected void showInfo() {
        System.out.println(this.getName() + "，草系寶可夢!");
    }
}
```
```Java
/** 小火龍 **/
public class Charmander extends Pokemon {
    @Override
    protected void showInfo() {
        System.out.println(this.getName() + "，火系寶可夢!");
    }
}
```
```Java
/** 傑尼龜 **/
public class Squirtle extends Pokemon {
    @Override
    protected void showInfo() {
        System.out.println(this.getName() + "，水系寶可夢!");
    }
}
```

ProfessorSamuelOak（大木博士）有三個靜態常數，代表桌子上的三顆精靈球的位置。玩家只要在其提供的方法 `getPokemon` 傳入靜態常數（選擇要哪個位置的精靈球），就會返回指定的寶可夢。從程式碼可看出，所有建立寶可夢類別的邏輯全部都被封裝起來，只要傳入參數便可得到寶可夢，這就是簡單工廠模式的特色。

```Java
/** 大木博士 **/
public class ProfessorSamuelOak {
    /**
     * 分別代表台上三顆精靈球
     **/
    public static final String LEFT = "left";
    public static final String MIDDLE = "middle";
    public static final String RIGHT = "right";
    /**
     * 選擇寶可夢
     *
     * @param choice
     * @return
     */
    public static Pokemon getPokemon(String choice) {
        switch (choice) {
            case LEFT:
                Charmander charmander = new Charmander();
                charmander.setName("小火龍");
                return charmander;
            case MIDDLE:
                Squirtle squirtle = new Squirtle();
                squirtle.setName("傑尼龜");
                return squirtle;
            case RIGHT:
                Bulbasaur bulbasaur = new Bulbasaur();
                bulbasaur.setName("妙蛙種子");
                return bulbasaur;
            default:
                System.out.println("沒有選擇寶可夢 !");
                return null;
        }
    }
}
```

### Sample：
現在玩家分別查看每個精靈球是什麼寶可夢，最後決定要哪隻寶可夢來開始自己的旅程。

```Java
/** 簡單工廠模式-示範 */
public class SimpleFactoryDemo {
    public static void main(String[] args) {
        System.out.println("選擇左邊的精靈球");
        Pokemon pokemon = ProfessorSamuelOak.getPokemon(ProfessorSamuelOak.LEFT);
        pokemon.showInfo();
        System.out.println("選擇中間的精靈球");
        pokemon = ProfessorSamuelOak.getPokemon(ProfessorSamuelOak.MIDDLE);
        pokemon.showInfo();
        System.out.println("選擇右邊的精靈球");
        pokemon = ProfessorSamuelOak.getPokemon(ProfessorSamuelOak.RIGHT);
        pokemon.showInfo();
    }
}
```
![Simple Factory Result](/1_Basic/Design Pattern/Image/SimpleFactory_R.png "Simple Factory Result")
