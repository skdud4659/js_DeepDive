# 일급 객체
### ☝🏻 일급 객체의 조건
- 무명의 리터럴로 생성할 수 있다. = 런타임에 생성이 가능하다.
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.
> 자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체다.
```js
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당
const increase = function (num) {
	return ++num;
};

const decrease = function (num) {
	return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const predicates = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(predicate) {
	let num = 0;

	return function () {
		num = predicate(num);
		return num;
	};
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const decreaser = makeCounter(predicates.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

### 함수가 일급 객체이다
  - = 함수를 객체와 동일하게 사용할 수 있다.
  - = 함수는 값을 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가된다.

### 일급 객체로서 함수의 특징
- 일반 객체와 같이 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.
### 일반 객체와의 차이
- 함수 객체는 호출할 수 있다.
- 함수 고유의 프로퍼티를 소유한다.
## 함수 객체의 프로퍼티
### arguments 프로퍼티
- 값이 arguments 객체
- 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체
- 함수 내부에서 지역 변수처럼 사용
> ☝🏻 함수 객체의 arguments 프로퍼티는 현재 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 폐지되었다.
따라서 function.arguments와 같은 사용법은 권장되지 않으며 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체를 참조하도록 해야한다.

- 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
  - 그렇다고 초과된 인수가 그냥 버려지는 게 아닌 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
- arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타낸다.
- arguments 객체의 callee 프로퍼티는 함수 자신을 가리키고 arguments 객체의 length 프로퍼티는 인수의 개수를 가리킨다.
- arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용한다.
```js
function sum() {
	let res = 0;
	
	// arguments 객체는 legnth 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있다.
	for (let i = 0; i < arguments.length; i++) {
		res += arguments[i];
	}
	return res;
}

console.log(sum()); // 0
console.log(sum(1,2)); // 3
console.log(sum(1,2,3)); // 6
```
- arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체이다.
> ☝🏻 유사 배열 객체 : length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체

### caller 프로퍼티
- ECMAScript 사양에 포함되지 않은 비표준 프로퍼티, 표준화될 예정도 없기에 사용할 일이 없다고 한ㄷ..
- 함수 자신을 호출한 함수를 가리킨다.

### length 프로퍼티
- 함수를 정의할 때 선언한 매개변수의 개수
- arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수의 개수를 가리킨다.

### name 프로퍼티
- 함수 이름
- ES6에서 정식 표준이 되어 그 전과 동작을 달리하므로 주의
- 함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 식별자로 호출
```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function() {};
// ES5 : name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6 : name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name); // bar
```

### __proto__ 접근자 프로퍼티
- [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티
- 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.
> ☝🏻 hasOwnProperty 메서드   
> - 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.

```js
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__')); // false
```

### prototype 프로퍼티
- 생성자 함수로 호출할 수 있는 함수 객체(=constructor)만이 소유하는 프로퍼티
```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype') // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype') // false
```