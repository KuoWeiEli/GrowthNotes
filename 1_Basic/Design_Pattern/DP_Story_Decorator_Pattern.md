# 裝飾模式：寶貝球
### Introduce：

能夠靈活地加入各種不同職責的類別，進而組合自己需要的類別。有以下特點：
* 明確切割職責，讓各個類別能夠專注自己的職責。
* 要加入新功能（職責）時，不需要修改原類別，建立新類別即可。

### Main：

相信玩家在收服寶可夢的時候，都會選擇一種稱為寶貝球的道具，將道具往寶可夢所在方向投擲過去。一開始大木博士會給你幾顆最便宜，最普通的寶可夢寶貝球。這種寶貝球的功能最少，收服寶可夢的能力也是一般。隨著劇情的發展，陸續會看到各式各樣的寶貝球，這裡也將看到**裝飾模式**的應用。

### UML：

各式各樣的寶貝球（MasterBall 大師球、BeautyBall 華麗球、PokeBall 普通寶貝球）都繼承 Ball（小型球體）。有沒有發現這三種球有什麼相異之處嗎？MasterBall、BeautyBall 各自都擁有一個自身父類別 Ball 的屬性。它們被稱為裝飾類別，在建構的時候必須要傳入具體類別 PokeBall。而 PokeBall 就是實作主要邏輯─**收服寶可夢**的類別。

![Decorator UML](/1_Basic/Design_Pattern/Image/Decorator_J.png "Decorator UML")

### Code：

Ball（小型球體），它的方法 `cast` 能夠將本身投擲出去。並且有個抽象方法 `gotcha`，其子類別必須實作此方法。（在這裡有另外一種模式 Pattern 的味道，讀者聞出來了嗎？哈哈）

```Java
/** 用來收服寶可夢的小型球體 **/
public abstract class Ball {

    /** 往想要收服的寶可夢方向丟擲 **/
    public void cast() {
        System.out.println("丟擲出去！");
        gotcha();
    }

    /** 收服寶可夢方法 **/
    public abstract void gotcha ();
}
```

PokeBall（普通寶貝球），它是具體類別，繼承了 Ball，實作了 `gotcha` 收服寶可夢的方法。
```Java
/** 普通寶貝球 **/
public class PokeBall extends Ball {

    @Override
    public void gotcha() {
        System.out.println("收服了寶可夢！");
    }
}
```

BeautyBall（華麗球）、MasterBall（大師球），也都繼承了 Ball，實作 `gotcha` 方法。但不是自己去實作，而是藉由建構時輸入的的參數 Ball，使用此參數的 `gotcha` 方法來實作。可看到 BeautyBall 在 `gotcha` 方法前後添加了特效功能，MasterBall 也在前方加強了收服邏輯。其實這跟 Proxy Pattern（代理模式）結構相似，只是在用途上則是完全不同。

```Java
/** 華麗球 **/
public class BeautyBall extends Ball {
    private Ball ball;

    public BeautyBall(Ball ball) {
        this.ball = ball;
    }

    @Override
    public void gotcha() {
        if (null == ball) return;

        /** 在收服寶可夢前後均會放出華麗的特效 **/
        beauty();
        ball.gotcha();
        beauty();
    }

    /** 華麗特效 **/
    private void beauty() {
        System.out.println("散發七彩光芒！");
    }
}
```
```Java
/** 大師球 **/
public class MasterBall extends Ball {
    private Ball ball;

    public MasterBall(Ball ball) {
        this.ball = ball;
    }

    @Override
    public void gotcha() {
        if (null == ball) return;

        /** 額外增加大師球的功能 **/
        System.out.println("大師球 100% ");
        ball.gotcha();
    }
}
```

### Sample：

現在玩家拿起了寶貝球...。

從下面的程式碼中可看出，任意的組合裝飾類別與實體類別，可以動態地增加寶貝球收服的功能，這就是裝飾模式的特色。

```Java
/** 裝飾模式 - 示範 **/
public class DecoratorDemo {
    public static void main(String[] args) {
        System.out.println("玩家拿起寶貝球...");
        Ball pokeBall = new PokeBall();
        pokeBall.cast();

        System.out.println("---------------------");
        System.out.println("遇到超夢...拿起大師球...");
        pokeBall = new MasterBall(new PokeBall());
        pokeBall.cast();

        System.out.println("---------------------");
        System.out.println("遇到超夢，女友又在旁邊...? 加上一些炫麗的特效好了...");
        pokeBall = new BeautyBall(new MasterBall(new PokeBall()));
        pokeBall.cast();
    }
}
```

![Decorator Result](/1_Basic/Design_Pattern/Image/Decorator_R.png "Decorator Result")