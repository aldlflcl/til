## http body 요청

```
    @ResponseBody => 응답 바디에 return값을 직접 넣는다.
    ex) return "ok"; => ok라는 문자열이 넘어간다.
     다른 타입일경우 Json형식으로 자동으로 변환이 된다.
```

### json

```java
@RequestBody =>요청 바디내용을 받을수 있게 해준다.
@RequestBody String data=>String 타입일 경우 바디 내용이 문자열 그대로 들어온다.
@RequestBody HelloData data=>요청 바디 내용이 Json형식이면 자동으로 매핑됨.
```

```java

@PostMapping(value = "/request-body-json")
@ResponseBody
public String requestBodyJson(@RequestBody HelloData helloData){
        log.info("helloData={}",helloData);
        log.info("username={}",helloData.getUserName(),helloData.getAge());

        return"ok";
        }
```

```java
@ResponseBody
@PostMapping(value = "/request-body-json", consumes = MediaType.APPLICATION_JSON_VALUE)
public String requestBodyJsonV4(RequestEntity<HelloData> data){
        //HttpEntity<HelloData> 를 사용해도됨
        HelloData helloData=data.getBody();
        log.info("messageBody={}",helloData);
        log.info("username={}, age={}",helloData.getUsername(),helloData.getAge());
        return"OK";
        }
```

***

### 문자열

```java
@ResponseBody
@PostMapping("/request-body - string")
public String requestBodyString(HttpEntity<String> httpEntity){
        String message=httpEntity.getBody(); // getBody()로 메시지 바디를 꺼낼수 있음
        log.info("messageBody={}",message);
        return"ok";
        }
```

***

### 요청 파라미터

```
@RequestParam => 
요청 파라미터를 받을수 있게 해준다. String, Integer, int 등 단순타입이면 생략가능
파라미터 name과 변수명이 동일하면 name 생략 가능 
ex) @RequestParam("hello") String hello => "hello"생략 가능

default옵션이 false면 해당 파라미터가 없어도 호출됨 기본값 true
true일때 요청 파라미터가 없으면 400error
false여도 int등 기본타입일경우 500error 발생 null값을 넣을수 없기 때문에 
ex ) @RequestParam("num") int num => 요청 파라미터 num이 없으면 500에러 발생(Integer)로 변경해야함
```

```java
@RepsonseBody
@RequestMapping("/request-param-map")
public String requestParamMap(@RequestParam MultiValueMap<String, Object> paramMap){
        // 일반 Map으로 직접 꺼낼수도 있고 MultiValueMap을 이용하면 한 파라미터에 여러개의 값을 꺼낼수 있음
        // ex) /members?username=lee&username=kim&age=28 일때 username과 같이 값이 2개이상인경우
        log.info("username={}, age={}",paramMap.get("username"),paramMap.get("age"));
        }
```

---

### @ModelAttribute

```
@ModelAttribute => 요청 파라미터를 이용하여 같은이름의 프로퍼티를 setter메소드를 사용하여 값을 넣어준다.
String, int, Integer등과 같은 타입이아니라 만들어진 타입이라면 ModelAttribute를 생략하여도 동작한다
하지만 협업시 생략하지말고 적는것을 권장

@ModelAttribute("name") Item item => model.addAttribute("name", item);
직접 모델에 담지 않아도 ModelAttribute를 사용하면 모델에 자동으로 담아준다.
@ModelAttribute Item item => model.addAttribute("item", item);
이름을 지정안하면 클래스명 첫글자를 소문자로 변경후 그 이름으로 모델에 담아준다.
```

```java
@RequestMapping("/model-attribute")
@ResponseBody
public String modelAttribute(@ModelAttribute HelloData helloData){
        log.info("username={}, age={}",helloData.getUsername(),helloData.getAge());
        log.info("helloData={}",helloData);

        return"ok";
        }
```

---

### RedirectAttribute(핸들러 파라미터)

```java
@PostMapping("/add")
public String addItemV6(Item item,RedirectAttributes redirectAttributes){

        Item savedItem=itemRepository.save(item);
        redirectAttributes.addAttribute("itemId",savedItem.getId());
        redirectAttributes.addAttribute("status",true);

        return"redirect:/basic/items/{itemId}";
        }

        addAttribute()메소드를 사용하면 키 밸류로 값을 저장가능하고
        return할때{...}에 추가하면 해당 url로 리다이렉트하고{...}에 사용되어지지 않으면
        url뒤에 쿼리파라미터로 붙는다
        ex)/basic/items/{itemId}?status=true
```