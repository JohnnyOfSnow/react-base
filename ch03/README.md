# React-learning

***
## 學習筆記
***

**Component 使你可以將 UI 拆分成獨立且可複用的程式碼**

* **學習到的知識**
  * 1.Function Component 與 Class Component
  * 2.Render 一個 Component
  * 3.組合 Component
  * 4.抽離 Component



***
### 1.Function Component 與 Class Component
***

```java
function welcome(props){
	return <h1>Hello {props.name}</h1>
}
```

```java
class Welcome extends ReactDOM.Component {
	render(){
		return <h1>Hello {this.props.name}</h1>
	}
}
```

> 前者接受props(屬性 property)，然後回傳一個react element，這種寫法稱為function component，因為它本身是javascript function

> 後者是用ES6 Class定義component，因此稱為Class component

> 兩者在react是同等的

***
### 2.Render 一個 Component
***

```java
const element = <Welcome name="Sara" />;

function Welcome(props){
	return <h1>Hello {props.name}</h1>
}

ReactDOM.render(
	element,
	document.getElementById('root')
);
```

> element呼叫了ReactDOM.render()

> React以{name: 'Sara'}作為props傳入了Welcome component並呼叫

> Welcome component 回傳 Hello Sara

> ReactDOM 將DOM更新成Hello Sara

**注意: component名要大寫，因為react會把小寫視為原始DOM標籤**

> 小寫字會被指向內建的component，例如：span, div等，大寫開頭字會被編譯成React.createElement(名稱)並且在你的 JavaScript 檔案裡對應自定義或導入的 component。

***
### 3.組合 Component
***

```java
function Welcome(props){
	return <h1>Hello {props.name}</h1>
}

function App(){
	return (
		<div>
			<Welcome name='Sara' />
			<Welcome name='Candy' />
			<Welcome name='Crush' />
		</div>
	);
}

ReactDOM.render(
	<App />,
	document.getElementById('root')
);
```

> App標籤會去找定義的component，然後它會回傳ReactDOM該更新的內容

> React 應用程式都有一個最高層級的 App component

***
### 4.抽離 Component
***

```java
function Comment(props){
	return (
		<div className='Comment'>
			<div className='userInfo'>
				<img className='Avatar' 
					src= {props.author.avatarUrl}
					alt= {props.author.name}
				/>
				<div className='userInfoName'>
					{props.author.name}
				</div>
			</div>
			<div className='Comment-text'>
				{props.text}
			</div>
			<div className='Comment-date'>
				{formatDate(props.date)}
			</div>
		</div>
	);
}
```


```java
function Comment(props){
	return (
		<div className='Comment'>
			<UserInfo user={props.author} />
			<div className='Comment-text'>
				{props.text}
			</div>
			<div className='Comment-date'>
				{formatDate(props.date)}
			</div>
		</div>
	);
}

function Avatar(props){
	return (
		<img className='Avatar' 
			src= {props.user.avatarUrl}
			alt= {props.user.name}
		/>
	);
}

function UserInfo(props){
	return (
		<div className='userInfo'>
			<Avatar user={props.user} />
			<div className='userInfoName'>
				{props.author.name}
			</div>
		</div>
	);
}
```

> component 抽離出來可能是一件繁重的工作，但是在較大的應用程式中，抽離出來並可以重複使用的 component 很值得。

> 自己多練習幾次

***
### 5.Props 是唯讀的
***

```java
function sum(a, b) {
  return a + b;
}
```

> a,b值不會改變，而且還回傳一樣的結果，這種稱為pure function


```java
function withdraw(account, amount) {
  account.total -= amount;
}
```

> account值會因為function執行而改變(改變了account.total)，所以這不是pure function

**react規定: 所有的 React component 都必須像 Pure function 一般保護他的 props**