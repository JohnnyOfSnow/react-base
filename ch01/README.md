# React-learning

***
## 學習筆記
***

**ch1. JSX 介紹**

* **學習到的知識**
  * 0.起手式
  * 1.JSX是什麼，然後透過react將它寫到網頁上
  * 2.利用javascript function回傳值到JSX
  * 3.javascript function可以回傳JSX
  * 4.JSX可以指定屬性
  * 5.JSX 防範注入攻擊
  * 6.JSX 表示物件


***
### 0.起手式
***

```java
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```


***
### 1.JSX是什麼，然後透過react將它寫到網頁上
***

```java
const name = 'Sakura Miko';
const element = <h1>Hello, {name}</h1>

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 利用ReactDOM.render(); ，將JSX敘述(element)render到root

> JSX是一個 JavaScript 的語法擴充(因為是在js檔案裡寫html標籤，其實Asp.net MVC的View的razor也是C#寫html標籤，概念上差不多)

> 因此JSX比較接近javascript


***
### 2.利用javascript function回傳值到JSX
***

```java
const user = {
  firstName: 'Sakura',
  lastName: 'Miko'
};

function formatName(user){
  return user.firstName + ' ' + user.lastName;
}

const element = (
  <h1>
    Hello, {formatName(user)}
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 建立一個user常數，裡面有兩個成員(名稱: '值')

> {formatName(user)}，呼叫定義的formatName(user) function 回傳值


***
### 3.javascript function可以回傳JSX
***

```java
mport './index.css';

const user = {
  firstName: 'Sakura',
  lastName: 'Miko'
};

function formatName(user){

  return user.firstName + ' ' + user.lastName;
}

function getGreeting(user){
  if(user){
    return <h2>Hello, {formatName(user)}</h2>;
  }else{
    return <h2>Hello, Stranger!</h2>;
  }
}

const element = (
  <h1>
    {getGreeting(user)}
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> getGreeting(user)裡面判斷user，return的內容可以有html tag

***
### 4.JSX可以指定屬性
***

```java
const element = <div tabIndex="0"></div>;

const element = <img src={user.avatarUrl}></img>;

const element = <img src={user.avatarUrl} />;
```

> 使用""將字串設定為屬性

> 也可以在屬性中使用大括號來嵌入一個 JavaScript expression

> 上面兩個方法只能選一個用，意思是不要用了""然後用{}

> />可以關閉空白的標籤

***
### 5.JSX 防範注入攻擊
***

```java
const title = response.potentiallyMaliciousInput;
// 這是安全的：
const element = <h1>{title}</h1>;
```

> React DOM 預設會在 render 之前 格式化(escape) 所有嵌入在 JSX 中的變數。這保證你永遠不會不小心注入任何不是直接寫在你的應用程式中的東西。所有變數都會在 render 之前轉為字串，這可以避免 XSS（跨網站指令碼）攻擊。

> XSS（跨網站指令碼）攻擊者透過有XSS漏洞的網站輸入惡意的HTML代碼，當用戶瀏覽網站時竊取Cookie、破壞網頁結構或是重定向到別的網站。

> React就是在用戶瀏覽網頁前，已經做了對html的編碼，然後才填入html的內容(字串)，故XSS攻擊者即便輸入指令也被當成字串看待，而不會視作javascript。


* **[【網頁安全】給網頁開發新人的 XSS 攻擊 介紹與防範](https://forum.gamer.com.tw/Co.php?bsn=60292&sn=11267)**

***
### 6.JSX 表示物件
***

```java

const element = (
  <h1 className="greeting">
    Hello World!
  </h1>
);


const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello World'
);


// 注意：這是簡化過的結構
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

> 第一組和第二組是一樣的，它們會被react編譯而生成第三組的react物件(React element)

> React 會讀取這些物件並用這些描述來產生 DOM 並保持他們在最新狀態

# react-base
