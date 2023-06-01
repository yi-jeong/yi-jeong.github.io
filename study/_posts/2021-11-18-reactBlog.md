---
layout: post
title: React로 블로그 구현하기
categories: [Study]
tags: [React, JS]
---




```js
import React, { createContext, useRef, useState } from 'react';
import CreateList from './CreateList';
import ContentList from './ContentList';
import './reset.css';
import './App.css';


function App() {

  const [Inputs, setInputs] = useState({
    tit:'',
    txt:''
  });

  const { tit,txt } = Inputs;
  const onChange = e => {
    const { name, value } = e.target;
    setInputs({
      ...Inputs,
      [name]: value
    });
  };
  

  const [BlogContent, setBlogContent] = useState([
    {
      id:1,
      tit: '그게 되나 적당히 좋아하는 게',
      txt: '그게 됐다면 지금 제가 이러고 있게요',
      like:0,
      com:0,
      date:'2021-10-11',
      active: false
    },
    {
      id:2,
      tit: '눈을 맞춰 술잔을 채워',
      txt: '천장에 파도가 일렁거리잖니 생각해봐 복잡하게 말고 말이야 단순하게 얼마 안 가 우린 죽을 거야',
      like:0,
      com:0,
      date:'2021-10-13',
      active: false
    },
    {
      id:3,
      tit: '왠지 그럼 안될것 같아',
      txt: '처음에 보자마자 불안했어요 혹시나 사랑할까봐 왠지 난 그러면 안될것만 같은사람 왜 내게 다가와버렸나요',
      like:0,
      com:0,
      date:'2021-10-17',
      active: false
    },
    {
      id:4,
      tit: '잠이 오질 않네요',
      txt: '당신은 날 설레게 만들어 조용한 내 마음 자꾸만 춤추게 해 얼마나 얼마나 날 떨리게 하는지 당신이 이 밤을 항상 잠 못 들게 해',
      like:0,
      com:0,
      date:'2021-10-17',
      active: false
    },
    {
      id:5,
      tit: '아직 널 사랑하는 이유야',
      txt: '아직 널 사랑하는 이유야 원하건 원하지 않건 만나고 헤어져 더 아픈 사람들',
      like:0,
      com:0,
      date:'2021-10-17',
      active: false
    },
    {
      id:6,
      tit: '너의 낮과 밤',
      txt: '너의 낮엔 다정한 햇살이 너를 감싸주길 너의 밤엔 포근한 달님이 벗이 되어주길',
      like:0,
      com:0,
      date:'2021-10-17',
      active: false
    }
  ]);

  const nextid = useRef(7);
  const onCreate = () => {
    const CreateContent = {
      id: nextid.current,
      tit,
      txt,
      like:0,
      com:0,
      date:'2021-11-17'
    };
    setBlogContent(BlogContent.concat(CreateContent));

    setInputs({
      tit: '',
      txt: ''
    });
    nextid.current += 1;
  };

  const onRemove = id => {
    setBlogContent(BlogContent.filter( CreateContent => CreateContent.id !== id ));
  };

  const likeCount = id => {
    setBlogContent(
      BlogContent.map( CreateContent =>
        CreateContent.id === id ? { ...CreateContent, active: !CreateContent.active, like: !CreateContent.active ? CreateContent.like + 1 : CreateContent.like - 1 } : CreateContent
      )
    );
  }

    return (
  
    <>
    <header className="header">
      <div className="container">
        <h1><a>Blog</a></h1>
      </div>
    </header>
  
    <section className="List">
      <div className="container">

        <ContentList BlogContent={BlogContent} onRemove={onRemove} likeCount={likeCount} />
  
      </div>
    </section>

    <section className="ListInsert">
      <div className="container">

        <CreateList 
          tit={tit}
          txt={txt}   
          onChange={onChange}
          onCreate={onCreate}  
        />

      </div>
    </section>
    </>

  );
}

export default App;
```

App.js  


배열 추가, 삭제, 좋아요 버튼을 구현했다.  
화살표 함수와 useState,useRef 를 이용해 작업했다 😀  
스크립트다 보니 새로고침하면 추가했던 배열이 초기화 되는 현상이 . . . 🥲  
세션에 담아두는 것도 고려해봐야겠다 !  

```js
import React from 'react';

function Content({n,onRemove,likeCount}){

    return(
      <div className="ListBox">      
        <dl>
          <dt><b style={{ color: n.active ? '#999' : 'black' }}>{ n.tit }</b></dt>
          <dd>{ n.txt }</dd>
        </dl>
        <p><span className="date">{n.date}</span><span className="emojiWrap"><span className="emoji" onClick={ () => likeCount(n.id) }>💗</span>+{ n.like }<span className="emoji">✉</span>+{ n.com }</span></p>
        <span className="Remove-btn"><a onClick={ () => onRemove(n.id) }>삭제</a></span>
      </div>
    );
  
}

function ContentList({ BlogContent, onRemove, likeCount }){
    return(
        <>
        {BlogContent.map(n => (
            <Content n={n} key={n.id} onRemove={onRemove} likeCount={likeCount} />
        ))}
        </>
    );
}

export default ContentList;
```
ContentList.js


리스트를 불러오는 컴포넌트  
map을 이용해 리스트를 나열 시켰다  
좋아요는 아이디당 하나씩 올릴 수 있게 배열에 active를 추가해 true/false로 구분을 할 수 있게 해주었다 !  

```js
import React from 'react';

function CreateList({ tit, txt, onChange, onCreate }){
    return(
    <>
        <div className="ListCreateBox">
            <label >제목</label>
            <input name="tit" value={tit} onChange={onChange}/>
            <label>내용</label>
            <input name="txt" value={txt} onChange={onChange}/>
            <p className="submit"><button onClick={onCreate}>등록</button></p>
        </div>    
    </>    
    );
}

export default CreateList;
```

CreateList.js

배열을 추가시키는 영역  

- - -

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20211118-blog.png"></p>
<p style="color: #ccc; text-align: center">블로그 구현화면</p>  

**[블로그 구현화면 보기](https://yi-jeong.github.io/ReactBlog/)**  
결과물은 위 링크에서 확인하실 수 있어요 :3  