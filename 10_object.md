# 객체
- 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 구성하는 거의 '모든 것'이 객체이다.
- 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조이며 객체는 변경 가능한 값이다.
- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성된다.
```js
var person = {
name: 'lee',  //프로퍼티 (키:값)
age: 20 ,     
increase: function() {
    this.num++
} // 메소드
};
```
> ☝🏻 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드(method)라 부른다.
- 프로퍼티 : 객체의 상태를 나타내는 값(data) > 객체 내의 변수
- 메서드 : 프로퍼티를 참조하고 조작할 수 있는 동작 > 객체 내의 함수

## 객체 리터럴에 의한 객체 생성
- 객체지향 프로그래밍에서 객체는 클래스와 인스턴스를 포함한 개념이다.
> 인스턴스 : 클래스에 의해 생성되어 메모리에 저장된 실체

- 객체 리터럴은 중괄호 내에서 0개 이상의 프로퍼티를 정의한다.
  - 객체 리터럴에서의 중괄호는 코드 블록을 의미하지 않는다.
- 객체를 생성하기 위해 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요가 없다.
만약, 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.
```js
var person = {
  name : 'lee',
  sayHello: function() {
  console.log(`Hello! my name is ${this.name}.`);
  }
};
console.log(type of person); //object
console.log(person); //{name: 'lee', sayHello: f}
```

## 프로퍼티
- 객체는 프로퍼티의 집합이며 프로퍼티는 키와 값으로 구성된다.
- 프로퍼티를 나열할 때는 쉼표로 구분하며 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않으나 사용해도 상관없다.
- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
  - 프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름인 경우 따옴표를 생략할 수 있다.
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값
```js
var person = {
  firstname : 'nayeong', //식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Kim'     //식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};
```

- 문자열 또는 문자열로 평가할 수 있는 표현식을 사용하여 프로퍼티 키를 동적으로 생성할 수 있으며 이 경우 대괄호로 묶어야 한다.
```js
var obj = {};
var key = 'hello';
//ES5 : 프로퍼티 키 동적 생성
obj[key] = 'world';
//ES6 : 계산된 프로퍼티 이름
var obj = {[key]:'world'};
```
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓰며 이 때 에러가 발생하지 않는다.
```js
var foo = {
    name: 'Lee',
    name: 'Kim'
}
console.log(foo) // {name: 'Kim'}
```
## 프로퍼티 접근
- 자바스크립트에서 사용 가능한 유효한 이름이면 마침표 표기법과 대괄호 표기법 모두 사용할 수 있다.
```js
var person = {
  name : 'lee'
};
//마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); //lee
//대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name'] //lee
```
> 대괄호 표기법을 사용하는 경우 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이여야 하며 그렇지 않은 경우 식별자로 해석한다.
또한, 프로퍼티 키가 네이밍 규칙을 준수하지 않은 이름일 경우 반드시 대괄호 표기법을 사용해야 한다.

## 프로퍼티 값 갱신
- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.
```js
var person = {
  name : 'lee'
};
person.name='kim';
console.log(person); // {name : 'kim'}
```
## 프로퍼티 동적 생성
- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.
```js
var person = {
  name : 'lee'
};
person.age=20;
console.log(person); // {name : 'lee', age:20 }
```
## 프로퍼티 삭제
- delete 연산자는 객체의 프로퍼티를 삭제하며 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.
```js
var person = {
  name : 'lee'
};
person.age=20;
delete person.age;
delete person.address;
console.log(person); // {name : 'lee'}
//객체에 address프로퍼티가 존재하지 않기 때문에 에러발생없이 그냥 무시되었다.
```

## ES6에서 추가된 객체 리터럴의 확장 기능
### 프로퍼티 축약 표현
- 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다.
이때 프로퍼티 키는 변수 이름으로 자동 생성된다.
```js
let x = 1, y = 2;
const obj = {x, y};
console.log(obj); //{x:1, y:2}
```
### 계산된 프로퍼티 이름
```js
const prefix = 'prop';
let i = 0;

const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1:1, prop-2:2, prop-3:3}
```
### 메서드 축약 표현
- function 축약
```js
//ES5
var obj = {
  name: 'lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
}
};
obj.sayHi(); //Hi! lee

//ES6
const obj = {
  name: 'lee',
  //메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};
obj.sayHi(); //Hi! lee
```
