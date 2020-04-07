# React-learning

***
## 學習筆記
***

**ch6. 條件 Render**

在 React 中，你可以建立不同的 component 來封裝你需要的行為。接著，你可以根據你的應用程式的 state，來 render 其中的一部份

* **學習到的知識**
  * 1.暖身(簡單的if判斷)
  * 2.將參數傳給 Event Handler
  * 3.Inline If 與 && 邏輯運算子
  * 4.Inline If-Else 與三元運算子
  * 5.防止 Component Render
  

***
### 1.暖身(簡單的if判斷)
***

```java
function Greeting(props){
	const isLogin = props.isLogin;
	if(isLogin){
		return <h2>Hi Sakura Miko</h2>;
	}else{
		return <h2>Hi!</h2>;
	}
}

ReactDOM.render(
	<Greeting isLogin = {true}/>,
	document.getElementById('root')
);

```

> function component接收的參數，記得用 props.參數 接收

> function component別忘記名稱首字要大寫

***
### 2.登入登出按鈕
***

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch06/login_logout.jpg)

```java
class LoginControl extends React.Component{
	constructor(props){
		super(props);
		this.state = {isLogin: false};
		this.handleLoginClick = this.handleLoginClick.bind(this);
		this.handleLogoutClick = this.handleLogoutClick.bind(this);
	}

	handleLoginClick(){
		this.setState({isLogin: true});
	}

	handleLogoutClick(){
		this.setState({isLogin: false});
	}

	render(){
		const login = this.state.isLogin;
		let button;

		if(login){
			button = <button onClick={this.handleLogoutClick} />
		}else{
			button = <button onClick={this.handleLoginClick} />
		}

		return(
			<div>
				<Greeting isLogin = {login} />
				{button}
			</div>
		);
	}
}

function Greeting(props){
	const isLogin = props.isLogin;
	if(isLogin){
		return <h2>Hi Sakura Miko</h2>;
	}else{
		return <h2>Hi!</h2>;
	}
}

ReactDOM.render(
	<LoginControl />,
	document.getElementById('root')
);
```

> 要關注的是isLogin，因此this.state = {isLogin: false}; 預設為未登入

> class LoginControl 內有兩個方法，分別處理登入與登出時isLogin的狀態

|   | const | var | let |
|---|---|---|---|
| 作用域 | block scoped | function scoped | block scoped |
| 重複宣告? | 不可以 | 可以 | 不可以 |

> 雖然 let button; 可以換成 var button; ，但在react裡優先使用ES6的let


***
### 3.Inline If 與 && 邏輯運算子
***

**定義一個常數message，裡面有3個成員的陣列，目標要顯示你有3則為獨的訊息**

```java
function Mailbox(props){
	const unreadMessage = props.unreadMessage;
	return (
		<div>
			{unreadMessage.length > 0 && 
				<h2>
					You have {unreadMessage.length} unread message.
				</h2>
			}
		</div>
	);

}

const message = ['React', 'Re: React', 'Re:Re: React'];

ReactDOM.render(
	<Mailbox unreadMessage={message} />,
	document.getElementById('root')
)

```

> function component Mailbox 傳入 message 參數，透過 unreadMessage 接收

> return 內 判斷 unreadMessage.length > 0 ，這裡大於0所以是true，&&後面跟著jsx敘述，React會回傳jsx敘述

|   | 敘述1 | 敘述2 | React |
|---|---|---|---|
| && | true | jsx | jsx | 
| && | false | jsx | 跳過 |

***
### 4.Inline If-Else 與三元運算子
***

```java
render() {
  const isGive = this.state.isGive;
  return (
    <div>
      Cindy <b>{isGive ? 'give' : 'not give'}</b> an apple.
    </div>
  );
}
```

> 三元運算子---變數 ? 變數true : 變數false

當然也可用在jsx的判斷上

```java
render() {
  const isGive = this.state.isGive;
  return (
    <div>
      {isGive
        ? <NotGiveButton onClick={this.handleGive} />
        : <GiveButton onClick={this.handleNotGive} />
      }
    </div>
  );
}
```

***
### 5.防止 Component Render
***

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch06/avoidCR.jpg)

```java
function WarningBanner(props){
	if(!props.warn){
		return null;
	}

	return(
		<div className="warning">
			Warning!
		</div>
	);
}

class Page extends React.Component{
	constructor(props){
		super(props);
		this.state = {showWarning: true};
		this.handleToggleClick = this.handleToggleClick.bind(this);

	}

	handleToggleClick(){
		this.setState(state => ({
			showWarning: !state.showWarning
		}));
	}

	render(){
		return(
			<div>
				<WarningBanner warn={this.state.showWarning} />
				<button onClick={this.handleToggleClick}>
					{this.state.showWarning ? 'Hide' : 'Show'}
				</button>
			</div>
		);
	}
}

ReactDOM.render(
	<Page />,
	document.getElementById('root')
);
```

> function component WarningBanner 當 warn 是 flase 時，回傳null，因此 class Page的render內<WarningBanner warn={this.state.showWarning} />會消失
