# 淺談物件的複製 By Javascript
## 前言
在現實中，我們很輕易地交換彼此的資訊，並使用資訊在各種場所，做著不同的事情。比如像 Amy 的身高 160 cm、體重 45 kg，這是一種資訊（資料訊息）。在程式中，160  cm、45 kg，一般都會使用宣告子宣告一個變數（比如 Javascript 使用 `var`，ES6 時又多了兩個關鍵字 `let`、`const`），來儲存這個數值。而 Amy 有這兩個資訊，我們在程式中可以稱 Amy 是一個物件。比如使用下列程式碼來保存 Amy 這個物件：

```Javascript
var amy = {
    height: 167,
    weight: 45
}
```

接下來筆者就用**現實**與**程式**兩相對照，來談論今天的主題：**物件的複製**！

## 關於複製
先講數值，比如把 Amy 的身高保存在變數 `amyHeigth`，因為 Cathy 身高目前跟 Amy 一樣，所以直接把變數給 Cathy。不久 Cathy 身高抽高到 170 cm。而 Amy 的身高依然維持在 160 cm。

```Javascript
var amyHeight = 160; 
var cathyHeight = amyHeight;
cathyHeight = 170;
console.log(amyHeight);     // 160
console.log(cathyHeight);    //170
```

這就如同現實中，你可以很輕易地保存 160、170 這些數值，並在任何地方使用它。這些數值在程式中稱為基本型別（Primative Type）。

再講物件。如果今天 Amy 和 Cathy 的身高體重一樣，直接把 Amy 這個變數給 Cathy，不久 Cathy 身高一樣抽高到 170 cm，這時候會發生甚麼事？

```Javascript
var amy = {
    height: 160,
    weight: 45
}
var cathy = amy;  
cathy.height = 170;
console.log(`amy's height is ${amy.height}`);     // amy's height is 170
console.log(`cathy's height is ${cathy.height}`); // cathy's height is 170
```

你會發現 Amy 的身高竟然也抽高到 170 cm。那是因為在現實中，Amy 是獨一無二的個體，並不是普通的數值。在這裡 Amy 和 Cathy 的關係就變成了個體和影子的關係，個體身高變了，影子也跟著變化。在程式中也是一樣，Amy 這個物件被保存到一個記憶體位置，如果你將 Amy 給 Cathy，實際上是將 Amy 的記憶體指標傳給 Cathy。所以你改了 Cathy 的身高，Amy 的身高也會跟著變動。而 Amy 在程式中稱為參考型別（Reference Type）。
```Javascript
var cathy = amy; // 把 Amy 物件的記憶體位置指標傳給 Cathy
```

Javascript 中的基本型別有五種，分別為**布林（Boolean）**、**數值（Number）**、**字串（String）**、**空值（null）**、**未定義（undefined）**。
而參考型別則只有一種：**物件（Object）** 。

> **Hint！**
>
> *  null、undefined 這兩類是 Javascript 中特別的設計，你用 `typeof null` 甚至顯示出 object。
> *  Function、Array、Math、RegExp 諸如此類都是 Object。

## Javascript 複製的方法
從前面講述中可知道，基本型別不用擔心數值會被更動，可以在任何地方去使用，而參考型別就需要特別注意。

在程式中，參考型別有兩種複製方法：
* 淺拷貝（shallow copy）：複製物件的記憶體指標。
* 深拷貝（deep copy）：複製物件到新的記憶體。 

所以 Cathy 要怎麼得到 Amy 的身高體重，又不會在事後影響到 Amy？
答案就是使用**深拷貝**，將 Amy 整個物件複製一份到新的記憶體空間。隨後將指標傳給 Cathy 當參考即可。簡單方法如下：
```Javascript
var cathy = {
  height: amy.height,
  weight: amy.weight
}
```

上面的寫法很冗長，如果在 ES6 可以使用更簡潔的寫法：`Object.assign`  方法：
```Javascript
var cathy = Object.assign({}, amy);
```

但是如果今天 Amy 物件裡的資料又包含了一個物件，`Object.assign` 就無法使用了（因為只能複製第一層資料，如果第一層資料是物件型態，將會複製其指標）。如果這個物件內的資料不是函式 Function 的話，那我們可以改用 JSON 將 Amy 轉為字串，在將字串解析為物件傳給 Cathy：
```Javascript
var cathy = JSON.parse(JSON.stringify(amy));
```

如果 Amy 的資料有函式的話，就不能夠使用 JSON 的方式，因為函式會被 JSON 給吃掉，無法轉換：
```Javascript
var amy = {
    height: 160,
    weight: 45,
    sayHello() {
        console.log('Hello !');
    }
}
var cathy = JSON.parse(JSON.stringify(amy));
console.log(cathy); // {height: 160, weight: 45} ，sayHello( ) 並沒有被複製起來
```

其實很多有名的 Javascript 框架都有提供淺拷貝與深拷貝，這裡介紹 JQuery 與 Angular：
```Javascript
// JQuery 淺拷貝
var cathy = $.extend({},  amy);   
// 或者使用 
var cathy = {};
$.extend(cathy, amy);
// Angular 淺拷貝
var cathy = angular.extend({}, amy); 
// 或者使用
var cathy = {};
angular.extend(cathy, amy);
// JQuery 深拷貝
var cathy = $.extend(true, {}, amy);  // JQuery
// 或者使用
var cathy = {};
$.extend(true,cathy, amy);
// Angular 深拷貝
var cathy = angular.copy(amy);
// 或者使用
var cathy = {};
angular.copy(amy, cathy);
```

## 嘗試自己寫一個 Deep Clone

筆者嘗試使用 `for...in` 遍歷物件屬性，再藉由判斷型態是物件的進行遞迴：
```Javascript
function deepClone(obj, cloneObj) {
    for (var attr in obj) {
        if (typeof obj[attr] === 'object') {
            cloneObj[attr] = {};
            deepClone(obj[attr], cloneObj[attr]);
        } else
            cloneObj[attr] = obj[attr];
    }
}
```

在 Object 的情況下適用，但是如果是 Array 的話會有一些問題存在，比如 Array :
```Javascript
var a = [undefined, undefined, null, 1];
var b = [];
deepClone(a, b);
```
執行完後，b會是什麼 ? 答案是 [undefined, undefined , {}, 1]，`null` 變成了一個物件，因為 `typeof null` 是 `object`，Javascript 需要注意此特性！

要複製 Array 用 ES6 的擴展語法（Spread 運算符 `...`），是會比較好的方法：
```Javascript
var b = [...a]:
```
