###URL 링크

```html
model.addAttribute("param1", "data1");
model.addAttribute("param2", "data2");

<li><a th:href="@{/hello}">basic url</a></li>
/hello
<li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
/hello?param1=data1&param2=data2
<li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
/hello/data1/data2
<li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
/hello/data1?param2=data2

url경로상에 {...}변수가 있으면 ()부분은 경로 변수로 사용됨.
경로 변수로 사용이 안되면 쿼리 파라미터로 사용된다.
```