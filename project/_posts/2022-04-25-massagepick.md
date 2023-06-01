---
layout: post
title: 토이프로젝트 모바일앱
categories: [Project]
tags: [React, TypeScript, JS, Design]
feature-img: "assets/img/project/thum/mag-thum.png"
hidden: true
---

<p class="box-title"># 기여도</p>

🎨 **디자인** 100%  
📝 **퍼블리싱** 100%  
🛠 **스크립트구현** 100%  

----
<p class="box-title"># 작업 환경 및 기술스택</p>

**DESIGN**  
   ⭐️ XD  
   ⭐️ Photoshop  
   ⭐️ Illustrator

**VCS**  
   ⭐️ Git

**IDE**   
   ⭐️ VSCode  

**FRONT**  
   ⭐️ React.js 17.0.44  
   ⭐️ react-router-dom 5.3.3  
   ⭐️ Styled-components 5.1.24  
   ⭐️ TypeScript 4.6.2

----

<p class="box-title"># 작업 내용</p>

🗓 **작업기간**  2022. 03. 11 ~ 2022. 04. 25

--- 


📌 **디자인 시안 작업**   

📌 **재사용이 가능한 컴포넌트 작업**  


📌 **모바일 앱 전용 퍼블리싱 작업**  

<p style="text-align: center;">
<img class="ylong-img" src="{{ site.baseurl }}/assets/img/project/massagePick.png">
</p>


📌 **마사지샵 리스트 api 데이터 랜더링**

<p style="text-align: left;">
<img class="ylong-img" src="{{ site.baseurl }}/assets/img/project/massage-list.gif">
</p>

```js
    useEffect(()=>{
        fetch('/massage-pick/data/listbox.json',{
            method: 'GET'
        })
        .then( res => res.json() )
        .then( data => {
            setShopList(data);
        });
    },[]);
```  

json 파일을 만들어 리스트 화면 랜더링을 해주었다 🙂  
다음에는 목업 라이브러리를 이용해보도록 하자!  


📌 **퓨터 영역 사업자정보 더보기 기능 구현**

<p style="text-align: left;">
<img class="ylong-img" src="{{ site.baseurl }}/assets/img/project/massage-footer.gif">
</p>

📌 **router-dom을 이용해 디테일 페이지 제작**

<p style="text-align: left;">
<img class="ylong-img" src="{{ site.baseurl }}/assets/img/project/massage-detail.gif">
</p>




