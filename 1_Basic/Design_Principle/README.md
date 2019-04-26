Design Principle
===========================
設計原則大概可以用一句話來描述：程式設計界的聖經！程式設計師不一定要知道設計原則，但知道設計原則並實踐的一定是好的程式設計師。

* 軟體界的三大原則：
	* DRY：Don't Repeat Yourself（不重複你自己）：重複的邏輯應該抽離出來，而不是發散各地。
	* KISS：Keep It Simple And Stupid（保持簡單愚蠢）：盡可能的簡單化，而不是愈發複雜。
	* YAGNI：You Ain't Gonna Need It（你不需要它）：設計到符合需求即可，而不是過度設計。
* 物件導向 SOLID 原則：
	* Single responsibility（單一職責）：每個物件應該只需負責一項職責（功能），過多的職責會使耦合過多。
	* Open-closed（開放－封閉）：開放擴展，封閉修改。在新增功能的時候是新增物件，而不是在原有物件添加新功能。
	* Liskov substitution（里式替換）：父類別應該隨時能夠被子類別所替換，亦即在一個模組中，好的抽象設計能夠讓父類別被複用，易於擴展。
	* Interface segregation（介面隔離）：不同的邏輯應該要有不同且具體的介面，而不是全部都依賴一個介面造成耦合。
	* Dependency inversion（依賴反轉）：高階與低階模組皆應依賴抽象，並根據抽象來進行設計。
* 其他原則：
	* 職責分配原則（General Responsibility Assignment Software Principle,  GRASP）：探討物件之間的耦合關係要如何設計。
	* 合成聚合原則（CARP Principle）：**擁有**某一物件比**繼承**某一物件有較低的耦合。
	* 迪米特法則（又稱最少知識原則 Law of Demeter, LoD）：描述物件之間如何達成鬆耦合，如存取權限降低（public → private）、使用中介者當媒介。
	





