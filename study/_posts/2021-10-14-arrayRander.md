---
layout: post
title: 배열 랜더링하기
categories: [Study]
tags: [React.js, JS]
---


```js
import React from 'react';

const Allarray=[
  {
    id:1,
    name: '도경수',
    age: '29'
  },
  {
    id:2,
    name: '변백현',
    age:'30'
  },
  {
    id:3,
    name: '김준면',
    age: '31'
  }
]

function App() {
  return (
    <>
    <ul>
      <li>이름: {Allarray[0].name}</li>
      <li>나이: {Allarray[0].age}</li>
    </ul>
    <ul>
      <li>이름: {Allarray[1].name}</li>
      <li>나이: {Allarray[1].age}</li>
    </ul>
    <ul>
      <li>이름: {Allarray[2].name}</li>
      <li>나이: {Allarray[2].age}</li>
    </ul>
    </>
  );
}

export default App;
```

하나하나 그대로 불러오는 방법과


```js
import React from 'react';

function User({ n }){
  return (
    <ul>
      <li>이름: { n.name }</li>
      <li>나이: { n.age }</li>
    </ul>
  );
}

function App() {

  const Allarray=[
    {
      id:1,
      name: '도경수',
      age: '29'
    },
    {
      id:2,
      name: '변백현',
      age:'30'
    },
    {
      id:3,
      name: '김준면',
      age: '31'
    }
  ];

  return (
    <>
    <User n={Allarray[0]} />
    <User n={Allarray[1]} />
    <User n={Allarray[2]} />
    </>
  );
}

export default App;
```

컴포넌트를 재사용 할 수 있게 작성하는 방법


```js
import React from 'react';

function User({ n }){
  return (
    <ul>
      <li>이름: { n.name }</li>
      <li>나이: { n.age }</li>
    </ul>
  );
}

function App() {

  const Allarray=[
    {
      id:1,
      name: '도경수',
      age: '29'
    },
    {
      id:2,
      name: '변백현',
      age:'30'
    },
    {
      id:3,
      name: '김준면',
      age: '31'
    }
  ];

  return (
    <>
    {Allarray.map(n =>(
      <User n={n} key={n.id}/>
    ))}
    </>
  );
}

export default App;
```

자바스크립트 내장함수 map을 이용하여 컴포넌트를 불러오는 방법  


map을 이용할 땐 해당 배열 고유의 키값이 필요하다!  
그래야 컴퓨터가 데이터를 정확하게 구분할 수 있다.  
DB 테이블 짤 때 고유 키 값 넣는 거랑 똑같이 생각하면 될 거 같기도 하다 ! ! ! 😀    



​