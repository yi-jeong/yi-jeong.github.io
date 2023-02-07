---
layout: post
title: BOF 와 EOF 에 대하여
categories: [Study]
tags: [ASP Classic, MSSQL]
---

ASP를 사용할 때 Recordset을 이용해 DB 정보를 불러옵니다.  
그런데 말이죠......... BOF와 EOF는 대체 뭘까요? 


### **⭐️ 이녀석이 무엇을 뜻하길래 사용하나 싶어서 작성해보는 포스팅 ⭐️**  

- - -

ASP는 if문이 참 신기합니다.

```sql
if ~ then
  참
else
  거짓
end if
```
​
로 약간 생소하죠. 일단 괄호를 안 쓴다는 것......,  
if 문은 이렇게 쓴다는 걸 기억해주시고  


일단은요.  
DB를 커넥트 해줍니다.  

```sql
Set DB=Server.CreateObject("ADODB.Connection")
strConnect = "Provider=SQLOLEDB;Data Source=접속할서버;Initial Catalog=접속할DB명;User ID=DB아이디;Password=DB비밀번호;"
DB.Open strConnect
```

이렇게 레코드 셋을 생성해주었습니다.  
DB를 연결하고, 어떠한 테이블을 불러옵시다!  

```sql
SQL = "Select * from H"
```

H라는 테이블을 읽으라고 명령을 내렸습니다.  
그리고 화면에 뿌려줍니다.  
그 전에 저는 이 테이블에 데이터가 있는 지 없는 지 체크를 해주고 싶어요.  

이럴 때 BOF와 EOF를 사용합니다!  

```sql
Set Rec= DB.Execute(SQL)

if Rec.BOF or Rec.EOF then
  ...
else
  ...
end if
```

이 녀석이 뜻하는 의미는 무엇일까요?  

- - -
### BOF와 EOF
* BOF = Begin of File
* EOF = End of File

BOF와 EOF는 레코드셋의 시작과 끝을 의미합니다 !  
내 명령어로 불러온 데이터가 존재하냐 안하냐를 BOF와 EOF로 알아낼 수 있어요.  



- - -

<p><img src="{{ site.baseurl }}/assets/img/20190117-bof.jpeg"></p>

데이터가 있을 경우 Pointer 는 BOF와 EOF 사이에 있는 데이터들의 맨 첫번째에 위치합니다.  
포인터는 BOF와 EOF에 위치해있지 않다는 것이죠!  
그래서 if Rec.BOF or Rec.EOF then 의 결과에 false 값을 뱉어냅니다.  

- - -

<p><img src="{{ site.baseurl }}/assets/img/20190117-bof2.jpeg"></p>

명령어로 불러온 데이터가 하나도 존재 하지 않을 때엔 Pointer의 위치는 BOF=EOF에 놓여집니다.  
BOF와 EOF 사이에 데이터가 아무것도 없기 때문에 BOF와 EOF의 위치는 동일하게 되는데요.  
이렇게 되면 pointer 의 위치가 BOF=EOF에 놓여지게 되니 if Rec.BOF or Rec.EOF then 의 결과물은 true가 되겠죠?  


if Rec.BOF or Rec.EOF then 은 사용자에게 데이터를 페이지 안에 보여줘야 할 때 많이 사용해요.  
무언가 웹상에 데이터를 나타내야한다싶으면 거의 사용했던 거 같고요 ! !  
게시물 같은 경우 데이터가 아무것도 없을 때  


***게시물이 존재하지 않습니다.***  
​
로 나타내거든요.

- - -


```asp
if Rec.BOF or Rec.EOF then
  등록된 게시글이 없습니다.
else
  A= Rec("test_A")
end if
```


이런 식으로 사용했어요  
( 예시엔 쓰지 않았지만 게시판은 보통 루프ㅡ돌려서 데이터를 순서대로 나열 시킵니다. )  

​

ASP는 어느정도만 알면 다루기 편한 스크립트 언어 같습니다.  
저는 아직 한참 모자라지만요....... 😅 

