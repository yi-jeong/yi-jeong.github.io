---
layout: post
title: Recordset 정리
categories: [Study]
tags: [ASP Classic, MSSQL]
---

Recordset 오픈 시 5개의 인자를 갖는다.

- - -

**1. 첫번째 인자 : 오픈할 테이블이나 쿼리**  


**2. 두번째 인자 : DBConnection**  
   ex. Set db=Server.CreateObject("ADODB.Connection") 일 경우 db가 들어간다.  


**3. 세번째 인자 : 커서타입**  
   - adOpenForwardonly (0) 레코드를 앞으로 이동시키면서 순차적으로 한번만 읽음
   - adOpenKeyset (1) 레코드를 자유로이이동-레코드갱신-다른사용자가추가한내용열람불가
   - adOpenDynamic (2) 레코드를 자유로이이동-레코드갱신-다른사용자가 작업한 모든내용 열람가능
   - adOpenStatic (3) 레코드를 자유로이이동-레코드갱신불가-단지 데이타를 가져오는데 유용
   기본값은 0.   


**4. 네번째 인자 : 잠금타입**  
   - adLockReadOnly (1) 레코드 읽기전용 ( 레코드 업데이트 X )
   - adLockPessimistic (2) 레코드 편집을 하는 쿼리가 전달되는 순간 잠금. update메소드 호출 시 잠금이 풀림 ( 업데이트 전 실행단계에서 잠금 )
   - adLockOptimistic (3) 레코드 단위로 록킹. 편집이 종료되고 upadate메소드를 실행하는 경우 잠금 ( 업데이트 되어야 잠금 )
   - adLockBatchOptimistic (4) 갱신모드로 들어가면 작업가능  ( 여러 레코드를 업데이트 할 경우 잠금 )


**5. 다섯번째 인자 : 옵션**  
   - adCmdText (1) 첫번째가 SQL문을 실행함을 지정
   - adCmdStoredProc (4) 첫번째가 저장프로시저를 사용함을 지정
   - adCmdTable (2) 첫번째가 테이블을 가져오는것임을 지정

- - -

* **SQL서버 - DSN 없이 ODBC 연결방법**   
   Driver={SQL Server}; Server= 디비서버이름;Database=디비이름; UID=아이디; PWD=비밀번호  
   ex) Driver={SQL Server}; Server= taeyo;Database=taeyoDB; UID=taeyo; PWD=11 

* **SQL서버 - OLE DB 직접 접근 방법**  
   Provider=SQLOLEDB; Data Source=디비서버이름;Initial Catalog= ;디비이름; User id= 아이디; password=비밀번호   
   ex) Provider=SQLOLEDB;Data Source= taeyo;Initial Catalog= taeyo;User id=taeyo; password=11



