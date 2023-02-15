---
layout: post
title: RestTemplate 으로 API 호출하기
categories: [Study]
tags: [JAVA, Spring]
---


외부 서버에 데이터 전달해줄 일이 있어 RestTemplate 을 사용해 작업하였다.  
RestTemplate 사용을 위해 의존성 추가를 해준다.  

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
```

**[스프링부트 기반일 경우 빈을 자동으로 주입해주기 때문에 빈 설정을 따로 할 필요가 없지만](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/autoconfigure/web/client/RestTemplateAutoConfiguration.html)**<br>
필자의 프로젝트는 레거시라 빈 설정을 해줘야했다 ! !  

```java
@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }

}
```

빈 설정 클래스를 생성해주었다  

```java
public ResponseEntity<String> send(SnstemplateVo snstemplateVo) throws JsonProcessingException {

        HttpHeaders headers = new HttpHeaders();
        headers.add("Content-Type", APPLICATION_JSON_VALUE);
        headers.add("User-Agent", userAgent);

        Sms sms = Sms.of(snstemplateVo);
        HttpEntity<String> entity = new HttpEntity<>(objectMapper.writeValueAsString(sms), headers);

        return restTemplate.exchange(url, HttpMethod.POST, entity, String.class);
}
```

그리고 서비스쪽에 RestTemplate 와 ResponseEntity 를 이용한 API 호출 코드를 작성해주었다.  
ResponseEntity 는 HttpEntity를 상속 받아 HttpStatus, HttpHeader 와 Httpbody 를 가질 수 있다.  
반환 값은 String 으로 설정했당 😎  

```java
public class ResponseEntity<T> extends HttpEntity<T> {
    private final Object status;

    ....
```

ResponseEntity 클래스 쪽을 보면 HttpEntity 상속받은 것을 확인할 수 있다 😀

- - -

```java
public ResponseEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers, HttpStatus status) {
        this(body, headers, (Object)status);
}
```

그외 여러가지 동작 하는 코드를 볼 수 있다. body는 여러 타입을 가질 수 있구나!

- - - 

```java
public ResponseEntity<String> send(SnstemplateVo snstemplateVo){

        MultiValueMap<String, String> sms = new LinkedMultiValueMap<>();
        sms.add("snsContent", snstemplateVo.getSnsContent());
        sms.add("senderName", snstemplateVo.getSenderName());
        sms.add("senderNumber", snstemplateVo.getSenderNumber());
        sms.add("receiverName", snstemplateVo.getReceiverName());
        sms.add("receiverNumber", snstemplateVo.getReceiverNumber());

        HttpHeaders headers = new HttpHeaders();
        headers.add("Content-Type", "application/json");
        headers.add("user-agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36");
        HttpEntity<MultiValueMap<String, String>> entity = new HttpEntity<>(sms, headers);

        String url = "http://localhost:8080/api/sms/send/new";

        return restTemplate.exchange(url, HttpMethod.POST, entity, String.class);
}
```
위 코드를 다른 방법으로 작성했다.  
위에 있던 첫번째 코드는 objectMapper 를 이용했고 
바로 위에 존재하는 두번째 코드는 MultiValueMap를 이용했당

MutilValueMap 를 이용해 파라미터 값을 넣어주고 HttpHeaders 에 Content-Type 정보를 넣어준다.

HashMap 이 아닌 MutilValueMap 사용하는 이유는  

```java
public RestTemplate() {
    this.messageConverters = new ArrayList();
    this.errorHandler = new DefaultResponseErrorHandler();
    this.headersExtractor = new HeadersExtractor();
    this.messageConverters.add(new ByteArrayHttpMessageConverter());
    this.messageConverters.add(new StringHttpMessageConverter());
    this.messageConverters.add(new ResourceHttpMessageConverter(false));
    if (!shouldIgnoreXml) {
        try {
            this.messageConverters.add(new SourceHttpMessageConverter());
        } catch (Error var2) {
        }
    }

    this.messageConverters.add(new AllEncompassingFormHttpMessageConverter());
    if (romePresent) {
        this.messageConverters.add(new AtomFeedHttpMessageConverter());
        this.messageConverters.add(new RssChannelHttpMessageConverter());
    }

    if (!shouldIgnoreXml) {
        if (jackson2XmlPresent) {
            this.messageConverters.add(new MappingJackson2XmlHttpMessageConverter());
        } else if (jaxb2Present) {
            this.messageConverters.add(new Jaxb2RootElementHttpMessageConverter());
        }
    }

    if (jackson2Present) {
        this.messageConverters.add(new MappingJackson2HttpMessageConverter());
    } else if (gsonPresent) {
        this.messageConverters.add(new GsonHttpMessageConverter());
    } else if (jsonbPresent) {
        this.messageConverters.add(new JsonbHttpMessageConverter());
    } else if (kotlinSerializationJsonPresent) {
        this.messageConverters.add(new KotlinSerializationJsonHttpMessageConverter());
    }

    if (jackson2SmilePresent) {
        this.messageConverters.add(new MappingJackson2SmileHttpMessageConverter());
    }

    if (jackson2CborPresent) {
        this.messageConverters.add(new MappingJackson2CborHttpMessageConverter());
    }

    this.uriTemplateHandler = initUriTemplateHandler();
}
```

MutilValueMap 를 컨버터 해주는 AllEncompassingFormHttpMessageConverter 가 존재하기 때문이다.  
HashMap은 디폴트로 등록이 안되어있음ㅎ..  

어쨌든 이렇게 저렇게 작성을 해서!!!  
데이터를 보냈는데... 
보냈는데...  

**cannot be cast to java.lang.String** <br>
에러가 뜨고만 것 🥲  


```java
@PostMapping("/new" )
public ResponseEntity<String> message(@RequestBody Map<String, Object> requestObj, HttpServletRequest request) {

    New new = new New();

    new.setSenderNumber((String) requestObj.get("senderNumber"));
```

new.setSenderNumber((String) requestObj.get("senderNumber"));  
이 부분에서 에러가 나는데.......... requestObj.get("senderNumber") 이 배열로 감싸져 들어와서 그랬던 것이었다..!!!  

MutilValueMap이 한 key에 여러가지 value를 넣어줄 수 있어서 그런 거 같은데.... 흠,,  
정확한 확인을 위해 MutilValueMap class를 보자  


```java
public class MultiValueMapAdapter<K, V> implements MultiValueMap<K, V>, Serializable {
	...

	public void add(K key, @Nullable V value) {
	        List<V> values = (List)this.targetMap.computeIfAbsent(key, (k) -> {
	            return new ArrayList(1);
	        });
	        values.add(value);
	}
	...

	public void set(K key, @Nullable V value) {
	    List<V> values = new ArrayList(1);
	    values.add(value);
	    this.targetMap.put(key, values);
	}
```

ArrayList 로 value를 반환 하는구나  
😱

```java
new.setSenderNumber(String.valueOf(requestObj.get("senderNumber")));
```

로 고쳐주자.  
그럼 오류없이 데이터가 잘 들어온당 ✌️  

### ⭐️ String.valueOf & toString & Casting 의 차이점 ⭐️  

Casting - (String) 변수가 null이면 문자열 "null"을 반환한다.  

**Casting 사용 할 때 주의점**
해당 코드 유형이 아닌 다른 유형을 참조할 때 예외(ClassCastException)가 발생한다.  
casting 사용을 위해 상속을 공부하자 . . .  
상속관계를 이루는 클래스끼리 형변환을 할 수 있다. 

* `자식 타입 -> 부모 타입` : 형변환 자동
* `부모 타입 -> 자식 타입` : 형변환 해줘야함

`toString` : 해당 값이 null 이면 NullPointerException 발생시키고 값이 없어도 string으로 값을 뱉는다.
`String.valueOf` : 해당 값이 null 이면 null 값을 반환한다. 예외처리 할 때 좋다!

