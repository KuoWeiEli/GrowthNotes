# 氣泡排序法

* 時間複雜度（Time Complexity）：
    * Bast Case：Ο(n)
    * Worst Case：Ο(n^2)
* 空間複雜度（Space Complexity）：θ(1)
* 穩定性（Stable/Unstable）：穩定（Stable）

* * *
Javascript：

```Javascript
var bubbleSort = data => {
	var flag = true; // 是否需要排序
	// 參數 data 進行迴圈
	for (var i = 0; i < data.length -1 && flag; i++) {
		// 開始比較參數 data，每一輪都會將最大值往右邊推進，所以經過 i 輪的下一輪會再減去 i 次
		flag = false; // 預想不需要排序，剛好數值已是由小到大
		for (var j = 0; j < data.length - 1 - i; j++) {
			// 第一筆若大於下一筆，則兩者交換
			if (data[j] > data[j+1]) {
				swap(data, j, j+1);
				flag = true; // 此輪有做交換，所以下一輪需要排序
			}
		}
	}
}
// 利用解構方式互換陣列 data 索引 i 及索引 j 的值
var swap = (data, i, j) => [data[i], data[j]] = [data[j], data[i]];
```
Result：
```Javascript
var data = [2,9,7,5,1,8,0,3,4,6];
bubbleSort(data);
console.log(data);  // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
