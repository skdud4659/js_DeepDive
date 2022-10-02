# strict mode
- 자바스트립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

## 적용
- 전역의 선두 또는 함수 몸체의 선두에 'use strict'를 추가한다.
---
- 전역의 선두에 추가하면 스크립트 전체에 적용된다.
```js
'use strict'

function foo() {
  x = 10 // ReferenceError: x is not defined
}
foo()
```
- 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 적용된다.
- 코드의 선두에 위치시키지 않으면 제대로 동작하지 않는다.
```js
function foo() {
  'use strict'
  x = 10 // ReferenceError: x is not defined
}
foo()

function foo1() {
  x = 10 // 에러를 발생시키지 않는다.
  'use strict'
}
foo1()
```

## 전역에 strict mode를 적용하는 것은 피하자
- 전역에 적용한 strict mode는 스크립트 단위로 적용된다.
```html
<!DOCTYPE html>
<html>
  <body>
    <script>
        'use strict'
    </script>
    <script>
        x = 1 // 에러가 발생하지 않는다.
        console.log(x) // 1
    </script>
    <script>
        'use strict'
        y = 1 // ReferenceError: y is not defined
    </script>
  </body>
</html>
```
- 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 적용한다.
```js
(function () {
  'use strict'
  // do sth
}())
```

## strict mode가 발생시키는 에러
### 암묵적 전역
- 선언하지 않은 변수를 참조하면 RefereceError가 발생한다.
```js
(function () {
  'use strict'
  
  x = 1
  console.log(x) // ReferenceError: x is not defined
}())
```
### 변수, 함수, 매개변수의 삭제
- delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.
```js
(function () {
  'use strict'
  
 var x = 1
  delete x // SyntaxError: Delete of an unqualified identifier in strict mode.
}())
```
### 매개변수 이름의 중복
```js
(function () {
  'use strict'
  
 // SyntaxError: Duplicate parameter name not allowed in this context
  function foo (x, x) {
    return x + x
  }
}())
```
### with 문의 사용
```js
(function () {
  'use strict'
  
 // SyntaxError: strict mode code may not include a with statement
  with({ x: 1}) {
    console.log(x)
  }
}())
```
## strict mode 적용에 의한 변화
### 일반 함수의 this
- strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다.
### arguments 객체
- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.