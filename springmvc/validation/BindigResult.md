### BindingResult

```java
@PostMapping("/add")
public String addItemV1(@ModelAttribute Item item,BindingResult bindingResult,RedirectAttributes redirectAttributes,Model model){

        //검증 로직
        if(!StringUtils.hasText(item.getItemName())){
        bindingResult.addError(new FieldError("item","itemName","상품 이름은 필수입니다."));
        }

        //특정 필드가 아닌 복합 룰 검증
        if(item.getPrice()!=null&&item.getQuantity()!=null){
        int resultPrice=item.getPrice()*item.getQuantity();
        if(resultPrice< 10000){
        bindingResult.addError(new ObjectError("item","가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값: "+resultPrice));
        }
        }

        //검증에 실패하면 다시 입력 폼으로
        if(bindingResult.hasErrors()){
        log.info("errors={}",bindingResult);
        return"validation/v2/addForm";
        }

        //성공 로직

        Item savedItem=itemRepository.save(item);
        redirectAttributes.addAttribute("itemId",savedItem.getId());
        redirectAttributes.addAttribute("status",true);
        return"redirect:/validation/v2/items/{itemId}";
        }
```

BindingResult: 스프링이 제공하는 검증오류를 보관할수 있는 객체 검증 오류가 발생하면 여기다가 보관하면된다.   
Model에 자동으로 보관되어 view에서 사용가능   
모델에 값을 넣지 못하는 상황이여도 BindingResult가 있으면 컨트롤러는 호출이 된다.

*** @ModelAttribute Item item 다음에 와야된다.(검증할 대상 바로 뒤에 위치해 있어야한다.)

hasErrors(): BindingResult객체에 오류가 담겨있는지 확인하는 메서드 boolean

addError(): 오류정보를 추가하는 메서드   
필드 에러: addError(new FieldError("모델에 담긴 객체", "필드명", "에러 메시지"))   
글로벌 에러(특정 필드를 넘어선 에러): addError(new ObjectError("모델에 담긴 객체", "에러 메시지"))

---

### 타임리프와 연동

#### 글로벌 에러

```html

<div th:if="${#fields.hasGlobalErrors()}">
    <p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">글로벌 오류 메시지</p>
</div>
```

`#fields.hasGlobalErrors()로 글로벌 에러 접근 가능   
globalErrors()로 반복문을 사용할수 있다.

#### 필드 에러

```html

<form action="item.html" th:action th:object="${item}" method="post">
    <input type="text" id="itemName" th:field="*{itemName}"
           th:errorclass="field-error"
           class="form-control" placeholder="이름을 입력하세요">
    <div class="field-error" th:errors="*{itemName}">
        상품명 오류
    </div>
```
th:field="*{itemName}"처럼 field를 사용하면 해당 필드에 에러가 발생하였을때
th:errorclass에 값을 class에 넣어준다.

th:errors="*{itemName}"은 해당 필드에 에러가 발생하였을때만 태그를 보여준다.
