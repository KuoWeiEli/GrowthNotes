# 讀書筆記：Java 8 Lambdas 技術手冊

### Chapter 1 - Introduce

* Java 8 引入了函數程式設計風格，著重於不可變值與數值之間的表示。
* 更簡單處理平行程式設計，為了能夠在多核心電腦上發揮最大效益。
* 程式碼即資料（code as data）的概念，著重更簡明的意圖。

### Chapter 2 - The feature - Lambda

* Lambda 的本體為函式介面（Functional Interface：只有單一抽象方法的介面）。
* 通過使用 Lambda 來改寫匿名函式（Annoymous Function），編譯器透過**型態推斷**可以讓程式碼更簡潔有力，展現更清楚的意圖。
* 而且以往匿名函式如果使用到外部變數，該變數需要使用關鍵字 `final` 來修飾。現在可以省略，惟注意該變數必須等效 `final`（須指派不超過一次），否則編譯會發生錯誤。
* 常用函式介面：
	1. Predicate<T>：接受 T 引數並回傳 boolean。
	2. Comsumer<T>：接受 T 引數並且不回傳。
	3. Function<T, R>：接受 T 引數並回傳 R。
	4. Supplier<T>：沒有引數並回傳 T。
	5. UnaryOperator<T>：一個 T 引數並回傳 T。
	6. BinaryOperator<T>：兩個 T 引數並回傳 T。
* 合法的 Lambda 寫法：
	1. `() -> System.out.println("hello world!");`
	2. `word -> System.out.println(word);`
	3. `() -> { // 使用大括號 }`
	4. `(x , y) -> x + y`
	5. `(Integer x, Integer y) -> x + y`

### Chapter 3 - The Stream and Collections

* Stream 內部迭代有著比傳統外部迭代 Interator 更清晰的結構，更清楚的意圖。（for 是語法蜜糖，實際上是使用 Iterator）。
* Stream 的性質：
	1. 惰性（Lazy）：回傳 Stream，意味著還未執行，只是顯示邏輯行為。
	2. 急性（Eager）：void 或有回傳值，意味著執行的結果。
* Common collections' method 每一個都是處理 Stream 上所有元素的方法：
	1. collect(toList())：以群集返回。
	2. map：轉換成另一種物件型態。
	3. filter：檢驗方法，符合的留下。
	4. flatMap：匯流 Stream。
	5. reduce & min & max：歸納類的方法：歸納, 最小值 or 最大值。
* 高階函式（higher-order function）：以函式做為引數或回傳值。
* Lambda 的設計應避免副作用的寫法，如將區域變數指派給 Lambda 函式，或使用 forEach 改變某個物件的狀態值。

### Chapter 4 - The Library

* 使用 Lambda 來改善舊有程式庫的執行效率。Example：Logger 的 `debug` 方法。
* 封箱型態（boxed）為基本型態（primary）封裝後的物件型態。雖然達到更好的封裝，但是有下列缺點：
	1. 封裝型態所佔據的記憶體大於基本型態。
	2. 裝箱（boxing）與拆箱（unboxing）在大量複雜的運算中將會明顯佔用效能。
* Stream 針對 **int**、**long**、**double** 三個基本型態設計專屬 Stream 流（前綴為基本型態，例如：LongStream），並且有其對應的方法，命名慣例如下：
	1. 回傳值為基本型態的方法，前綴字會有 `To`。例如：ToLongFunction。
	2. 引數為基本型態的方法，前綴字為該基本型態名稱。例如：LongFunction。
	3. 使用基本函式的高階函式，其中綴與後綴會是 `To`+`基本型態`。例如：mapToLong。
	4. 基本 Stream -> 封箱 Stream：使用 `mapToObj`,`boxed` 方法。
	5. 基本 Stream 有各自的**統計**物件，可以取得各種統計數值。例如：IntSummaryStatistics 的 `getMax()`, `getSum()`, etc.。
* 關於 Overload 重載方法：
	1. 多個符合 Lambda 型態的方法，將會選擇最具體的型態。
	2. 多個符合 Lambda 型態的方法，且沒有最具體的型態時，需要明確規範其物件型態。
* Annotation─@FunctionalInterface，規範被標註的物件必須是函式介面，否則編譯錯誤。
* 為了向後相容誕生出的特性：Default 預設方法，可以在任何介面上宣告，子類別無實作時呼叫。
	1. Collection.`stream`、Iterable.`forEach` 都是預設方法。
	2. 類別優先，為避免無法向後相容。繼承預設方法的優先權為：父類別 > 實作介面 > 原介面。
	3. 繼承多個介面如有相同方法時，需覆寫並使用 `super` 指定使用哪個介面的方法。如下：InterfaceName.super.methodName。
	4. 介面可以建構靜態方法。如 Stream.`of`、Stream.`iterate`。
* Optional 物件的誕生，避免了 null 所造成的錯誤，實用方法如下：
	1. Optional.`of`：工廠方法。
	2. Optional.`ofNullable`：轉換 null 為 Optional。
	3. Optional.`empty`：產生空的 Optional。
	4. `isPresent`：常用於檢查是否有現值。
	5. `get`：取得數值。
	6. `orElse`：建立替代值，`get` 沒有數值時將以此取代。
	7. `orElseGet`：在 `get` 沒有數值的時候才建立替代值，可以避免昂貴運算。
	
### Chapter 5 - 進階群集與收集器

* Lambda 表達式可以使用方法參考（method reference），比如以下範例：
	1. 方法：Object::toString。
	2. 建構式：Object::new，Object[]::new。
* 定序（ordering）：
	1. Stream 是否有序取決於來源是否有序。
	2. 有些方法可以建立接觸順序（encounter order），如 `sort`。
	3. 有時候使用 `unordered` 方法打亂順序會有更大的效能效益。但 `filter`、`map`、`reduce` 不太需要（本身很有效率）。
	4. `forEach` 是無序的，有序可以使用 `forEachOrder`。
* 收集器（collector）
	* 將 stream 匯出某種結果：
		1. 結構：其實作不需要指定，底層會判斷。如 `toList()`、`toSet()`、`toCollection()`。指定實作範例：`toCollection(TreeSet::new)`。
		2. 數值：依照邏輯計算出結果。如 `minBy(comparing)`、`maxBy(comparing)`、`averagingInt(Function)`。
		3. 分組：將資料依照某種分類法分組。如 `partitionBy(Predicate)`、`groupBy(Function)`。
		4. 字串：將元素串接成字串。如 Collectors.`joining(seperator, prefix, suffix)`。
	* 組裝收集器：`groupBy(Function, downstream)`，downstream 稱為下游收集器，能夠處理分組的資料。例如：
		1. groupBy(Function, counting())：計算每個分組其元素個數。
		2. groupBy(Function, mapping(Supplier, Supplier -> List))：利用 mapping 收集器處理更複雜的結構。
	* 自訂收集器：
		* 可以實作 Collector<A,B,C> 介面：
			1. A 代表收集的元素型態。
			2. B 代表積累器型態。
			3. C 代表結果型態。
			4. Collector 介面的覆寫方法介紹：
				1. supplier：建立容器的方法。
				2. accumulator：將元素整理到收集器的方法。
				3. combiner：將容器合併的方法。
				4. finisher：產生最終結果的方法。
		* 使用 Colletor.reducing 實作。
* Map 提供了幾個實用方法如下：
	1. `compute`：計算數值。
	2. `computeIfPresent`：如果數值存在則計算。
	3. `computeIfAbsent`：如果數值不存在則計算。
	4. `forEach(BiComsumer)`來取代傳統的 entrySet 使用方式。
	
### Chapter 6 - Parallelism 		

* Parallelism（平行）：同個時段上同時執行。
* Concurrency（並行）：同個時段上交互執行。
* 在平行的時候，其合併函式必須符合結合律（associative），譬如 (x + y) + z = x + (y + z)。
	- 切割無數個分段不會影響最終結果才能執行平行。
* 呼叫 `sequential()` 循序執行，呼叫 `parallel()` 平行執行。
* 呼叫平行的時候，如果使用鎖定可能帶來麻煩。不需要執行此操作，Stream 會處理。
* 考慮平行因素：
	1. 資料量大小。
	2. 運算邏輯複雜度，複雜度影響每個元素處理的時間。
	3. 資料是否封箱，基本型態比封箱型態快。
	4. 可用核心數，參考 Runtime.getRuntime().availableProcessors() 方法。
	5. 資料結構 ArrayList、IntStream.range > HashSet、TreeSet > LinkedList、Stream.iterate、BufferedReader.lines。
* 平行的速度也基於方法是否有狀態：有狀態操作為了維持狀態得耗費相對資源。
	* 有狀態方法：`sorted`、`distinct`、`limit`。
	* 無狀態方法：`map`、`flatMap`、`filter`。
* Arrays 的方法：
	1. `parallelPrefix`：使用給定的函式對陣列值進行累計。
	2. `parallelSetAll`：使用 lambda 表達式更新陣列中的值。
	3. `parallelSort`：平行地對元素進行排序。
	
Chapter 7 - Debug and Testing

* `peek()`方法不會中斷 Stream，可用來顯示一些中介資訊。
* 不測試 Lambda，而是將當中的業務邏輯抽離出來測試。
* 如果某 Stream 流程相似且散落各地，將其抽離出來成高階函式。

Chapter 8 - Pattern and Principle

* 諸多設計模式在結構方面存在著介面函式的構造，Lambda 可以取代之。
* Lambda 的特性像是外部迭代，方法參考都遵守了 SRP。
* 藉由設計高階函式可以輕鬆達到 OCP、DIP。

Chapter 9 - Lambda's concurrency

* Vertx：
	* extends Verticle as servlet，`start()` 類似於 `main()` 方法，為開始執行的入口。
	* example：
	```Java
		public void start() {
			vertx.createNetServer()
				 .connectHandler(socket -> {
					container.logger().info("socket connected");
					socket.dataHandler(new User(socket, this));
				 }).listen(10_000);
			container.logger().info("ChatVerticle started");
		}
	```
	* extends Handle<T> 為 dataHandler 的 argument。其 `handle(Buffer buffer)` 方法為處理訊息的邏輯，buffer 為傳遞的訊息。
	* EventBus 物件可以以非阻斷方式傳遞訊息，`send(位置, 訊息)`, `publish(位置, 訊息)`, `registerHandler(位置, Comsumer<T>)`。彼此無共用狀態。
* CompletableFuture：
	* IOU（I owe you），處理某個邏輯的表示式，使用特定方法取得結果。例如 Future 物件的 `get()`，發生阻斷取得結果。
	* CompletableFuture 以似 Stream 方法將 IOU 管線化，得以使用並行達到非阻斷效果。
		* `thenCompose`：接續動作，以上一個 IOU 結果作為引數接續下去。
		* `thenCombine`：合併動作，合併另一個 CompletableFuture，並將兩者的 IOU 結果作為引數接續下去。
		* `thenApply`：如果要將 IOU 結果有 Stream.`map` 效果，使用之。
		* `thenRun`、`thenAccept`：接續不回傳值得區塊使用，類似 Comsumer、Runmable。
		* `exceptionally`：例外而結束時，需要邏輯處理。
		* `handle`：同時考量正常與例外的 Stream.`map`。
		* `complete`、`completeExceptionally`：設置正常結束與例外結束的結果，只能挑一種。
		* `isDone`、`isCompletedExceptionally`：可察看目前的處理狀態。
		* `join`：無例外受檢的取得結果。
	* CompletableFuture.supplyAsync(Supply[, service])：CompletableFuture 工廠方法，`service` 可選擇指定的 ExecutorService 執行。預設使用平行的 Fork/Join 執行緒池執行。
* RxJava：
	* 將 CompletableFuture 的單一值概念擴展到整個 Stream 流。
	* 應用 Observer Pattern 使得能夠隨著事件的變化而改變。
		* `onError`：例外結束。
		* `onNext`：事件流的成員。
		* `onCompleted`：標示 Observable 結束。
		* `take`：限制筆數。