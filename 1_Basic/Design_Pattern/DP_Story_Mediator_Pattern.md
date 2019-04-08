# 仲介者模式：聊天系統
### Introduce：

建立一個物件（仲介者）管理多個物件彼此之間的互動，使得原本多對多的結構轉變成一對多。有以下特色：

* 多對多結構讓多個物件在很多地方都有耦合。一對多結構可以解決此問題，讓耦合集中在仲介者上面。
* 互動相關邏輯都在仲介者上面，管理邏輯能力提升但邏輯複雜度也相對提升。

### Main：

寶可夢從很久之前的版本開始，遊玩模式幾乎都傾向於單機。現在的版本不知道有沒有像一般手遊那樣，擁有豐富的社交功能。通常進入遊戲的第一個畫面是大廳，任何玩家都可以在大廳內隨意發言給任何人，或是有些重要的訊息會統一公告給所有玩家。其實也不一定要在大廳才有辦法發送訊息，像是在副本或是組隊遊玩時，也都需要與隊友互相溝通，一起探討戰術等等。可以說訊息無處不在，在任何地方都會使用到。注意上一句「**在任何地方都會使用到**」，意味著訊息互動的程式邏輯到處都會出現。如何將互動的邏輯統一由一個物件保管，就可以使用**仲介者模式**來解答！

### UML：

訊息有好幾種傳輸型式，像是玩家對玩家，玩家對頻道，頻道對玩家等等。我們可以看出這裡有兩個類別，分別是 Player（玩家）、Channel（頻道）。它們都繼承了 Member（聊天室物件），而互動邏輯都交給了 Chatroom（聊天室）的子類別 PokemonChatroom（寶可夢聊天室）去做處理。可以看到 Member 與 Chatroom 互為參考，一個 Chatroom 會對應到多個 Member，為一對多結構。在這裡 Chatroom 就被稱為仲介者物件。 

![Mediator UML](/1_Basic/Design_Pattern/Image/Mediator_J.png "Mediator UML")

### Code：

我們先來看看仲介者物件 Chatroom（聊天室），它是個抽象類別。因為是一對多結構，可以看到其成員 `channels` 與 `players` 都是 Member 的集合。並提供方法 `addChannel`、`addPlayer` 可以讓頻道或玩家加入到聊天室裡，這些集合（`channels` 、`players`）將會被子類別實作方法 `chat` 使用到。

```Java
/** 聊天室 **/
public abstract class Chatroom {
    protected List<Member> channels = new ArrayList();
    protected List<Member> players = new ArrayList();

    /** 將頻道加入到聊天室裡 **/
    public void addChannel(Member channel) {
        this.channels.add(channel);
    }

    /** 將玩家加入到聊天室裡 **/
    public void addPlayer(Member player) {
        this.players.add(player);
    }

    /** 聊天室的主要功能，傳送訊息 **/
    public abstract void chat(String msg, Member channel, Member player);
}
```

再來看看繼承 Chatroom 的類別─PokemonChatroom（寶可夢聊天室）。它實作了 `chat` 方法，這個方法就是一開始所說的訊息互動邏輯。原本可能會在任何地方使用此邏輯，現在統一集中到此類別，使得管理互動邏輯變得輕鬆許多，但會比散落到各個地方的邏輯較為複雜。這就是仲介者模式的特色。

```Java
/** 寶可夢聊天室 **/
public class PokemonChatroom extends Chatroom {
    @Override
    public void chat(String msg, Member channel, Member player) {
        /** player 為 null 時，代表發送訊息到頻道上 **/
        if (player == null)
            if (channels.contains(channel))
                System.out.printf("發送「%s」到頻道《%s》%n", msg, channel.getName());
            else
                System.out.printf("頻道《%s》不存在！", channel.getName());
        else
            if (players.contains(player))
                System.out.printf("發送「%s」到頻道《%s》中的玩家【%s】%n", msg, channel.getName(), player.getName());
            else
                System.out.printf("不存在玩家【%s】！", player.getName());
    }
}
```

因為互動邏輯都交給了仲介者物件，所以 Member（聊天室物件）會參考 Chatroom 的 `room`。並且其子類別必須實作 `send` 方法。

```Java
/** 聊天室物件 **/
public abstract class Member {
    /** 物件名稱 **/
    private String name;
    /** 每個聊天室物件都會參考到聊天室 **/
    protected Chatroom room;

    public Member(String name, Chatroom room) {
        this.name = name;
        this.room = room;
    }

    /**
     * @param msg 欲傳送訊息
     * @param channel 接收者所在頻道
     * @param player 接收者
     */
    public abstract void send(String msg, Member channel, Member player);

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

現在，Channel（頻道）、Player（玩家）繼承了 Member。可以看到其實作方法 `send` 呼叫了仲介者物件 `room`  的 `chat` 方法去實現訊息互動邏輯。

```Java
/** 頻道 **/
public class Channel extends Member {
    public Channel(String name, Chatroom room) {
        super(name, room);
        this.room.addChannel(this);
    }

    @Override
    public void send(String msg, Member channel, Member player) {
        /** 發送訊息到頻道上 **/
        this.room.chat(msg, channel, null);
    }
}
```
```Java
/** 玩家 **/
public class Player extends Member {
    public Player(String name, Chatroom room) {
        super(name, room);
        room.addPlayer(this);
    }

    @Override
    public void send(String msg, Member channel, Member player) {
        room.chat(msg, channel, player);
    }
}
```

### Sample：

筆者的夢中出現了最新版的寶可夢。這個寶可夢擁有豐富的社交功能...（夢 ing...）。

```Java
/** 仲介者模式 - 示範 **/
public class MediatorDemo {
    public static void main(String[] args) {
        /** 寶可夢聊天室 **/
        Chatroom room = new PokemonChatroom();

        /** 新增頻道1 **/
        Member firstChannel = new Channel("1", room);
        /** 新增 John、Ida **/
        Member john = new Player("John", room);
        Member ida = new Player("Ida", room);

        /** Ida 傳訊息給 John **/
        ida.send("要不要一起去抓皮卡丘？", firstChannel, john);

        /** 頻道一發送公告 **/
        firstChannel.send("頻道二因為出了一點狀況，將臨時關閉！", firstChannel, null);
    }
}
```
![Mediator Result](/1_Basic/Design_Pattern/Image/Mediator_R.png "Mediator Result")

