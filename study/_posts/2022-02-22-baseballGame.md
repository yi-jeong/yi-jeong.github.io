---
layout: post
title: 숫자야구 ⚾️ 만들기 ✌️
categories: [Study]
tags: [JS]
feature-img: "assets/img/thum/js.png"
excerpt_separator: <!--more-->
---


<p style="text-align: center;"><img src="{{ site.baseurl }}/assets/img/baseball.png"></p>
<p style="color: #ccc; text-align: center">숫자야구 구현화면</p>  

**[숫자야구 플레이하기](https://yi-jeong.github.io/js/baseball.html)**  
위 링크에서 플레이 가능합니다 😀  

> 스크립트로 무슨 게임을 만들 수 있을까, 고민을 하던 차에 학창시절 정말 열심히 하던 야구게임이 생각났다.  
스크립트로 충분히 구현할 수 있을 거 같아 작업하게된 숫자야구 😎 

- - -


#### 구현 기능  
1. input 에 데이터를 입력하면 다음칸으로 포커스 이동  
2. input 에 들어간 데이터 일괄 삭제 처리  
3. 사용자가 입력한 값과 정답이 일치하는 지 체크  

  
```js
<script>

        const answerList=[1,2,3,4,5,6,7,8,9,0];
        const answer=[];
        const input=document.querySelectorAll(".baseball");

        //랜덤으로 정답 추출
        function answerRandon(){
            for( let i=0; i<3; i++){
                const Random=Math.floor(Math.random()*answerList.length);
                answer[i] = answerList.splice(Random, 1)[0];
            }
        }

        answerRandon();

        console.log(answer)

        //input에 숫자를 입력하면 자동으로 다음칸 이동하는 함수
        function next(num){
            switch(num){
                case 0:
                    const checkInput1 = input[0].value.length;
                    if (checkInput1 == 1){
                        input[1].focus();
                    }
                case 1:
                    const checkInput2 = input[1].value.length;
                    if (checkInput2 == 1){
                        input[2].focus();
                    }
            }
        }

        //input에 들어간 숫자를 지우는 함수
        function reset(){
            input[0].value="";
            input[1].value="";
            input[2].value="";
        }

        //사용자가 입력한 값과 정답이 일치하는 지 체크
        function check(){ 

            if( input[0].value == input[1].value || input[0].value == input[2].value || input[1].value == input[2].value){
                if( input[0].value == input[1].value || input[0].value == input[2].value || input[1].value == input[2].value){
                alert("같은 숫자를 입력하셨습니다.")
                return input[0].focus();
            }
            }else{
                let userAnswer = input[0].value + input[1].value + input[2].value;
                let strike = 0
                let ball = 0
                for(let i=0; i<3; i++){
                    if( input[i].value == ""){
                        alert( i+1 + "번째 칸이 비어있습니다.");
                        return input[i].focus();
                    }else if(input[i].value == answer[i]){
                        strike++;
                    }else if(answer.includes(Number(input[i].value))){
                        ball++;
                    }
                }

                const listCon=document.querySelectorAll(".list-con");
                const listConli = document.createElement('li'); 
                const text = document.createTextNode(userAnswer + " strike: " + strike + " ball: " + ball);
                listConli.appendChild(text);
                listCon[0].appendChild(listConli);

                if( strike==3 ){
                    for( let i=0; i<3; i++){
                        input[i].style.background="#eee"
                    }
                    alert("축하합니다! 정답을 맞추셨습니다.")
                }
            }
        }

    </script>
```