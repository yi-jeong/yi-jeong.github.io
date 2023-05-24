---
layout: post
title: 자바스크립트의 prototype 이해하기
categories: [Study]
tags: [JS]
---

자바스크립트의 객체를 보고 의문이 들었습니다.  
대체 prototype이 뭐지? class랑 뭐가 다른거야?  
그리고 왜 this의 값은 자바와 다르게 내가 원하는 대로 바인딩 되지 않는거지? 😡   

자바스크립트를 사용하여 개발하는 개발자로서 이대로 넘어갈 수 없었습니다 😞  
JavaScript의 prototype 을 이해해봅시다.  

### 🧐 프로토타입(prototype)이 뭔데?
---

자바스크립트의 모든 객체는 다른 객체를 참조하는 특별한 내부 속성인 `prototype`을 가지고 있습니다. 이 `prototype` 속성은 또 다른 객체를 참조하거나, 끝에 도달할 때까지 다른 객체들의 체인을 이루며 이를 프로토타입 체인이라고 합니다.

객체의 속성을 참조하려고 할 때, 해당 객체에 그 속성이 없다면, 자바스크립트는 객체의 `prototype`을 따라가면서 해당 속성을 찾습니다. 이는 프로토타입 체인이 끝날 때까지 계속됩니다. 이렇게 프로토타입 체인을 통해 속성을 찾는 방식을 프로토타입 상속이라고 합니다.

프로토타입과 상속을 활용한 예시 코드를 봅시다 🙂

```js
function Family(name, part) {
    this.name = name;
    this.part = part;
}

Family.prototype.partInfo = function() {
    console.log('내 이름은 ' + this.name + ', 우리집에서 ' + this.part + '을(를) 담당 중!');
}

var jeongInfo = new Family('jeong', '가장');

jeongInfo.partInfo(); // 내 이름은 jeong, 우리집에서 가장을(를) 담당 중!

function Animal(name, part, kind) {
    Family.call(this, name, part);
    this.kind = kind;
}

Animal.prototype = Object.create(Family.prototype);
Animal.prototype.constructor = Animal;

Animal.prototype.kindInfo = function() {
    console.log('나는 ' + this.kind);
}

Animal.prototype.allInfo = function() {
    Family.prototype.partInfo.call(this);
    console.log('참고로 나는 ' + this.kind);
}

var merryInfo = new Animal('메리', '금수', '강아지');
var dalpongInfo = new Animal('달퐁', '금수', '고양이');

merryInfo.partInfo(); // 내 이름은 메리, 우리집에서 금수을(를) 담당 중!
dalpongInfo.allInfo(); // 내 이름은 달퐁, 우리집에서 금수을(를) 담당 중! 참고로 나는 고양이
```

결과를 보니 this는 실행 컨텍스트에 따라 변하는 것을 확인할 수 있었습니다.  
프로토타입 체인을 사용하여 메소드를 상속받아도 this는 호출한 객체를 바라봅니다.  
this를 잘 활용하려면 호출 시점을 잘 파악해야 하겠죠?    

그리고 es6부터 class 키워드가 도입되어, 클래스 기반 언어처럼 비슷하게 코드를 짤 수 있게 되었습니다! 와~  
하지만 여전히 내부에선 프로토타입 기반으로 동작합니다.  
아래는 class를 사용한 예시 코드 입니다.  

```js
class Family {
    constructor(name, part) {
        this.name = name;
        this.part = part;
    }

    partInfo() {
        console.log('내 이름은 ' + this.name + ', 우리집에서 ' + this.part + '을(를) 담당 중!');
    }
}

let jeongInfo = new Family('jeong', '가장');

jeongInfo.partInfo(); // 내 이름은 jeong, 우리집에서 가장을(를) 담당 중!

class Animal extends Family {
    constructor(name, part, kind) {
        super(name);
        this.part = part;
        this.kind = kind;
    }

    kindInfo() {
        console.log('나는 ' + this.kind);
    }

    allInfo(){
        super.partInfo();
        console.log('참고로 나는 ' + this.kind);
    }
}

let merryInfo = new Animal('메리', '금수', '강아지');
let dalpongInfo = new Animal('달퐁', '금수', '고양이');

merryInfo.partInfo(); // 내 이름은 메리, 우리집에서 금수을(를) 담당 중!
dalpongInfo.allInfo(); // 내 이름은 달퐁, 우리집에서 금수을(를) 담당 중! 참고로 나는 고양이
```

ㅋㅋ  
class를 사용하니 확실히 this를 파악하기 편해진 것 같습니다..^^  


### 🧐 프로토타입으로 할 수 있는 것
---

class와 prototype은 상속을 통해 속성 및 메서드를 자식이 재사용할 수 있다는 공통점을 가지고 있지만 명확한 차이가 있어요.    
예를들어 자바의 클래스는 컴파일 시점에 정의되어 더이상 수정할 수 없는 정적 객체이지만, 자바스크립트의 프로토타입은 동적이기 때문에 코드가 실행되는 와중에도 객체의 동작을 동적으로 변경할 수 있습니다.  
왜 이렇게 다르게 만들어졌는가? 라는 의문이 든다면 해당 언어가 추구하는 철학을 이해해야하죠...  
자바로 프로그램을 짤 때엔 엄격한 구조와 규칙을 지켰어야 했지만  
(규칙을 지키지 않으면 컴파일부터 되지 않는--;;;)   
자바스크립트는 그런 것이 없죠... 자유로운게.. 참 알면 알수록 머리아프고 매력적인 언어입니다..~  