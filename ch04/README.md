# React-learning

***
## 學習筆記
***

**ch4. State 和生命週期**

* **學習到的知識**
  * 1.練習封裝
  * 2.轉換 Function 成 Class
  * 3.加入 Local State 到 Class
  * 4.加入生命週期方法到 Class
  * 5.關於State
  * 6.為什麼State會稱為local state或是封裝



***
### 1.練習封裝
***

```java
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

```java
function Clock(props){
	return (
		<div>
      		<h1>Hello, world!</h1>
      		<h2>It is {props.date.toLocaleTimeString()}.</h2>
    	</div>
	);
}

function tick() {
  ReactDOM.render(
  	<Clock date= {new Date()} />,
  	document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

> 這樣的話，是將Clcok這個function component每秒呼叫並更新一次

> 但是，它缺少了一個重要的需求：Clock 設定 timer 並在每秒更新 UI 應該是 Clock 物件裡面實作的內容(更新秒數)才對。

```java

<Clock date= {new Date()} />,

<Clock />,

```

因此，希望是用上述的第二種方式，只建立一個Date物件，然後更新的是該物件的方法

> state 會是能夠達成目標的手段，要使用state必須將function定義成class，這樣就能擁有local state的屬性控制


***
### 2.轉換 Function 成 Class
***

* **步驟**
  * 1.建立一個相同名稱並且繼承 React.Component 的 ES6 class。
  * 2.加入一個 render() 的空方法。
  * 3.將 function 的內容搬到 render() 方法。
  * 4.將 render() 內的 props 替換成 this.props。
  * 5.刪除剩下空的 function 宣告。


```java
class Clock extends React.Component{
	render() {
		return (
			<div>
	      		<h1>Hello, world!</h1>
	      		<h2>It is {this.props.date.toLocaleTimeString()}.</h2>
	    	</div>
		);
	}
}

function tick() {
  ReactDOM.render(
  	<Clock date={new Date()}/>,
  	document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

***
### 3.加入 Local State 到 Class
***

* **步驟**
  * 1.將 render() 方法內的 this.props 替換成 this.state
  * 2.加入一個 class constructor 並分配初始的 this.state：
  	* 2-a. constructor(props)
  	* 2-b. 裡面super(props) 呼叫繼承的Class constructor
  	* 2-c. 利用this.state 設定查看狀態的參數，這裡是date，並給予值new Date()
  * 3.移除 ReactDOM.render 那裡2-c給過的值


```java
class Clock extends React.Component{
	constructor(props){
		super(props);
		this.state = {date: new Date()};
	}

	render(){
		return (
			<div>
	      		<h1>Hello, world!</h1>
	      		<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
	    	</div>
		);
	}
}

function tick() {
  ReactDOM.render(
  	<Clock />,
  	document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

***
### 4.加入生命週期方法到 Class
***

```java
class Clock extends React.Component{

	constructor(props){
		super(props);
		this.state = {date: new Date()};
	}

	componentDidMount(){
		this.timerId = setInterval(
			() => this.tick(), 1000
		);
	}

	componentWillUnmount(){
		clearInterval(this.timerId);
	}

	tick(){
		this.setState({
			date: new Date()
		});
	}

	render(){
		return(
			<div>
	      		<h1>Hello, world!</h1>
	      		<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
	    	</div>
		);
	}
}

ReactDOM.render(
	<Clock />,
	document.getElementById('root')
);
```
> 當Clock標籤被ReactDOM.render()傳入，會建立Clock物件並呼叫constructor，此時this.state就會被設定當下的時間(透過 new Date())

> ReactDOM.render()獲得內容(Clcok class 的 render回傳)之後，將它顯示在螢幕上

> 每當Clcok要輸出時，React會呼叫生命週期方法componentDidMount()，裡面設定了timer並呼叫tick()方法內的

> 1000就是一秒，每一秒透過setState方法，取得當下時間

> 然後react的特性，state一改變就會呼叫render()，將改變的時間顯示在螢幕上


***
### 5.關於State
***

**請不要直接修改 State**

```java
// 錯誤
this.state.comment = 'Hello';
```

```java
// 正確
this.setState({comment: 'Hello'});
```

> 請使用setState修改，如果你要用指定的方式請在constructor

***
**State 的更新可能是非同步的**

```java
// 錯誤
this.setState({
  counter: this.state.counter + this.props.increment
});
```

> 這個程式碼可能無法更新 counter，因為你不知道this.state.counter和this.props.increment會不會同步更新

> props是function的參數，state是component的私有屬性

```java
// 正確
this.setState((state, props) => ({
	counter: state.counter + props.increment
}));
```

```java
// 正確
this.setState(function(state, props){
	return {
		counter: state.counter + props.increment
	};
});
```

***
**State 的更新將會被 Merge**

```java
constructor(props){
	super(props);
	this.state = {
		posts: [],
		comments: []
	};
}
```

```java
componentDidMount(){
	fetchPosts().then(response => {
		this.setState({
			posts = response.posts
		});
	});

	fetchComments().then(response => {
		this.setState({
			comment = response.comments
		});
	});
}
```

> 可以在constructor設定多個state

> componentDidMount() 可以對state個別進行setState()，意思是comment因為fetchComments()的關係設定成response.comments，但是不影響posts

***
### 6.為什麼State會稱為local state或是封裝
***

父類別 和 子類別 component 不會知道某個 component 是 stateful(有狀態) 或 stateless(無狀態) 的 component，而且不在意component是用function還是class定義。
這就是 state 通常被稱為 local state 或被封裝的原因。除了擁有和可以設定它之外的任何 component 都不能訪問它。


> stateful component: 含有state的變數的component，可以存放各種型態的狀態資料，而且可以被子物件使用

> stateless component: 沒有state的變數的component，單純的接收狀態並在螢幕繪製出來

Component可以選擇哪個state作為props往下傳(向下資料流)，不能向上傳(想像state是水龍頭，水只能向下流)

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch04/downdata.jpg)




