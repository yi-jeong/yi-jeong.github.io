---
layout: post
title: CORS 오류🤢 해결하기😖
categories: [Study]
tags: [React, JS]
---

네이버 api를 이용해 검색기능을 구현하던 중 오류가 발생했다.

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20220216-cors.png"></p>

<p style="padding: 1rem;text-align: center; background: #f5f5f5;">
Access to XMLHttpRequest at 'https://openapi.naver.com/v1/search/movie.json?query=starwars&display=10' from origin 'http://localhost:3000' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
</p>  

<p style="color: #ccc; text-align: center">뭐라는거야...? 😥</p>  

- - - 

​
처음보는 오류에 당황하였으나 수많은 구글링 결과 이 오류를 해결 할 수 있었다.  
CORS(Cross-origin resource sharing)는 브라우저의 구현 스펙에 포함되는 정책이며 보안상 같은 호스트의 정보만 허용한다고 한다.  
브라우저 정책이기 때문에 서버-서버간 통신할 때는 문제 되지 않는다.  
이 오류는 개발자를 불편하게 하나 사실 고마운 녀석이다.  
모든 데이터를 호출할 수 있게 된다면 못된 마음을 가진 사용자가 질 나쁜 스크립트를 심을 수 있으니 그것을 걸러주는 역할을 한다.  


- - -

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20220216-cors02.png"></p>
<p style="color: #ccc; text-align: center">통신과정을 그림으로 나타내보았다.</p>  

이 오류를 해결하기 위해서 몇가지 작업을 해줘야한다.  
뒷단에서 데이터를 받아 앞단으로 데이터를 보내주는 게 좋다는데  
필자는 백서버를 사용하지 않아 프론트서버에서 프록시 설정을 해주어야 했다 😢  

React 에 모듈을 하나 설치해주자  

```js
Npm I http-proxy-middleware --save﻿
```

```
src
┣ assets
┃ ┣ css
┃ ┣ bootstrap.css
┃ ┣ reset.css
┃ ┗ style.css
┣ component
┃ ┣ MovieList.js
┃ ┗ MovieSearch.js
┣ pages
┃ ┗ Main.js
┣ App.css
┣ App.js
┣ index.css
┣ index.js
┣ reportWebVitals.js
┣ setupProxy.js
┗ setupTests.js
```

작업중인 디렉토리 구조  
setupProxy.js 파일을 src 하단에 생성 후 아래 내용 그대로 입력해준다.  


```js
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app){
    app.use(
        createProxyMiddleware( '/naver', { target: 'https://openapi.naver.com',
        changeOrigin: true,
        pathRewrite:{ '^/naver/':'/' }
    }) )
};
```

Target에 우회할 origin 을 작성한다.  
Origin은 URL에서 프로토콜, 도메인, 포트 번호를 합친 부분이다.  
필자는 네이버API를 이용할 것이기 때문에 네이버에서 데이터를 제공해주는 주소를 작성해주었다.  

- - -

### origin의 구조

<table>
    <thead>
        <tr>
            <td colspan="2" ailgn="center">https://yi-jeong.github.com:80/</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>프로토콜</th>
            <td>https://</td>
        </tr>
        <tr>
            <th>도메인</th>
            <td>yi-jeong.github.com</td>
        </tr>
        <tr>
            <th>포트번호</th>
            <td>:80</td>
        </tr>
    </tbody>
</table>
<br>
- - -


**[네이버 영화 API 코드 보기 📝](https://github.com/yi-jeong/movie-react)**  

​
위 프로젝트에서 http-proxy-middleware 을 사용해 cors 문제를 해결했다.  
현재 작업중인 프로젝트이므로 완성단계는 아니지만 API는 제대로 불려와진다!  

