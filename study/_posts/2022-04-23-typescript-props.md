---
layout: post
title: typescript를 사용한 React props 넘기기 😕
categories: [Study]
tags: [typescript, React.js]
---


정말........ 타입스크립트는 신경써주어야 할 것이 많다......  
분명 자바스크립트인데 자바를 쓰고있는 느낌 😑  

---

부모 컴포넌트에서 자식 컴포넌트로 props에 값을 넘겨준다는 건 모두가 아는 기본이니 생략하고 ‼️‼️  
필자는 자식에서 어떠한 조작을 해 부모의 값을 바꿔주고 싶었다 😑  
setTypeCode를 넘겨줄 수도 있었으나 함수를 전달하는 방법도 있어 함수를 이용해보았다.  

```js
function Parent(){

  const [typeCode,setTypeCode] = useState<number>(1)
  const getData = ( n:number ) => {
    setTypeCode( n )
  }

  return(
    <>
      <children getData={getData} typeCode={typeCode}/>
    </>
  )
}
```

부모 컴포넌트에서 변수와 함수를 선언해주고


```js
interface setDataType{
  typeCode : number,
  getData:(n:number)=>void
}

function Children(props:setDataType){
  return(
    <div>
      <button onClick={ ()=> props.getData(1) }>1</button>
      <button onClick={ ()=> props.getData(2) }>2</button>
      <p> { props.typeCode } </p>
    </div>
  )
}
```

부모로부터 받는 값을 인터페이스에 타입을 정의해주었다.  
부모 컴포넌트에다 인터페이스를 작성하고 export해서 불러오는 방법도 있다 😮  
getData 는 리턴값이 없으니 void 로 지정 ❗️  
return 값이 숫자다!!! 그러면 void 대신 number를 입력해주면 된다.  


그리고 부모에서 자식으로 배열을 넘겨주고 싶을 수도 있을 때  

```js
export interface listType{
  list: [{
     id: number;
     title: string;
     content: string;
  }]
}

function Parent(){

  const [list,setList] = useState<listType['list']>([{id:1, title:"타이틀이다.", content:"내용입니다."}])

  return(
    <>
      <children {...list}/>
    </>
  )
}
```

스프레드 연산자를 이용해서 전달해줄 수 있다  

​
```js
import { listType } from '../components/Parent'

function Children(list:listType['list']){
  return(
    <ul>
      { Object.values(list).map( n =>{
         return <li key={n.id}>제목: { n.title } , 내용: { n.content }</li>
      })}
    </ul>
  )
}
```

데이터 프로토타입이 object로 들어왔기 때문에 다시 배열 형태로 바꿔준 뒤 map 을 돌려주었다. 

