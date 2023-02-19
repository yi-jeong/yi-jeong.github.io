---
layout: post
title: bottom sheet
categories: [Study]
tags: [JS]
---


<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/bottomsheet.gif"></p>
<p style="color: #ccc; text-align: center">바텀시트 구현화면</p>  

**[바텀시트 구현페이지](https://yi-jeong.github.io/js/bottomsheet.html)**  
위 링크에서 확인 가능합니다 😀  

리액트 라이브러리에서 사용하던 바텀 시트를 
순수 자바스크립트로 구현해보았다.  

- - -

```css
<style>
    html,body,div,ul,li{ 
        margin: 0;
        padding: 0; 
    }
    ul, li{
        list-style: none;
    }
    a{
        text-decoration: none;
        color:#333;
    }
    .bottom-sheet-open {
        display: flex;    
        flex-flow: column wrap;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    .bottom-sheet-container{
        position:fixed;
        z-index: 999;
        top:110vh;
        left:50%;
        transform: translate(-50%,0);
        width:100%;
        min-height:80vh;
        max-height:80vh;
        padding-bottom:40px;
        background: #fff;
        box-shadow: 0px 0px 7px 0px rgb(0 0 0 / 10%);
        border-radius: 15px;
        transition:0.5s ease;
    }
    .active{
        top: 60vh;
        transition:1.5s ease;
    }
    .bottom-sheet-control{ overflow:hidden; }
    .bottom-sheet-icon{ 
        display:block;
        margin: 15px auto;
        width:10px;
        height:2px;
        border-top:1px solid #ddd;
        border-bottom:1px solid #ddd;
     }
    .bottom-sheet-container ul{
        padding:20px;
    }
    .bottom-sheet-container ul li{ padding:10px ; border-bottom:1px solid #eee; }
    
</style>
```

<br>

```html
    <div class="bottom-sheet">

        <div class="bottom-sheet-open">
            <button onclick="ContainerOpen();">OPEN</button>
        </div>
        
        <div class="bottom-sheet-container">
            <div class="bottom-sheet-control"><span class="bottom-sheet-icon"></span></div>
            <div style="width:100%; height:30vh; overflow: hidden; overflow-y: auto;">
                <ul>
                    <li><a href="#">Test01</a></li>
                    <li><a href="#">Test02</a></li>
                    <li><a href="#">Test01</a></li>
                    <li><a href="#">Test02</a></li>
                    <li><a href="#">Test01</a></li>
                    <li><a href="#">Test02</a></li>
                    <li><a href="#">Test01</a></li>
                    <li><a href="#">Test02</a></li>
                    <li><a href="#">Test01</a></li>
                    <li><a href="#">Test02</a></li>
                    <li><a href="#">Test01</a></li>
                    <li><a href="#">Test02</a></li>
                    <li><a href="#">Test01</a></li>
                </ul>
            </div>
        </div>

    </div>
```

<br>
  
```js
<script>
    const bottomSheet=document.querySelector('.bottom-sheet-container');
    const bottomSheetControl=document.querySelector('.bottom-sheet-control');
    let start_Y, move_Y, end_Y;
    

    bottomSheetControl.addEventListener('touchstart', touch_start, false);
    bottomSheetControl.addEventListener('touchmove', touch_move, false);
    bottomSheetControl.addEventListener('touchend', touch_end, false);

    function ContainerOpen(){
        const className = bottomSheet.classList.contains('active');
        if( className ){
            bottomSheet.classList.remove('active');
        }else{
            bottomSheet.className += ' active';
            console.log(bottomSheet.scrollHeight);
            console.log(bottomSheet.offsetHeight);
        }
    }

    function touch_move(e) {
        e.preventDefault();
        move_Y = e.changedTouches[0].pageY;
        if (move_Y>start_Y){
            console.log("모달내려감")
        }else{
            console.log("모달올라감")
        }
        
    }

    function touch_start(e){
        start_Y = e.changedTouches[0].pageY;
        console.log(start_Y);
    }
    
    function touch_end(e) {
        e.preventDefault();
        end_Y = e.changedTouches[0].pageY;
        console.log("결과: "+ ( start_Y - move_Y ) );
        if( ( start_Y - move_Y ) > -30 ){
            console.log("올림");
            
        }else{
            console.log("내림");
            bottomSheet.classList.remove('active');
        }
        
    }
    
</script>
```