# React-learning

***
## 學習筆記
***

**ch5. 事件處理**

* **學習到的知識**
  * 1.React element 跟 DOM element 在語法上的差異
  * 2.將參數傳給 Event Handler
  * 3.自我練習:counter

***
### 1.React element 跟 DOM element 在語法上的差異
***

```java
//DOM element
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

```java
//React element
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

> 注意React element的onClick接收的參數，它沒有()，這就表示定義activateLasers class的時候，在constructor要綁定這個方法

> 另外一個差異是，在 React 中，你不能在 HTML DOM 中使用 return false 來避免瀏覽器預設行為。你必須明確地呼叫 preventDefault。例如，在純 HTML 中，若要避免連結開啟新頁的預設功能，你可以這樣寫：

```java
//DOM element
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

```java
//React element
function ActionLink() {
  function handleClick(e) {
    e.preventDefault(); // must be called
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

> 在這裡，e 是一個綜合事件（synthetic event）

> 當使用 React 時，你不需要在建立一個 DOM element 後再使用 addEventListener 來加上 listener。你只需要在這個 element 剛開始被 render 時就提供一個 listener

底下試看看用react建立一個按鈕，上面的文字會On Off交錯出現

```java
class Toggle extends React.Component{
	constructor(props){
		super(props);
		this.state = {isToggleOn: true};

		this.handleClick = this.handleClick.bind(this); // because <button onClick={this.handleClick}>, handleClick後面沒有()，you should bind this function
	}

	handleClick(){
		this.setState(state => ({
			isToggleOn: !state.isToggleOn
		}));
	}

	render(){
		return(
			<button onClick={this.handleClick}> // you should bind handleClick function
				{this.state.isToggleOn ? 'On': 'Off'}
			</button>
		);
	}
}

ReactDOM.render(
	<Toggle />,
	document.getElementById('root')
);
```

> 請特別注意 this 在 JSX callback 中的意義。在 JavaScript 中，class 的方法預設沒有綁定（bound）。如果你忘了綁定 this.handleClick 並把它傳遞給 onClick 的話，this 的值將會在該 function 被呼叫時變成 undefined。

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch05/bind.jpg)

> 這不只在 React ，而是 function 在 JavaScript 中的運作模式。總之，當你使用一個方法，卻沒有在後面加上 () 之時（例如當你使用 onClick={this.handleClick} 時），你應該要綁定這個方法。

如果不要用bind()方法，你可以在callback 中使用 arrow function

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch05/arrow_fn.jpg)


***
### 2.將參數傳給 Event Handler
***

```java
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

> e 是一個綜合事件（synthetic event）



***
### 3.自我練習
***

**counter**

```java
class Test extends React.Component{
	constructor(props){
		super(props);
		this.state = {number: 1};
	}

	addNumber(){
		this.setState(() => ({
			number: this.state.number + 1
		}));
	}

	render(){
		return(
			<button onClick={this.addNumber.bind(this)}>
				<span>{this.state.number}</span>
			</button>
		);
	}
}

ReactDOM.render(
	<Test />,
	document.getElementById('root')
);
```

**3X3按鈕OX**


```java
class Test extends React.Component{
	constructor(props){
		super(props);
		this.state = {number: true};
	}

	addNumber(){
		this.setState(() => ({
			number: !this.state.number
		}));
	}

	render(){
		return(
			<button onClick={this.addNumber.bind(this)}>
				<span>{this.state.number ? 'O' : 'X'}</span>
			</button>
		);
	}
}

function App(){
	return(
		<div>
			<Test />
			<Test />
			<Test />
			<br />
			<Test />
			<Test />
			<Test />
			<br />
			<Test />
			<Test />
			<Test />
		</div>
	);
}

ReactDOM.render(
	<App />,
	document.getElementById('root')
);
```