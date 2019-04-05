# 責任鏈模式：積分對戰
### Introduce：

將處理請求的不同邏輯封裝成一系列的物件，請求可以沿著此物件形成的鏈狀結構不斷傳遞直到有物件處理為止。
* 處理請求的不同邏輯彼此之間沒有耦合關係。
* 可以輕易更改鏈狀結構，使得處理請求的邏輯可以很靈活地添加、修改。

### Main：

因為以前寶可夢剛風靡全球的時代，並不像現在網路那麼發達。所以如果要與其他玩家進行寶可夢對戰的話，只能使用通信對戰（這個筆者也從來沒有用過，所以不懂）。如果那時候的網路像現在那麼發達，寶可夢對戰可以像現在大部分的手遊一樣，擁有競技場功能，可以線上匹對對手，與積分相近的對手對戰的話，那該有多好啊～！那麼這樣的機制，可以由**責任鏈模式**來設計。

### UML：

Battle（對戰）是個抽象類別，MasterBattle（大師對戰）、AdvancedBattle（進階對戰）、BeginnerBattle（初級對戰）依序代表著積分由高而低的對戰。它們都繼承 Battle 這個抽象類別，實作其方法處理 Request（對戰請求）以達到各自對戰的配對邏輯。

![ChainOfResponsibility UML](/1_Basic/Design_Pattern/Image/ChainOfResponsibility_J.png "ChainOfResponsibility UML")

### Code：

我們先想想何謂鏈狀結構，因為這是此模式的核心精神。鏈狀就是像鏈子一樣，由許多物件彼此頭尾相連成為一條直線。這些彼此相連的物件可以發現它們的**本質**是一樣的。舉例來說金項鍊每個環會是金這個元素，而不是銀（否則就要叫銀項鍊了）。

拉回來看積分對戰這個例子。積分對戰從低到高都有不同的配對邏輯，Battle（對戰）就是積分對戰的本質。我們想要由低到高形成鏈狀，就是本質與本質需要有個互相連接的**參考**，意味著下一個接著你的本質是什麼？而下一個本質也同樣下去就會形成鏈狀結構！而這個參考就需要 `nextBattle` 變數來儲存。而方法的鏈狀設計其實也有異曲同工之妙，Battle 的 `setNextBattle` 就是採取 Chain method 方式設計（可以看看 Demo 的用法）。最後，所有的子類別都須實作 `match` 方法，達成它們的配對邏輯！

```Java
/** 對戰 **/
public abstract class Battle {
    /** 下一等級對戰 **/
    private Battle nextBattle;
    /** 配對 **/
    public abstract void match(Request request);

    public Battle getNextBattle() {
        return nextBattle;
    }

    /** 這邊返回下一級對戰物件，方便可以使用 Chain method 方式設定 **/
    public Battle setNextBattle(Battle battle) {
        this.nextBattle = battle;
        return this.nextBattle;
    }
}
```

剛剛上面有提到繼承 Battle 須實作 `match` 方法。我們可以看到下列各級對戰都有不同配對邏輯的實作！

```Java
/** 初級對戰 **/
public class BeginnerBattle extends Battle {
    @Override
    public void match(Request request) {
        if (request.getScore() < 40) {
            System.out.println("載入街頭、空地等場景。");
            System.out.println("尋找初級的對手進行配對！");
        } else {
            if (getNextBattle() != null)
                getNextBattle().match(request);
        }
    }
}
```
```Java
/** 進階對戰 **/
public class AdvancedBattle extends Battle {
    @Override
    public void match(Request request) {
        if (request.getScore() < 80) {
            System.out.println("載入道館等場所。");
            System.out.println("尋找高手進行配對中！");
        } else {
            if (getNextBattle() != null)
                getNextBattle().match(request);
        }
    }
}
```
```Java
/** 大師對戰 **/
public class MasterBattle extends Battle {
    @Override
    public void match(Request request) {
        if (request.getScore() <= 99) {
            System.out.println("載入聯盟比賽場地。");
            System.out.println("與大師等級的對手進行配對！");
        } else {
            System.out.println("恭喜！您已是全聯盟最強的訓練家！");
        }
    }
}
```

上面的這些對戰類別都參考著傳入參數 Request（對戰請求） 的 `getScore` 積分值來決定要採用哪種對戰配對邏輯！一個高積分值的對戰請求將會從 BeginnerBattle 開始去做判斷，如果不符合此層級，則會到下一層級 AdvancedBattle 去做判斷。如果都無法處理這個對戰請求，意味著積分值達到最高。此時請求已傳到 MasterBattle 並且沒有下一層級的對戰能夠處理了，所以顯示「**恭喜！您已是全聯盟最強的訓練家！**」。

```Java
/** 對戰請求 **/
public class Request {
    /** 積分 **/
    private int score;

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }
}
```

可以發現每個對戰類別彼此不相干擾，沒有任何耦合。請求的處理順序完全依靠鏈狀怎麼去設計（這個例子為 BeginnerBattle → AdvancedBattle → MasterBattle），隨時想要更改順序或增加對戰邏輯都是非常容易的！這就是責任鏈模式的特色！

### Sample：

現在來演示積分由低到高進行積分對戰匹配！

```Java
/** 責任鏈模式 - 示範 **/
public class ChainOfResponsibilityDemo {
    public static void main(String[] args) {
        /** 設定對戰順序，初階→進階→大師 **/
        Battle beginner = new BeginnerBattle();
        beginner.setNextBattle(new AdvancedBattle())
                .setNextBattle(new MasterBattle());

        System.out.println("初心者發出對戰請求...");
        Request request = new Request();
        request.setScore(0);
        beginner.match(request);

        System.out.println("高手發出對戰請求...");
        request.setScore(60);
        beginner.match(request);

        System.out.println("大師發出對戰請求...");
        request.setScore(92);
        beginner.match(request);

        System.out.println("冠軍發出對戰請求...");
        request.setScore(100);
        beginner.match(request);
    }
}
```

![ChainOfResponsibility Result](/1_Basic/Design_Pattern/Image/ChainOfResponsibility_R.png "ChainOfResponsibility Result")
