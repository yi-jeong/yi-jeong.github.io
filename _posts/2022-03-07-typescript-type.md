---
layout: post
title: 타입 구성🔧과 타입 정의🗡 에 대하여
categories: [Study]
tags: [typescript, React.js]
---

### 📌 타입스크립트를 사용하는 이유

* **컴파일 단계에서 에러를 확인 할 수 있다.**  
  - ❗️동적인 자바스크립트를 정적으로 사용 할 수 있다.  
  - ❗️디버깅이 쉬워짐  

* **IDE 자동완성 기능**  
  - ❗️작업시간을 단축 시킬 수 있다.  

​
- - -

### 📌 타입 정의하기


```js
Const test = {
  id:0,
  name: “Merry”,
  kind: “dog”
}
```

이런 변수가 있다고 가정해보자. 🤔  
이 변수는 id 영역에 string이 들어갈 수도 있고 number가 들어갈 수도 있다.  
id 에 number 만 들어갈 수 있게 설정해주고 싶다면 타입스크립트를 이용해 타입을 정의해주면 된다 😄  


객체의 형태를 명시적으로 나타내기 위해서 interface를 사용한다.  
Interface 대신 class를 사용할 수도 있다. 하지만 interface를 활용하도록 하자‼️  

```js
Interface Test {
  id: number;
  name: string;
  kind: string;
}
```

interface로 객체의 형태를 지정해주었다.

```js
Class TestClass {
  id: number;
  name: string;
  kind: string;

  constructor(id: number, name: string, kind: string){
    this.id = id;
    this.name = name;
    this.kind = kind;
  }

}
```

class로 선언할 경우엔 이렇게 활용할 수 있다.  
변수 선언 뒤에 interface로 선언한 타입명을 적어주면 된다.  

```js
Const test: Test = {
  id:0,
  name: “Merry”,
  kind: “dog”
}
```





- - -


### 타입구성

* **유니온 (Unions)**
  유니온은 자바스크립트의 or 연산자 (||) 와 같이 'A이거나 B이다' 란 의미이다.  
  타입을 여러개 지정해놓고 그 중 하나일 수 있음을 선언한다.  

```js
type Kind = “dog” | “cat” | “etc”;
```
​

**TIP🌟**  
Typeof 로 변수의 타입을 검사할 수 있다.  
검사 할 수 있는 타입은 아래와 같다.  

```js
String, boolean, undefined, number, function
```
​
- - -

* **제네릭 (Generics)**  
  제네릭은 여러 데이터 타입에 대해 동일하게 동작할 수 있게 해주는 기능이다.  
  주로 여러 가지 타입에서 동작하는 컴포넌트를 생성하는데 사용한다.  

```js
Function Test( text ){
  return text;
}

Test(’string’);
Test(5);
Test(true);
```

어떤 값을 넘겨줘도 그대로 반환한다.  
각자 타입을 지정해준다면 다음과 같이 코드를 짜야한다.   

```js
Function TestString( text: string ): string{
  return text;
}

Function TestNumber( text: number ): number{
  return text;
}

Function TestBoolean( text: boolean ): boolean{
  return text;
}
```

코드는 동일하나 데이터 타입이 다르기에 타입에 따른 함수를 각각 만들어주었다.  
참 비효율적인 코드가 아닐 수 없다. 🤔 이럴 때 제네릭을 사용하면 편하다.  
제네릭을 사용해서 데이터를 넘겨보도록 하자.  

​
```js
Function Test<T>( text: T ): T{
  return text;
}

Test<string>(’string’);
Test<number>(5);
Test<boolean>(true);
```

제네릭은 이런 방식으로 사용하면 편하다 ✌️  