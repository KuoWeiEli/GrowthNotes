# 備忘錄模式：遊戲存檔
### Introduce：

將一個物件內部狀態封裝成物件並保存，如要回復先前內部狀態，只要調用物件即可。有以下特色：
* 內部狀態的封裝，減少了其他程式的耦合。
* 可以實作命令模式中的取消功能，回復執行命令前的狀態。

### Main：

寶可夢遊戲有很多特色，除了收服寶可夢、挑戰道館、劇情任務外，還可以孵蛋、培養寶可夢的成長值等等。這並不是一朝一夕就可以破玩的，如果隔天要繼續玩寶可夢，是不是就不行關掉遊戲機？但遊戲機多半是使用電池來維持運作，總有電池沒電需要做更換的時候，那麼該怎麼辦才能夠保持記錄，下次繼續遊玩呢！還好寶可夢提供了記錄功能，能夠使玩家保存當下遊戲的狀態。這邊就可以使用**備忘錄模式**來達成這個功能！

### UML：

Player（玩家）保存著玩家遊玩的資訊，它將這些資訊封裝成 Archive（記錄）並保存到 ArchiveZone（卡匣記憶區）中。

![Memento UML](/1_Basic/Design_Pattern/Image/Memento_J.png "Memento UML")

### Code：

Player（玩家）這個類別擁有著許多遊玩資訊。像是 `pokemons`（收服的寶可夢）、`gyms`（打敗的道館）、`location`（目前所在的位置）。這些資訊都會隨著遊玩時間愈多而改變其數值。玩家可以使用 `save`（存檔）方法來保存，或者日後使用 `load`（讀取）方法來載入。也可以使用 `showInfo`（顯示玩家資訊）來查看。

```Java
/** 玩家 **/
public class Player {
    /** 收服的寶可夢 **/
    private List<String> pokemons = new ArrayList();
    /** 打敗的道館 **/
    private List<String> gyms = new ArrayList();
    /** 目前所在的位置 **/
    private String location;

    /** 存檔 **/
    public Archive save() {
        return new Archive(pokemons, gyms, location);
    }

    /** 讀檔 **/
    public void load(Archive archive) {
        this.pokemons = archive.getPokemons();
        this.gyms = archive.getGyms();
        this.location = archive.getLocation();
    }

    /** 顯示玩家資訊 **/
    public void showInfo() {
        System.out.printf("寶可夢：%s%n道館：%s%n目前位置：%s%n",
            this.pokemons, this.gyms, this.location);
    }

    public List<String> getPokemons() {
        return pokemons;
    }

    public void setPokemons(List<String> pokemons) {
        this.pokemons = pokemons;
    }

    public List<String> getGyms() {
        return gyms;
    }

    public void setGyms(List<String> gyms) {
        this.gyms = gyms;
    }

    public String getLocation() {
        return location;
    }

    public void setLocation(String location) {
        this.location = location;
    }
}
```

Archive（記錄）類別負責保存玩家的遊玩資訊。

```Java
/** 記錄 **/
public class Archive {
    /** 收服的寶可夢 **/
    private List<String> pokemons = new ArrayList();
    /** 打敗的道館 **/
    private List<String> gyms = new ArrayList();
    /** 目前所在的位置 **/
    private String location;

    public Archive(List pokemons, List gyms, String location) {
        this.pokemons = pokemons;
        this.gyms = gyms;
        this.location = location;
    }

    public List<String> getPokemons() {
        return pokemons;
    }

    public void setPokemons(List<String> pokemons) {
        this.pokemons = pokemons;
    }

    public List<String> getGyms() {
        return gyms;
    }

    public void setGyms(List<String> gyms) {
        this.gyms = gyms;
    }

    public String getLocation() {
        return location;
    }

    public void setLocation(String location) {
        this.location = location;
    }
}
```

ArchiveZone（卡匣記憶區）類別則是負責保存 Archive 物件。

```Java
/** 卡匣記憶區 **/
public class ArchiveZone {
    /** 寶可夢遊戲只能保存一個存檔 **/
    private Archive archive;

    public Archive getArchive() {
        return archive;
    }

    public void setArchive(Archive archive) {
        this.archive = archive;
    }
}
```

### Sample：

現在就來看看寶可夢遊戲保存記錄的過程。玩家今日遊玩一段時間後，收服了不少寶可夢也打敗了兩個道館。到了第三個城市-枯葉市時已經累了，所以選擇存檔關機，明天再繼續玩！可以看見遊玩資訊被封裝成物件，並保存在記憶體區。日後讀取的時候只要再從記憶體區取回資訊即可，這就是備忘錄模式的特色！

```Java
/** 備忘錄模式 - 示範 **/
public class MementoDemo {
    public static void main(String[] args) {
        Player player = new Player();
        List pokemons = new ArrayList();
        pokemons.add("綠毛蟲");
        pokemons.add("小拉達");
        pokemons.add("皮卡丘");
        player.setPokemons(pokemons);

        List gyms = new ArrayList();
        gyms.add("深灰道館");
        gyms.add("華藍道館");
        player.setGyms(gyms);

        player.setLocation("枯葉市");

        /** 玩累了，存檔後關機，明天再玩！ **/
        ArchiveZone zone = new ArchiveZone();
        zone.setArchive(player.save());

        System.out.println("----------------關機----------------");
        System.out.println("-------------隔天讀取紀錄-------------");
        Player tomorrowPlayer = new Player();
        tomorrowPlayer.load(zone.getArchive());
        tomorrowPlayer.showInfo();
    }
}
```

![Memento Result](/1_Basic/Design_Pattern/Image/Memento_R.png "Memento Result")
