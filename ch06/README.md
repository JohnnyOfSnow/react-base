# React-learning

***
## 學習筆記
***

**ch6. 條件 Render**

在 React 中，你可以建立不同的 component 來封裝你需要的行為。接著，你可以根據你的應用程式的 state，來 render 其中的一部份

* **學習到的知識**
  * 1.暖身(簡單的if判斷)
  * 2.將參數傳給 Event Handler
  

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

> 雖然 let button; 可以換成 var button; ，但在react裡先使用ES6的let


