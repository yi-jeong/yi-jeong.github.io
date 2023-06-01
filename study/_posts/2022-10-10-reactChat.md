---
layout: post
title: socket.io 로 채팅창📝 구현하기
categories: [Study]
tags: [React, JS]
---


<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/chat.gif"></p>
<p style="color: #ccc; text-align: center">외로운 1인 채팅 . . . 🥲</p>  

**[깃허브 코드보기](https://github.com/yi-jeong/chatApp)**  
위 링크에서 코드 확인 가능합니다 😀  

- - - 

### 작업계기

> noSQL 을 접해볼 기회가 없어 어떤 프로젝트를 진행하면 써볼 수 있을까... 고민하던 끝에 채팅이 떠올랐다.
최근에 자바로 사이트 하나 구현했으니 이번 프로젝트는 리액트로 작업 하였다.
리액트의 장점 중 하나인 글로벌 상태관리에도 신경을 쓰며 작업했다 ‼️
서버는 node.js 기반 express 를 사용했는데 SQL 작업하면서 스프링 부트로 바뀔 수도 있다. 😮  

- - -

### 환경 구성

* Front:
    1. React.js
* Server:
    1. Node.js
    2. express
    3. socket.io

- - -

### ⭐️ 체크 포인트 ⭐️  

```js
const socket = io.connect("http://localhost:4000");
```

/src/ChatContext.js  
커넥션 상수는 글로벌 상태관리 코드 쪽에 작성해주었다.  

```js
useEffect(() => {
        socket.on("receive chat", (message) => {
            setChatList((chatList) => chatList.concat(message));
            dispatch({
                type: "CHANGE_NAME",
                name: message.name
            });
        });
    }, []);
```

ChatContext.js  
채팅 입력 후 초기화 되는 name 값을 콜백에 들어있는 데이터로 다시 채워주었다.  

```js
function ChatContentBox(){

    const messageBoxRef = useRef();
    const chatList = useChatList();
    
    const scrollToBottom = () => {
        if (messageBoxRef.current) {
          messageBoxRef.current.scrollTop = messageBoxRef.current.scrollHeight;
        }
      };

      useEffect(() => {
        scrollToBottom();
      }, [chatList]);

    return(
        <div className="chat-wrap">
            <div className="container">
                <div className="chat-content-box" ref={messageBoxRef}>
                    { chatList.map((el, index) => (
                        <div className="chat" key={index}>
                            <div className="chat-name">{el.name}</div>
                            <div className="chat-message">{el.message}</div>
                        </div>
                    ))
                    }
                </div>
            </div>
        </div>
    )
}
```

/src/Components/ChatContentBox.js  
새 메시지를 받을 때 마다 스크롤이 맨 아래에 위치하기 위해 useRef 로 DOM을 컨트롤 하였다.  