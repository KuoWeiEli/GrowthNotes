# 狀態模式：異常狀態
### Introduce：

將物件某個狀態的判斷、業務等相關邏輯封裝成一系列的物件。有以下特色：
* 減少大量條件分支（`if...else...`等），減低程式碼複雜度。
* 達成狀態物件與原本物件的職責分離，減少互相依賴。易於增加新狀態物件或修改其邏輯！


### Main：

還記得在 [DP ─ 命令模式：寶可夢中心的治療機器](DP_Story_Command_Pattern.md "Command_Pattern")一文中提到寶可夢的異常狀態嗎？戰鬥時，如果寶可夢遭受到特定種類的攻擊時，將會陷入異常狀態！各種不同的異常狀態將會造成寶可夢不同的影響。比如寶可夢如果陷入睡眠狀態，自己回合的時候將無法行動。陷入中毒狀態時，將會損失 HP（血量）！異常狀態其實不只這些，假設都用 `if...else...` 來判斷寶可夢的狀態值是屬於哪個異常狀態，並執行該異常狀態邏輯的話。光是 `if...else` 條件分支的部分就會產生大量程式碼！有沒有更好的設計可以解決這個問題呢？就是你了！去吧！**狀態模式**。

### UML：

所有的狀態都繼承 StatusCondition（異常狀態 ）。這些異常狀態有 BurnCondition（灼傷狀態）、SleepyCondition（睡眠狀態）、PoisonCondition（中毒狀態）以及沒有發生異常狀態的狀態 NormalCondition（正常狀態）。根據遭受什麼異常狀態，可以取用 Flag（異常狀態常數）對 Pokemon（寶可夢）類別進行設置。

![State UML](/1_Basic/Design_Pattern/Image/State_J.png "State UML")

### Code：

StatusCondition（異常狀態）是個抽象類別，定義了如果身陷異常狀態，會由 `react` 方法去完成該邏輯。

```Java
/** 異常狀態 **/
public abstract class StatusCondition {
    /** 陷入狀態寶可夢會出現什麼反應 **/
    public abstract void react(Pokemon pokemon);
}
```

NormalCondition（正常狀態）繼承 StatusCondition 實作 `react` 方法。可以看到方法內有一個 `if...else...` 條件敘述句，根據傳入的參數 `pokemon` 其 `getStatusFlag` 取得的狀態值來判斷目前寶可夢是否陷入異常狀態。如果陷入異常狀態，將 SleepyCondition（睡眠狀態）透過 `pokemon.setStatus` 設置。並且在將 `pokemon` 參數傳給剛剛設置的 SleepyCondition 其 `react` 方法，進行**下一回合**的狀態判斷處理。

```Java
/** 沒有陷入異常狀態 **/
public class NormalCondition extends StatusCondition {
    @Override
    public void react(Pokemon pokemon) {
        if (pokemon.getStatusFlag() < 1) {
            System.out.println("寶可夢沒有陷入任何異常狀態！");
        } else {
            pokemon.setStatus(new SleepyCondition());
            pokemon.getStatus().react(pokemon);
        }
    }
}
```

可以看到 SleepyCondition（睡眠狀態）與上一個 NormalCondition 其邏輯差不多一樣。一樣透過參數 `pokemon` 的 `getStatusFlag` 做狀態值的判斷。不符合則交給 PoisonCondition（中毒狀態）做**下一回合**的狀態判斷處理。

```Java
/** 睡眠狀態 **/
public class SleepyCondition extends StatusCondition {
    @Override
    public void react(Pokemon pokemon) {
        if (pokemon.getStatusFlag() == 1) {
            System.out.println("寶可夢陷入睡眠狀態，正在深深的沉睡中，無法動作！");
        } else {
            pokemon.setStatus(new PoisonCondition());
            pokemon.getStatus().react(pokemon);
        }
    }
}
```

剩下的異常狀態 PoisonCondition（中毒狀態）、BurnCondition（灼傷狀態）都是這樣子的邏輯結構。惟 BurnCondition 是本例的最後一個異常狀態，所以就沒有 `else` 之後的邏輯。

```Java
/** 中毒狀態 **/
public class PoisonCondition extends StatusCondition {
    @Override
    public void react(Pokemon pokemon) {
        if (pokemon.getStatusFlag() == 2) {
            System.out.println("寶可夢陷入中毒狀態，造成 HP 損失 20 點！");
        } else {
            pokemon.setStatus(new BurnCondition());
            pokemon.getStatus().react(pokemon);
        }
    }
}
```
```Java
/** 灼傷狀態 **/
public class BurnCondition extends StatusCondition {
    @Override
    public void react(Pokemon pokemon) {
        if (pokemon.getStatusFlag() == 3) {
            System.out.println("寶可夢陷入灼傷狀態，防禦力減低並且減少 HP 5 點傷害！");
        }
    }
}

```

介紹完狀態的類別後，接下來介紹的就是會與狀態互相依賴（互動）的 Context 類別（通稱上下文類別）─ Pokemon（寶可夢）。會依據什麼異常狀態透過 `setStatusFlag` 方法保存到 `statusFlag` 變數中。這些異常狀態值可以參考 Flag（異常狀態常數）所提供的靜態成員 `SLEEPY`、`POISON`、`BURN`。在輪到自己回合後，就會執行 `active` 方法。該方法先預設寶可夢沒有陷入任何異常狀態，透過 `setStatus` 方法將 NormalCondition 存入 `status` 變數中。然後執行 `stateHappened`（狀態開始邏輯）方法，呼叫 NormalCondition 的 `react` 方法，並把自身（Pokemon）當作參數傳入，開始進行**每一回合的狀態判斷**！

原本在 `stateHappened` 方法會有一大堆 `if...else...` 去判斷 `statusFlag` 為何，並且執行該異常狀態的邏輯。現在可以看到這些狀態邏輯從 Pokemon 中分離並封裝成狀態物件，只剩下短短的一行程式碼 ` status.react(this);`。大幅增加了程式碼可讀性！未來如果有其他的異常狀態需要加入，新增異常狀態物件即可。這就是狀態模式的特色！

```Java
/** 異常狀態常數 **/
public class Flag {
    /** 睡眠 **/
    public static final int SLEEPY = 1;
    /** 中毒 **/
    public static final int POISON = 2;
    /** 灼傷 **/
    public static final int BURN = 3;
}
```
```Java
/** 寶可夢 **/
public class Pokemon {
    /** 寶可夢的異常狀態 **/
    private StatusCondition status;

    /** 異常狀態值 **/
    private int statusFlag;

    /** 行動 **/
    public void active() {
        System.out.println("輪到自己回合了！");
        setStatus(new NormalCondition());
        stateHappened();
    }

    /** 狀態開始 **/
    public void stateHappened() {
        status.react(this);
    }

    public StatusCondition getStatus() {
        return status;
    }

    public void setStatus(StatusCondition status) {
        this.status = status;
    }

    public int getStatusFlag() {
        return statusFlag;
    }

    public void setStatusFlag(int statusFlag) {
        this.statusFlag = statusFlag;
    }
}
```

### Sample：

現在來掩飾遭受不同異常狀態時，寶可夢會有什麼樣的反應（好像有點虐待寶可夢...）。

```Java
/** 狀態模式 - 示範 **/
public class StateDemo {
    public static void main(String[] args) {
        Pokemon pokemon = new Pokemon();
        System.out.println("寶可夢受到催眠攻擊！陷入睡眠狀態...");
        pokemon.setStatusFlag(Flag.SLEEPY);
        pokemon.active();

        System.out.println("寶可夢遭受火焰攻擊！陷入灼傷狀態...");
        pokemon.setStatusFlag(Flag.BURN);
        pokemon.active();

        System.out.println("寶可夢遭受毒藥攻擊！陷入中毒狀態...");
        pokemon.setStatusFlag(Flag.POISON);
        pokemon.active();
    }
}
```

![State Result](/1_Basic/Design_Pattern/Image/State_R.png "State Result")

