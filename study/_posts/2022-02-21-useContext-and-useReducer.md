---
layout: post
title: useContext 와 useReducer ‼️ 로 글로벌 상태 관리하기 ✌️
categories: [Study]
tags: [React, JS]
---


<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20220221-chart.png"></p>
<p style="color: #ccc; text-align: center">child4에게 props를 전달해주기 위해선 props를 사용하지 않는 child2에도 props를 전달해줘야한다. </p>  

- - - 

> 부모 컴포넌트에서 하위 자식 컴포넌트로 props를 전달하는 건 어렵지 않다.
하지만 부모에서 자식으로 전달한 뒤 자식의 자식에게 계속 props를 전달하기엔 사용하지 않는 컴포넌트에도 데이터를 전달해줘야하니 참 비효율적인 작업이다. 😞
그렇기 때문에 보통은 글로벌 상태관리 라이브러리를 사용하는데 (Ex. 리덕스) 
라이브러리를 사용하지 않고도 글로벌 상태관리가 가능하다.
React Hook에서도 이러한 기능을 제공해준다.
useReducer 와 useContext만으로도 props를 글로벌하게 사용할 수 있다. ✌️

- - -

### useReducer 함수

```js
const [state,dispatch] = useReducer(reducer, initialState);
```

* State: 현재상태
* Dispatch: action 을 발생시키는 함수 타입을 넘겨줘야함
* Reducer: 상태 업데이트 함수
* initialState: 초기상태

> **잠깐 ‼️ 🖐**  
useState 와 useReducer 중 무얼 쓰는 게 좋은가요❓  


컴포넌트에서 관리하는 상태값이 적거나 단순한 숫자나 문자열 같은 경우엔 useState를 사용하고,  
관리하는 값이 많고 어떠한 값을 받아 새로운 상태로 갱신 해야한다면 useReducer를 사용하는 것이 좋다.  

댓글 수 카운트라던가 글 수정, 삭제 기능을 구현할 때 쓰면 좋을 거 같다 !  

- - -

### useContext 함수

Context API : 프로젝트 안에서 전역적으로 사용 할 수 있는 값을 관리  
컴포넌트에게 props를 전달해줘야 하는 상황에서 코드의 구조가 훨씬 깔끔해짐  

```js
createContext(initialState)
```

Context 객체생성  
CreateContext의 파라미터에 context의 기본값을 설정할 수 있다.  


```js
context.provider value={꺼내서 쓸 객체}
```

Context.provider 생성한 context를 하위 컴포넌트에 전달하는 역할

```js
const test= useContext(createcontext로 생성한 객체)
```

사용할 페이지에 작성후 createcontext로 생성한 객체를 불러온다.

- - -

```
src
┣ Component
┃ ┗ Component.js
┣ Contexts
┃ ┗ ContextProvider.js
┗ app.js
```

디렉토리 구조  

```js
import ContextProvider from './Contexts/ContextProvider';
import Component from './Component/Component';

function App() {
  return (
    <>
      <ContextProvider>
        <Component />
      </ContextProvider>
    </>
  );
}

export default App;
```

App.js  
Props를 사용할 컴포넌트를 contenxtProvider로 감싸준다  


```js
import React, { useReducer } from "react";

export const Context = React.createContext();

const initialState = {
    count: 0
};

const reducer = (state,action) => {
    switch(action.type){
        case "PLUS":
            return{
                ...state,
                count: action.value,
            }
        case "MINUS":
            return{
                ...state,
                count: action.value,
            }
        default:
            throw new Error();
    }
};

const ContextProvider = ({children}) => {
    const [state, contextDispatch] = useReducer(reducer,initialState)
    return(
        <Context.Provider value={{ count: state.count, contextDispatch }}>
            {children}
        </Context.Provider>
    );
};

export default ContextProvider;
```

ContextProvider.js

```js
import React, { useContext } from "react";
import { Context } from "../Contexts/ContextProvider";

const Component = () => {
    const { count, contextDispatch} = useContext(Context)

    return(
        <div>
            <p> count: {count}</p>
            <button onClick={()=> contextDispatch({type:"PLUS", value: count+1})}>+1</button>
            <button onClick={()=> contextDispatch({type:"MINUS", value: count-1})}>-1</button>
        </div>
    );
}

export default Component;
```

Component.js  

사용할 상태를 useContext 로 불러온다.  
버튼을 클릭했을 때 변경 값을 dispatch로 전달하면 ContextProvider.js 에 있는 state 자리에 들어가 reducer 객체로 상태를 변경한다.  

그러면   
+1 버튼을 눌렀을 때 1씩 오르고  
-1 버튼을 눌렀을 때 1씩 빠지는  
카운터가 완성된다‼️  

- - -

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20220221-result.png"></p>

**[투두리스트 구현 페이지](https://github.com/yi-jeong/TodoList)**  
위 링크에서 코드 확인 가능합니다 😀  

위 내용을 토대로  
useContext와 useReducer를 이용해 작업한 투두리스트✌️  
자식들에게 하나하나 props를 넘겨주지 않아도 useContext를 사용하면 전역 공유가 가능해진다 😁  

