# 線性搜尋法

又稱循序搜尋法（Sequential Search）。有兩種方法實作，分別為**崗哨（Sentinel）**、**非崗哨（Non-Sentinel）**。
* 時間複雜度（Time Complexity）：Ο(n)

* * * 
###  崗哨
Javascript：
```Javascript
var linearSearchWithSentinel = (data, key) => {
	var index = 0; // 索引從 0 開始
	data.push(key); // 在最後面加入 key（崗哨），
	
	/** 遍歷參數 data 是否在某個索引位置其值為 key。
		因為在最後面有設置崗哨，所以不必檢查陣列是否結束。
		資料量大時，比無崗哨節省大約一半的時間 **/
	while(data[index++] !== key) {} 
	data.pop(key); // 移除崗哨，使 data.length 回歸原始長度

	// 當索引不等於 data 的長度，該索引的數值就等於 key。否則找不到則遍例到 data 末端。
	return --index !== data.length ? index : -1; 
}
```
Result：
```Javascript
var data = [2,9,7,5,1,8,0,3,4,6];
var index = linearSearchWithSentinel(data, 1);
console.log(index); // 4
```
* * * 
### 非崗哨
Javascript：
```Javascript
var linearSearchWithoutSentinel = (data, key) => {
	var index = 0; // 索引從 0 開始
	while (index < data.length) { // 遍歷 data，檢查陣列是否結束
		// data 的某個索引值等於 key，則返回該 index
		if (data[index++] == key) return index -1; 
	}
	return -1; // 找不到則返回 -1
}
```
Result：
```Javascript
var data = [2,9,7,5,1,8,0,3,4,6];
var index = linearSearchWithoutSentinel(data, 1);
console.log(index); // 4
```
* * *
### 崗哨與非崗哨比較

崗哨與非崗哨最大的差別在於，崗哨避免了**檢查陣列是否結束**這個邏輯。理論上比非崗哨節省一半時間。下面就來實測看看：

```Javascript
// 計時器
var timer = fn => {
	var start = new Date().getTime();
	fn();
	var end = new Date().getTime();
	console.log(`花費${end-start}ms`);
}
// 測試資料塞入 1千萬01個數字，尋找索引為第一千萬的位置，來比較兩方法所花費的時間
var largeData = [...Array(10000001).keys()]

// 非崗哨測試
timer(()=>console.log('result: ' + linearSearchWithoutSentinel(largeData, 10000000)));
// 100000000，35ms

// 崗哨測試
timer(()=>console.log('result: ' + linearSearchWithSentinel(largeData, 10000000)));
// 100000000，22ms

```
可發現搜尋大量資料的時候，崗哨測試比非崗哨快了 13ms（樣本數一千萬）。