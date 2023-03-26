---
layout: post
title: 자바스크립트의 실행순서를 공부해요 (+쓰레드)
categories: [Study]
tags: [JS]
---

스크립트를 작성하다보면 원하는 시점에 기능이 동작하지 않는 경우가 있습니다.  
작성한 코드와 실행 결과 사이에 미묘한 차이가 나는 건 왜일까요?  
오늘은 자바스크립트의 동작에 대해 알아봅시다 🙂  

JavaScript는 원래 웹 브라우저에서 실행되는 스크립트 언어로 개발되었기 때문에 단일 쓰레드 모델로 설계되었습니다. 웹 페이지에서 JavaScript 코드가 실행될 때 브라우저는 해당 페이지의 GUI 쓰레드에서 JavaScript 코드를 실행시킵니다.  

앗, 여기서 궁금점이 생겼습니다.   
**JavaScript는 싱글 스레드(single-thread)인데 어떻게 비동기 처리**를 하는걸까요?  

---

### 📝 JavaScript의 동작순서
✅ 먼저 파싱 과정을 거쳐서 문법적인 에러를 체크합니다.  
✅ 실코드 실행을 위해 함수, 전역 코드, Eval 코드 등 실행 컨텍스트(Execution Context)를 생성합니다.  
✅ 현재 실행 컨텍스트의 변수와 상위 스코프의 변수를 연결하여 스코프 체인(Scope Chain)을 생성합니다.  
✅ 변수와 함수의 선언문을 미리 처리합니다. 이때 선언문은 코드의 상단으로 이동하여 처리됩니다.  
✅ this는 함수 호출 패턴에 따라 바인딩됩니다.  
✅ 실행순서를 스택(Stack)에 저장하고 실행합니다. 비동기적인 코드는 이벤트 큐(Event Queue)에 등록되어 나중에 실행됩니다.  

답은 JavaScript의 동작 순서에 나와있었습니다.  
JavaScript는 싱글 스레드이기 때문에 하나의 스택(Stack)만을 가지고 있습니다.  
초반에 읽어들인 함수, 상수, 변수 등을 스택(Stack)에 정리해두고,   
addEventListener, setTimeout, promise, postMessage 같은 함수들은 이벤트 큐(Event Queue)에 등록되어 사용됩니다.  

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20230326-stackandqueue.png"></p>

스택(Stack)에 저장되어있는 모든 함수를 실행하면 이벤트 큐(Event Queue)를 실행하게 됩니다.  

JavaScript에서는 일반적으로 비동기적인 처리를 많이 사용하고, 이벤트 큐를 통해 처리되는 함수들은 우리가 직접 호출하지 않기에 함수의 동작 순서를 완벽하게 제어할 수는 없습니다. 함수의 실행 순서를 완벽하게 제어할 수는 없지만 적절한 방법으로 콜백 함수나 Promise, async/await 등을 사용하여 비동기적인 처리를 수행하고, 이벤트 큐(Event Queue)를 효율적으로 활용하여 원하는 동작을 구현할 수 있습니다. 🙂