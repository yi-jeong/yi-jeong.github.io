---
layout: post
title: 싱크(Sync)와 어싱크(Async), 블록(Block)와 논블록(Non-block)을 공부해요
categories: [Study]
tags: [etc]
---


프로그래밍에서 주로 사용되는 용어인  
싱크(Sync)와 어싱크(Async), 블록(Block)와 논블록(Non-block)  
이들이 무엇을 의미하는 지 알아봅시다 🙂


### 📚 싱크(Sync)와 어싱크(Async)  
---
싱크(sync)는 동기(synchronous), 어싱크(async)는 비동기(asynchronous)의 뜻을 가지고 있습니다.   

**✅ 동기(Synchronous)**    
동기는 작업이 순차적으로 실행되는 것을 의미해요.  
한 작업이 완료될 때까지 다음 작업은 대기하게 됩니다.   
이 방식은 프로그래밍이 쉽고 직관적이지만, 작업이 서로 차단(block)될 수 있으므로 효율성이 떨어질 수 있습니다.  

이해하기 쉽게 실생활로 예시를 들자면,  
> 나: 커피를 주문  
바리스타: 커피를 만듦   
나: 기다림  
바리스타: 완성된 커피 전달  
나: 다른 음료를 주문하거나 테이블에 앉음   

코드로도 확인해봅시다. 🙂  
예시 코드로 멀티쓰레드인 Java와 싱글쓰레드인 JavaScript를 사용하였습니다 ✌️ 

**Java** 
```java   
public class SyncExample {
    public static void main(String[] args) {
        System.out.println("Start step 1");
        performTask();
        System.out.println("End step 1");

        System.out.println("Start step 2");
        performTask();
        System.out.println("End step 2");
    }

    private static void performTask() {
        // 2초뒤 작업 종료
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

**JavaScript**
```js
console.log('Start step 1');
performTask();
console.log('End step 1');

console.log('Start step 2');
performTask();
console.log('End step 2');

function performTask() {
    let start = new Date().getTime();
    while (new Date().getTime() < start + 2000); // 2초뒤 작업 종료
}
```

<br/>

**✅ 비동기(Asynchronous)**   
비동기는 여러 작업이 동시에 실행될 수 있는 것을 의미해요.  
한 작업이 완료되기를 기다리지 않고 다음 작업을 진행합니다.  
이 방식은 복잡성이 늘어나지만, 효율성이 높아질 수 있습니다.  

> 나: 커피를 주문  
바리스타: 커피를 만듦   
나: 기다리는동안 책을 읽음  
바리스타: 완성된 커피 전달  
나: 다른 음료를 주문하거나 테이블에 앉음  

코드로도 확인해봅시다. 🙂 

**Java** 
```java
import java.util.concurrent.*;

public class AsyncExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(2);

        System.out.println("Start step 1");
        executorService.submit(() -> performTask());
        System.out.println("End step 1");

        System.out.println("Start step 2");
        executorService.submit(() -> performTask());
        System.out.println("End step 2");

        executorService.shutdown();
    }

    private static void performTask() {
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

멀티 쓰레드는 하나의 쓰레드만을 가지고 있지 않으므로  
쓰레드를 추가해 병렬로 처리해주는 것이 가능합니다.  


**JavaScript**  
```js
console.log('Start step 1');
performTask(() => console.log('End step 1'));

console.log('Start step 2');
performTask(() => console.log('End step 2'));

function performTask(callback) {
    setTimeout(callback, 2000);
}
```

자바스크립트는 싱글쓰레드지만 이벤트 큐가 따로 존재해 비동기처리가 가능합니다.  

<br/>

### 📚 블록(Block)와 논블록(Non-block)   
---
블록(block)과 논블록(non-block)은 프로그램이 데이터를 요청하거나 작업을 수행할 때의 동작 방식을 나타냅니다.

**✅ 블로킹(Block)**  
어떤 작업의 완료를 기다리는 동안 실행 흐름이 멈추는 것을 말합니다.  
예를 들어, 데이터를 요청하고 그 응답이 도착할 때까지 기다리는 동안 프로그램은 그 외의 다른 일을 처리하지 않습니다.  

이해하기 쉽게 실생활로 예시를 들자면,  
> 1. 사과와 우유를 사야해  
2. 우유도 사야하지만 사과를 장바구니에 담을 때 까지 우유를 찾지 않음  
3. 사과를 찾아 장바구니에 담음   
4. 우유를 찾음  

Java에서는 기본적으로 I/O 작업이 `블로킹(blocking)`으로 동작합니다.  
DB Connection 으로 예를 들어볼까요? 

**Java** 
```java
import java.sql.*;

public class DBConnection {
    public static void main(String[] args) {
        String url = "jdbc:xx.xxx.xxx.xxx";
        String user = "username";
        String password = "password";

        try {
            Connection conn = DriverManager.getConnection(url, user, password);
            Statement stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM test");

            while (rs.next()) {
                System.out.println(rs.getString("your_column"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

커넥션을 시도하고, 해당 쿼리 결과값을 받아올 때 까지 while문은 동작하지 않고 대기 하게 됩니다.  
블로킹은 처리를 끝낼 때 까지 다음 단계를 진행하지 않아요 ! ! !   

JavaScript 로도 블로킹 동작을 구현할 수 있어요.  
`async/await`를 사용하여 블로킹 동작처럼 구현해봅니다.  

```js
const mysql = require('mysql2/promise');

async function main() {
    const connection = await mysql.createConnection({
        host: 'localhost',
        user: 'user',
        password: 'password',
        database: 'database'
    });

    const [rows, fields] = await connection.execute('SELECT * FROM test');
    
    console.log(rows);
    
    await connection.end();
}

main();
```

`await` 키워드는 mysql.createConnection과 connection.execute가 완료될 때까지 코드의 실행을 일시 중지시킵니다.

<br/>

**✅ 논블로킹(Non-block)**  
논블로킹은 특정 작업의 완료를 기다리는 동안에도 다른 작업을 계속할 수 있는 방식입니다.  
즉, 하나의 작업이 완료될 때까지 대기하지 않고, 다른 작업을 실행해요.  
이를 통해 시스템의 효율성을 높일 수 있지만, 블로킹 방식에 비해 로직이 복잡해질 수 있답니다.  
 
> 1. 사과와 우유를 사야해  
2. 사과를 찾는 도중 우유를 발견해 우유를 장바구니에 담음  
3. 사과를 찾아 장바구니에 담음  

자바에서 논블로킹 방식으로 구현하려면 라이브러리를 써야합니다.  
그러니 자바 예시코드는 작성하지 않겠습니다.  

```js
const mysql = require('mysql');

const connection = mysql.createConnection({
    host: 'localhost',
    user: 'user',
    password: 'password',
    database: 'database'
});

connection.query('SELECT * FROM test', function (error, results, fields) {
    if (error) throw error;
    console.log(results);
});

connection.end();
```

콜백 함수를 이용하여 논블로킹 연산을 수행합니다.  
쿼리가 실행되는 동안 프로그램은 다른 작업을 계속 수행할 수 있습니다.  


### 🧐 두 의미가 너무 비슷해서 차이를 찾기가 힘들어요.  
---

동기와 블록은 특정 작업이 완료될 때까지 기다리는 방식에서 공통점을 가지고 있지만 살짝 다른 개념을 가지고 있어요.   
동기와 블로킹의 차이점은 "무엇을" 기다리느냐에 있지요.   
동기적인 연산에서는 "결과"를 기다리고, 블로킹 연산에서는 "작업의 완료"를 기다립니다.   
그러니 동기 작업이 항상 블로킹일 필요는 없으며, 블로킹 작업이 항상 동기적일 필요도 없는 것이죠. 

싱크와 어싱크, 블록과 논블록을 이해하고 잘 활용해 효율적인 프로그램을 짜는 그날까지 화이탱 😁  
