# 解譯器模式：石板之謎
### Introduce：

將某段文字其文法規則封裝成一系列的物件。有以下特色：
* 用戶端不會與文法規則的邏輯產生耦合。文法規則物件的擴展極為容易。
* 用戶端因為抽離出文法規則，所以提高了程式碼可讀性。

### Main：

玩家總會在某個時候在某個山洞找到一塊古代石板。這個石板從 NPC 那裏得知是一個藏寶圖，相傳解開藏寶圖之人能夠得到重要珍貴的道具。因此玩家找到了瘋狂科學家 NPC，拜託他製作能夠解開古代石板上的文字，得到其中透露的消息，找到這個傳說中的大秘寶。這時候瘋狂科學家就可以使用**解譯器模式**來製造機器幫助玩家。

### UML：

瘋狂科學家正在考察 AncientPlate（古代石板），上面刻畫了這串文字：`H70 B1101111 O156 H64`。他發現似乎有某種規則存在，因此他製作了三部解譯器 BinaryMachine（2 進制數字解譯器）、OctalMachine（8 進制數字解譯器）、HexMachine（16 進制數字解譯器）。它們都繼承了 Machine（解譯機器），各自處理古代石板上的記述文字其某種規則邏輯。

![Interpreter UML](/1_Basic/Design_Pattern/Image/Interpreter_J.png "Interpreter UML")

### Code：

AncientPlate（古代石板）上面有某種時代盛行的文字所書寫的字段 `text`。

```Java
/** 古代石板 **/
public class AncientPlate {
    private String text;

    public String getText() {
        return text;
    }

    public void setText(String text) {
        this.text = text;
    }
}
```

為了解譯這段古文，瘋狂科學家製作了 Machine（解譯器），能夠使用 `interpret` 方法分析古代石板上的文字。因為這段古文似乎有三種規則存在，這三種規則分別交給其子類別負責，所以定義了抽象方法 `exec`。

```Java
/** 解譯機器 **/
public abstract class Machine {
    /** 設置石板到解譯機器 **/
    public void interpret(AncientPlate plate) {
        if (plate.getText().length() == 0) return;

        /** 空格所在的位置 **/
        int spaceIndex = plate.getText().indexOf(" ");
        /** 取出數字的部分 **/
        String number = plate.getText().substring(1, spaceIndex);
        /** 將數字進行解譯 **/
        exec(number);
        /** 將已解譯的部分從石板中剃除 **/
        plate.setText(plate.getText().substring(spaceIndex + 1));
    }

    /** 執行 **/
    public abstract void exec(String value);
}
```

而負責這三個規則的解譯器分別為 BinaryMachine（2 進制數字解譯器）、OctalMachine（8 進制數字解譯器）、HexMachine（16 進制數字解譯器）。各自擁有解譯古文的規則邏輯。

```Java
/** 2 進制數字解譯器 **/
public class BinaryMachine extends Machine {
    @Override
    public void exec(String value) {
        System.out.printf("%s", (char) Integer.parseInt(value, 2));
    }
}
```
```Java
/** 8 進制數字解譯器 **/
public class OctalMachine extends Machine {
    @Override
    public void exec(String value) {
        System.out.printf("%s", (char) Integer.parseInt(value, 8));
    }
}
```
```Java
/** 16 進制數字解譯器 */
public class HexMachine extends Machine {
    @Override
    public void exec(String value) {
        System.out.printf("%s", (char) Integer.parseInt(value, 16));
    }
}
```

### Sample：

瘋狂科學家終於完成了解譯器，玩家取得解譯器後，開始分析古代石板上的古文。可以從下列程式碼中發現，古文的規則邏輯因為被封裝成物件，與用戶端的職責彼此分離，增加了程式碼的可讀性。如果之後又發現了另塊石板有相似的文法規則，就可以重複使用或是增加新的解譯器（新的規則類別）進行解譯。這就是解譯器模式的特色。

```Java
/** 解譯器模式 - 示範 **/
public class InterpreterDemo {
    public static void main(String[] args) {
        /** 發現一古代石板 **/
        AncientPlate plate = new AncientPlate();
        plate.setText("H70 B1101111 O156 H64 ");

        /** 向瘋狂科學家借了可以解譯古文的機器，
         *  根據讀取到不同的字段，會採用不同的部件進行解譯 **/
        System.out.printf("%s：", "寶藏地點");
        Machine machine = null;
        while (plate.getText().length() > 0) {
            switch (plate.getText().charAt(0)) {
                case 'H':
                    machine = new HexMachine();
                    break;
                case 'O':
                    machine = new OctalMachine();
                    break;
                case 'B':
                    machine = new BinaryMachine();
            }

            machine.interpret(plate);
        }
    }
}
```

![Interpreter Result](/1_Basic/Design_Pattern/Image/Interpreter_R.png "Interpreter Result")
