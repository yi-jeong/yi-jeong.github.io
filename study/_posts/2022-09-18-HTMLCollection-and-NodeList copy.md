---
layout: post
title: HTMLCollection 와 NodeList 에 대하여
categories: [Study]
tags: [JS]
excerpt_separator: <!--more-->
---


> 들어가기에 앞서 DOM을 먼저 설명한다.  

- - -
  
### DOM (document object model) 이란?

자바스크립트는 html 자체를 읽을 수 없으므로, 자바스크립트에서 읽을 수 있게 바꿔줘야한다.  
`DOM API`는 html 의 태그들을 웹 브라우저의 메모리에 올려 js에서 조작할 수 있게 해준다.  
js에서 읽을 수 있게 객체가 된 데이터는 DOM의 노드가 된다.  

​
`DOM`의 접근 메소드는 여러가지가 있는데 이것은 인터넷에 서치하면 나오는 정보니 생략하겠다.  
`DOM`을 메소드로 접근을 하게 되면 js에서 데이터를 반환하는데  
getElementById 같은 단일 데이터의 경우 리턴 타입은 `HTMLLIElement` 가 된다.  
허지만 여러개의 데이터를 반환하는 querySelector나 getElementByClassName 같은 경우는 
유사배열 형태로 데이터를 리턴한다.  
*(아놔 배열이면 배열이지 유사배열은 대체 뭐람--;;?)*  
css 선택자 api 로 호출한 데이터의 경우는 `NodeList` 형태로 리턴하고, 
클래스나 태그로 호출한 데이터는 `HTMLCollection` 형태로 리턴한다.  

<!--more-->

이 둘은 유사배열이기 때문에 map 이나 reduce같은 함수를 사용하지는 못한다.........

그러나 `NodeList`에는 forEach 함수가 내장되어있고, `HTMLCollection`은 lenght 메소드를 이용해 for문을 돌려 item 메소드로 하나하나 꺼내주는 방법이 있다. 배열로 변환해 꺼내주고 싶다면 array.property.from 을 이용하면 된다. 

<br>
<br>

### 그래서 HTMLCollection 와 NodeList 의 차이가 뭔데?  

`HTMLCollection`은 실시간으로 반영이 되는 라이브 컬렉션이고, `NodeList`은 돔이 변경되어도 처음 읽었던 데이터 그대로 유지되는 정적 컬렉션이다.   
(childNodes로 반환한 경우 NodeList는 라이브 컬렉션으로 동작한다.)  
⭐️

이 둘의 차이는 아래 예시로 설명하겠다.
```html
<div class="test">테스트1</div>
<div class="test">테스트2</div>
<div class="test">테스트3</div>
<div class="test">테스트4</div>
```
html 에 같은 클래스를 가진 4개의 요소를 작성한다.

<br>

#### **HTMLCollection** 살펴보기 

```js
const classnameArray = document.getElementsByClassName("test");
```

getElementsByClassName 메소드를 이용해 하나의 상수를 만든다. 클래스네임으로 불러왔기 때문에 리턴값은 HTMLCollection이다.  

```js
for( let el of classnameArray ){
  el.classList.remove("test");
  console.log(classnameArray.length);
}
```

그리고 해당 상수에 들어있는 모든 객체의 class에 들어있는 test를 없애준다.  
그리고 콘솔에 찍힌 결과값을 보자.  

```js
3
2
```

0까지 도달하지 않았다.

HTMLCollection은 라이브 컬렉션이기 때문이다.  
classnameArray 의 length 값은 3(0에서 부터 시작하므로)인데 한 요소의 test 클래스를 지운 것이 실시간 반영으로 인해 2가 되어버린 것이다.  
인덱스의 값은 0에서 시작하니 인덱스가 2가 될 때 classnameArray 의 length 값은 1이 되어버려 for 문의 조건이 맞아버리지 않으니 더이상 동작하지 않게 되는 것이다.  
NodeList 의 경우는 어떨까?  

<br>

#### **NodeList** 살펴보기 

```js
const queryselectorArray = document.querySelectorAll(".test");
```

상수를 하나 생성해서,  

```js
for( let el of queryselectorArray){
  el.classList.remove("test");
  console.log(queryselectorArray.length);
}
```

for 문을 돌린 결과값은

```js
4
4
4
4
```

이다.  
정적 컬렉션이기 때문에 해당 상태에 변화가 일어나도 처음 집었던 length 의 값을 그대로 유지하는 것이다.  

HTMLCollection은 name 이나 id, index 값으로 데이터를 불러 올수가 있지만, NodeList 는 오직 index 로만 데이터를 가져올 수 있기에  
처리속도는 HTMLCollection가 빠르다고 한다. 그러니 정적 컬렉션을 꼭 활용할 일이 아니라면 라이브 컬렉션을 활용하는 게 좋겠다. 
  
상황에 맞게 잘 쓰도록 하자.  