# React-learning

***
## 學習筆記
***

**建立 React 應用程式最小的單位是 element。**

* **學習到的知識**
  * 1.Render Element 到 DOM 內
  * 2.更新被 Render 的 Element



***
### 1.Render Element 到 DOM 內
***

```java
<div id="root"></div>
```

```java
const element = <h1>Hello World!</h1>
ReactDOM.render(element, document.getElementById('root'));
```

> 假設在html檔裡有div的標籤，並且id命名root

> 可以在js檔建立一個變數，然後利用ReactDOM.render(變數, document.getElementById('root'));，把剛剛的變數內容寫進id命名成root的div標籤

***
### 2.更新被 Render 的 Element
***

```java
function tick(){
	const element = (
		<div>
			<h1>Hello World!</h1>
			<h2>It is {new Date().toLocaleTimeString()}.</h2>
		</div>
	);
	ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

> 利用setInterval設定秒數呼叫function，讓每次呼叫都ReactDOM.render

> new Date().toLocaleTimeString() 可以顯示時:分:秒，Locale不要拼錯字

> 打開開發工具，發現react只更新有變動的內容。
