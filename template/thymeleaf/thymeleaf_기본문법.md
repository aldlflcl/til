### 타임리프 기본문법

```html

<html xmlns:th="http://www.thymeleaf.org">
타임리프를 사용한다는 것을 선언
```

```html
변수 표현식
${...}
ex) <p th:text="{guest.name}">hello</p>
모델에 담겨있는 값이나 타임리프 변수로 선언된 값을 조회할수 있다.
프로퍼티 접근법 사용 guest.getName()
```

```html

<link href="../css/bootstrap.min.css"
      th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
링크표현식 링크를 삽입할때는 @{}안에 넣어주어야한다.
@{} 링크 표현식을 사용하면 자동으로 서블릿 컨텍스트도 붙는다. (레거시에서 많이 사용)
```

```html
링크 표현식
<a th:href="@{/basic/items/{itemId}(itemId=${item.id})}">test</a>
itemId의 값을 소괄호 안에서 설정해줄수 있다.
/basic/items/3
<a th:href="@{/basic/items/{itemId}(itemId=${item.id}, queryParameter='test')}">test</a>
또한 쿼리파라미터의 값도 생성해준다
/basic/items/3?queryParameter=test

<a th:href="@{|/basic/items/${item.id}|}"
   이런식으로 리터럴 표현식을 이용하여 간단하게 사용 가능
```

```html

<button class="btn btn-primary float-end"
        onclick="location.href='addForm.html'"
        th:onclick="|location.href='@{/basic/items/add}'|"
        type="button">상품 등록
</button>
속성 변경 th:onclick
타임리프 뷰 템플릿을 거치게 되면 원래 속성을 th:xxx속성값으로 변경한다.
만약 값이 없다면 새로 생성

html파일을 열었을때는 기존 html속성이 사용되고 뷰 템플릿을 거치면
th:href값이 href로 대체되면서 동적으로 변경 가능
```

```html

<tr th:each="item : ${items}">

</tr>
반복 출력 th:each
모델에 있는 items 컬렉션 데이터가 item변수에 담긴다.
컬렉션의 개수만큼 하위태그를 포함해서 생성한다.
```

```html

<form action="item.html" th:action="/basic/items/add" method="post"> </form>
<form action="item.html" th:action method="post"></form>
th:action 속성값을 비워두면 현재 url이 그대로 적용이 된다.
```

```html

<button class="btn btn-primary float-end"
        onclick="location.href='addForm.html'"
        th:onclick="|location.href='@{/basic/items/add}'|">
    상품 등록
</button>

th:onclick="|location.href='@{/basic/items/add}'|"
리터럴 문법이 적용되었다.
자바스크립트의 백틱을 이용한 템플릿 리터럴과 비슷..
|템플릿 리터럴| |...| 이렇게 사용하며 표현식을 그대로 문자사용하듯이 사용가능
ex) <p th:text="'hello' + ${guest.name} + '!'">hello</p>
<p th:text="|hello ${guest.name}!"></p>
```