---
layout: post
title: CORS μ€λ₯π€’ ν΄κ²°νκΈ°π
categories: [Study]
tags: [React.js, JS]
---

λ€μ΄λ² apiλ₯Ό μ΄μ©ν΄ κ²μκΈ°λ₯μ κ΅¬ννλ μ€ μ€λ₯κ° λ°μνλ€.

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20220216-cors.png"></p>

<p style="padding: 1rem;text-align: center; background: #f5f5f5;">
Access to XMLHttpRequest at 'https://openapi.naver.com/v1/search/movie.json?query=starwars&display=10' from origin 'http://localhost:3000' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
</p>  

<p style="color: #ccc; text-align: center">λ­λΌλκ±°μΌ...? π₯</p>  

- - - 

β
μ²μλ³΄λ μ€λ₯μ λΉν©νμμΌλ μλ§μ κ΅¬κΈλ§ κ²°κ³Ό μ΄ μ€λ₯λ₯Ό ν΄κ²° ν  μ μμλ€.  
CORS(Cross-origin resource sharing)λ λΈλΌμ°μ μ κ΅¬ν μ€νμ ν¬ν¨λλ μ μ±μ΄λ©° λ³΄μμ κ°μ νΈμ€νΈμ μ λ³΄λ§ νμ©νλ€κ³  νλ€.  
λΈλΌμ°μ  μ μ±μ΄κΈ° λλ¬Έμ μλ²-μλ²κ° ν΅μ ν  λλ λ¬Έμ  λμ§ μλλ€.  
μ΄ μ€λ₯λ κ°λ°μλ₯Ό λΆνΈνκ² νλ μ¬μ€ κ³ λ§μ΄ λμμ΄λ€.  
λͺ¨λ  λ°μ΄ν°λ₯Ό νΈμΆν  μ μκ² λλ€λ©΄ λͺ»λ λ§μμ κ°μ§ μ¬μ©μκ° μ§ λμ μ€ν¬λ¦½νΈλ₯Ό μ¬μ μ μμΌλ κ·Έκ²μ κ±Έλ¬μ£Όλ μ­ν μ νλ€.  


- - -

<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/20220216-cors02.png"></p>
<p style="color: #ccc; text-align: center">ν΅μ κ³Όμ μ κ·Έλ¦ΌμΌλ‘ λνλ΄λ³΄μλ€.</p>  

μ΄ μ€λ₯λ₯Ό ν΄κ²°νκΈ° μν΄μ λͺκ°μ§ μμμ ν΄μ€μΌνλ€.  
λ·λ¨μμ λ°μ΄ν°λ₯Ό λ°μ μλ¨μΌλ‘ λ°μ΄ν°λ₯Ό λ³΄λ΄μ£Όλ κ² μ’λ€λλ°  
νμλ λ°±μλ²λ₯Ό μ¬μ©νμ§ μμ νλ‘ νΈμλ²μμ νλ‘μ μ€μ μ ν΄μ£Όμ΄μΌ νλ€ π’  

React μ λͺ¨λμ νλ μ€μΉν΄μ£Όμ  

```js
Npm I http-proxy-middleware --saveο»Ώ
```

```
src
β£ assets
β β£ css
β β£ bootstrap.css
β β£ reset.css
β β style.css
β£ component
β β£ MovieList.js
β β MovieSearch.js
β£ pages
β β Main.js
β£ App.css
β£ App.js
β£ index.css
β£ index.js
β£ reportWebVitals.js
β£ setupProxy.js
β setupTests.js
```

μμμ€μΈ λλ ν λ¦¬ κ΅¬μ‘°  
setupProxy.js νμΌμ src νλ¨μ μμ± ν μλ λ΄μ© κ·Έλλ‘ μλ ₯ν΄μ€λ€.  


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

Targetμ μ°νν  origin μ μμ±νλ€.  
Originμ URLμμ νλ‘ν μ½, λλ©μΈ, ν¬νΈ λ²νΈλ₯Ό ν©μΉ λΆλΆμ΄λ€.  
νμλ λ€μ΄λ²APIλ₯Ό μ΄μ©ν  κ²μ΄κΈ° λλ¬Έμ λ€μ΄λ²μμ λ°μ΄ν°λ₯Ό μ κ³΅ν΄μ£Όλ μ£Όμλ₯Ό μμ±ν΄μ£Όμλ€.  

- - -

### originμ κ΅¬μ‘°

<table>
    <thead>
        <tr>
            <td colspan="2" ailgn="center">https://yi-jeong.github.com:80/</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>νλ‘ν μ½</th>
            <td>https://</td>
        </tr>
        <tr>
            <th>λλ©μΈ</th>
            <td>yi-jeong.github.com</td>
        </tr>
        <tr>
            <th>ν¬νΈλ²νΈ</th>
            <td>:80</td>
        </tr>
    </tbody>
</table>
<br>
- - -


**[λ€μ΄λ² μν API μ½λ λ³΄κΈ° π](https://github.com/yi-jeong/movie-react)**  

β
μ νλ‘μ νΈμμ http-proxy-middleware μ μ¬μ©ν΄ cors λ¬Έμ λ₯Ό ν΄κ²°νλ€.  
νμ¬ μμμ€μΈ νλ‘μ νΈμ΄λ―λ‘ μμ±λ¨κ³λ μλμ§λ§ APIλ μ λλ‘ λΆλ €μμ§λ€!  

