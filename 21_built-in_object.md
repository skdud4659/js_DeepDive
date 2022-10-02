# 빌트인 객체
## 자바스크립트 객체의 분류
1. 표준 빌트인 객체
- Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 개의 표준 빌트인 객체를 제공한다.
- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
- 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.
2. 호스트 객체
3. 사용자 정의 객체

## 원시값과 래퍼 객체
- 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작한다.
```js
const str = 'hello'

console.log(str.length) // 5
console.log(str.toUpperCase()) // HELLO
```
- 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

> 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체(wrapper object)라 한다.

- String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장하지도 않는다.
- 문자열, 숫자, 불리언, 심벌 이외의 원시값, 즉 null과 undefined는 래퍼 객체를 생성하지 않는다.

## 전역 객체
- 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.
- 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다.
  - 브라우저 환경에서는 window(또는 self, this, frames)가 전역 객체를 가리키지만 Node.js환경에서는 global이 전역 객체를 가리킨다.
> #### globalThis
> - ES11에서 도입된 globalThis는 브라우저 환경과 Node.js 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자다.

- 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체다.
- 전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

### 전역 객체의 특징
- 전역 객체는 개발자가 의도적으로 생성할 수 없다.
  - 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- 전역 객체는 프로퍼티를 참조할 때 window를 생략할 수 있다.
- 전역 객체는 Object, String, Number, Boolean, Function, Array, RegExp, Date, Math, Promise 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 추가적으로 프로포티와 메서드를 갖는다.
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
```js
// var 키워드로 선언한 전역 변수
var foo = 1
console.log(window.foo) // 1

// 선언하지 않은 변수에 값을 암묵적 전역, bar은 전경 변수가 아니라 전역 객체의 프로퍼티다.
bar = 2 // window.bar = 2
console.log(window.bar) // 2

// 전역 함수
function baz() { return 3 }
console.log(window.baz()) // 3
```
- let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
  - window.foo와 같이 접근할 수 없다.
  - let이나 const 키워드로 선언한 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.
```js
let foo = 123
console.log(window.foo) // undefined
```
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다.
  - 여러 개의 script 태그를 통해 자바스크립트 코드를 분리해도 하나의 전역 객체 window를 공유하는 것은 변함이 없다.

### 빌트인 전역 프로퍼티
#### Infinity
- 무한대를 나타내는 숫자값 Infinity를 갖는다.
#### NaN
- 숫자가 아님을 나타내는 숫자값 NaN을 갖는다.
#### undefined
- 원시 타입 undefined를 값으로 갖는다.

### 빌트인 전역 함수
#### eval
- 전달받은 문자열 코드가 표현식이라면 문자열 코드를 런타임에 평가하여 값을 생성
- 전달받은 인수가 표현식이 아닌 문이라면 문자열 코드를 런타임에 실행
- 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행

#### isFinite
- 전달받은 인수가 정상적인 유한수인지 검사하여 boolean으로 반환
#### isNaN
- 전달받은 인수가 NaN인지 검사가혀 그 결과를 boolean으로 반환
#### parseFloat
- 전달받은 문자열 인수를 실수로 해석하여 반환
#### parseInt
- 전달받은 문자열 인수를 정수로 해석하여 반환
#### encodeURI / decodeURI
- 완전한 URI를 문자열로 전달받아 인코딩 또는 디코딩
#### encodeURIComponent / decodeURIComponent
- URI 구성 요소를 인수로 전달받아 인코딩 또는 디코딩

### 암묵적 전역
```js
// 전역 변수 x는 호이스팅이 발생한다.
console.log(x) // undefined
// 전역 객체의 프로퍼티인 y는 호이스팅이 발생하지 않는다.
console.log(y) // ReferenceError

var x = 10
function foo() {
  // 선언하지 않은 식별자에 값을 할당
  // 전역 객체의 프로퍼티가 마치 전역 변수처럼 동작한다. = 암묵적 전역
  y = 20 // window.y = 20
}
foo()

console.log(x+y) // 30
```