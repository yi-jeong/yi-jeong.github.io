---
layout: post
title: map 과 reduce 를 공부해요 👻
categories: [Study]
tags: [JS]
---

### 자바스크립트 map 과 reduce는 어떤 상황에 쓰일까?  

map과 reduce 는 배열을 다루는 메소드다.  
어떤 배열에 무언가 추가를 하고 싶다거나, 어떠한 요소를 빼고 사용하고 싶을 때 사용하지만 기존에 있는 배열을 수정해서 쓰는 것이 아닌  새로운 배열을 뱉어낸다. 
기존 데이터는 소중하니까요 👻  
filter 메소드도 배열을 다루는데 해당 메소드는 다음 기회에 포스팅하기로 하고..  

- - -


### Map의 구조  

``` js
배열.map((요소,인덱스,배열)=>{ return 요소});
```

```js
Const array = [1,2,3,4,5]
Let mapArray = array.map( function(el){
   return el;
})
```

Map은 array에 들어있는 배열 수 만큼 순회한다.
그리고 기존 배열을 사용하는 척 mapArray에 새로운 배열을 만들어낸다 😮 ‼️ 
배열을 복사하는 것과 같다고 볼 수 있다.

```js
const array=[1,2,3,4,5];
let mapArray=array.map(function(el,index,arr){
return (
  {
    el: el,
    index: index,
    arr: arr
  }
)
});
```

그리고 array 데이터를 참고해 리턴값에 따른 새로운 배열을 생성해준당  
map으로 우리가 원하는 생김새의 배열을 만들 수 있다 ✌️  
Map 의 경우, push를 하지 않아도 알아서 리턴값을 받아 배열을 만들어 준다  
배열 갯수도 동일하게 들어오니 배열 내 요소를 바꿔서 사용하고 싶을 때 쓰면 좋을 거 같다 😎  

<br>

- - -

### reduce의 구조  

```js
배열.reduce((누적값, 현잿값, 인덱스, 요소))=>{ return 결과 }, 초깃값); 
```

reduce는 자세히 알아보기 전 단순히 더하는 메소드라고만 생각했는데 공부를 하고나니 활용성이 높은 메소드였다 😮  
reduce로 map의 기능을 구현할 수 있다‼️  
데이터를 누적하는 메소드라고 보면 되겠다  


```js
const array=[1,2,3,4,5];
let mapArray=array.reduce(function(acc, cur, i){
if(cur % 2){
  acc.push(cur);
}
  return acc;
},[]);
```

초기값을 []로 설정해주었으니 []안으로 데이터를 쌓아준다.  
배열 갯수만큼 순회해 cur 자리에 데이터가 하나씩 순서대로 들어간다 🥺  
그리고 if 문을 이용해서 조건이 맞다면 acc 에 해당 데이터를 넣으라고 코드를 작성해주었다.  
앗! 이렇게 쓰면 map과 filter를 쓰는 것과 같은 동작을 한다 😮  

map은 종종 사용해봤지만 reduce는 생소한 메서드였다..  
이렇게 활용하기 좋은 메소드를 이제서야 공부하다니 자바스크립트 공부는 끝이 없다 😢  

Json 데이터를 다룰 때 많이 쓰는 map 과 reduce 를 정리해보았다.  
api 고수가 되는 그날까지 열심히 공부해야지 🥺



