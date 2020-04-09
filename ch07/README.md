# React-learning

***
## 學習筆記
***

**ch7. 列表與 Key**


* **學習到的知識**
  * 1. Render 多個 Component
  * 2. Key
  * 3. 用 Key 抽離 Component
  * 4. Key值在全域不必唯一
  

***
### 1.Render 多個 Component
***

```java
const numbers = [1,2,3,4,5];

const listNumber = numbers.map((number) => 
	<li>{number}</li>
)

ReactDOM.render(
	<ul>{listNumber}</ul>,
	document.getElementById('root')
);
```

> 利用javascripts map()方法一一帶入numbers的成員，並添加li tage，這樣就有5個li

> 在ReactDOM.render這裡，用ul tag 包住剛剛產生的5個li，然後顯示到畫面上

**當然你可以將建立列表寫成一個function component NumberList，來確認自己學到這裡的成果**

```java
function NumberList(props) {
	const numbers = props.numbers;
	const listItems = numbers.map((number) =>
		<li>{number}</li>
	);
	return (
		<ul>{listItems}</ul>
	);
}

const numbers = [1, 2, 3, 4, 5];

ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root')
);
```

依舊和原本一樣出現了5個li在螢幕上，不過瀏覽器的開發工具出現了警告

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch07/keyWarning.jpg)

它說在list的每一個子物件要有唯一的key prop，於是在li內加入 key={number.toString()}，利用number的值作為li的key值

```java
function NumberList(props) {
	const numbers = props.numbers;
	const listItems = numbers.map((number) =>
		<li key={number.toString()}>{number}</li>
	);
	return (
		<ul>{listItems}</ul>
	);
}

const numbers = [1, 2, 3, 4, 5];

ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root')
);
```

***
### 2. Key
***

**Key 幫助 React 分辨哪些項目被改變、增加或刪除。在 array 裡面的每個 element 都應該要有一個 key，如此才能給予每個 element 一個固定的身份：**

A.像是利用成員的值作為key

```java
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

B.自己有設定專門識別的id作為key值

```java
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

C.使用項目的索引做為 key(不建議，因為加入或減少項目時的索引變動，會讓react困惑)

```java
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```

* **[Index as a key is an anti-pattern](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)**

***
### 3.用 Key 抽離 Component
***

**定義一個常數message，裡面有3個成員的陣列，目標要顯示你有3則未讀的訊息**

```java
function ListItems(props){
	return <li>{props.value}</li>
}

function NumberList(props) {
	const numbers = props.numbers;
	const listItems = numbers.map((number) =>
		<ListItems key={number.toString()}  value={number} />
	);
	return (
		<ul>
			{listItems}
		</ul>
	);
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root')
);
```

> 一個好的經驗法則是，在 map() 呼叫中的每個 element 都會需要 key。

***
### 4. Key值在全域不必唯一
***

底下範例說明 post.id 可以在不同的array裡作為key值使用

```java
const post = [
  {id:1, title:'Hello World!', content:'Welcome to join ARK!'},
  {id:2, title:'Installation', content:'You can install ARK from Steam.'}
];

function Blog(props){
  const sidebar = (
    <ul>
      {props.posts.map((post) => 
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );

  const content = (
    <ul>
      {
        props.posts.map((post) =>
          <li key={post.id}>
            {post.content}
          </li>
      )}
    </ul>
  );

  return(
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

ReactDOM.render(
  <Blog posts = {post}/>,
  document.getElementById('root')

);
```

**Key 的功能是提示 React，但它們不會被傳遞到你的 component。如果你在 component 中需要同樣的值，你可以將這個值用一個不同的名稱作為 prop 傳下去**

```java
// 以前一個範例為例，post裡有 id , title , content 可以傳

<MyPost myId={post.id} Key={post.id} myTitle={post.title} myContent={post.content}>

```

> 你可以自定義function component or class component然後取不同名子接收post的參數(可以回去複習第四章提及的向下資料流概念)

> 注意：不能用props.key，當然post.key也是不行

***
### 5. 在 JSX 中嵌入 map()
***

![image](https://github.com/JohnnyOfSnow/react-base/blob/master/ch07/mapInJSX.jpg)
