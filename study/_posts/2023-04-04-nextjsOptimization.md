---
layout: post
title: Next.js로 작업한 프로젝트 최적화하기
categories: [Study]
tags: [Next]
feature-img: "assets/img/thum/next.png"
---

### 🧐 **웹 사이트 최적화, 왜 중요한가?**

우리는 웹 사이트를 통해 사용자에게 서비스를 제공합니다. 서비스가 잘 되려면 더 많은 사용자를 모아야 하는데 페이지가 느리고 사용하기 어렵다면 사용자들은 답답함을 느끼고 사이트를 떠나게 되는 불상사가 발생할 거예요.
그런 불상사가 발생하지 않도록 우리는 웹 사이트 최적화에 대해 많은 고민을 해야합니다. 🙂

해당 프로젝트는 구글에서 제공하는 `LightHouse`로 성능을 측정하였습니다.  
`LightHouse`는 **성능, 접근성, 권장사항, 검색엔진 최적화를 체크**해줍니다.  



### **⚙️ 성능 테스트 진행하기**  

![KakaoTalk_Photo_2023-04-12-20-46-57.png](/assets/img/opt-01.png)

초기 성능 테스트 결과입니다. 

웹 표준을 지켜 마크업을 진행하였기 때문에 접근성과 검색엔진 최적화에 높은 점수를 받았지만  
성능 점수가 현저히 낮았기 때문에 성능에 대한 개선이 필요 했습니다 🥲  
웹 최적화에 많은 기능을 제공하는 Next.js로 프로젝트를 진행 하였음에도 불구하고 처참한 점수가 나왔습니다.  

대체 왜 이런 점수가 나온 걸까요? 🤔  

원인을 알아봅시다.  

![KakaoTalk_Photo_2023-04-12-20-51-30.png](/assets/img/opt-02.png)

LightHouse는 어느 부분이 성능 저하의 원인이 되었는 지 알려줍니다.  
해당 프로젝트의 경우 **Largest Contentful Paint로 인해 낮은 점수를 받게 되었다는 것**을 확인하였습니다.  
LCP는 **로딩 속도의 지표**를 나타냅니다. 그렇다면 로드 되는 파일 용량을 확인해봐야 합니다.  

![KakaoTalk_Photo_2023-04-12-20-52-53.png](/assets/img/opt-03.png)
 
빌드된 main js 파일이 1MB가 넘는 것을 확인 하였습니다.  
작업량을 생각하면 저정도의 크기가 나올 수가 없는데 이상합니다 😨  
프로젝트 빌드 결과물을 확인해봅시다.  



### 🌏 bundle-analyzer 사용하기

`Bundle Analyzer`는 웹 애플리케이션에서 사용되는 자바스크립트 및 CSS 파일과 같은 리소스 파일의 번들 크기를 시각적으로 분석하는 도구입니다.  
`Bundle Analyzer`를 사용하면 번들 파일의 크기를 파악하고, 파일 크기가 큰 모듈을 찾아 최적화할 수 있습니다.  


**🛠️ bundle-analyzer 사용을 위한 설정**

```bash
npm install @next/bundle-analyzer
```

해당 프로젝트에 @next/bundle-analyzer을 설치합니다.  

```jsx
const withBundleAnalyzer = require('@next/bundle-analyzer')({
	enabled: process.env.ANALYZE === 'true'
})

const nextConfig = {
	...
}

module.exports = withBundleAnalyzer(nextConfig);
```

next.config.js 에 @next/bundle-analyzer을 require 합니다.

```jsx
"build": "ANALYZE=true NODE_ENV=production next build && next export",
```

빌드 명령어를 변경합니다.

![KakaoTalk_Photo_2023-04-14-16-21-50.png](/assets/img/opt-04.png)

bundle-analyzer 사용을 위한 셋팅을 완료한 뒤 빌드를 진행하면 빌드 결과를 시각적으로 확인할 수 있게 됩니다 🙂
어떠한 라이브러리의 일부 코드를 사용했는데 빌드된 파일이 너무 크다면 Tree Shaking 설정으로 웹 최적화를 진행 해줍니다. 
해당 프로젝트는 Next.js 12 버전입니다. 12 버전에 도입된 swc로 빌드를 진행하였습니다 🙂 
(swc의 자세한 설명은 다음에 포스팅 할 예정입니다)

```js
module.exports = {
  swcMinify: true,
};
```

config 설정에 추가한 뒤 빌드를 해준 뒤 배포를 하면  

![KakaoTalk_Photo_2023-04-14-16-21-50.png](/assets/img/opt-05.png)

멋진 최적화 점수가 나옵니다 🎉  


### 🐳 빠른 웹 사이트는 고래도 춤추게 한다.  

이전보다 빨라진 웹 사이트를 보니 속이 후련하다고 해야할까요 😁  
이것저것 신경써주어야 할 것이 많았지만 모든 사용자 분들이 저의 웹 사이트를 편하게 이용해주셨으면 합니다.  
포스팅을 읽어주셔서 감사합니다 😀
