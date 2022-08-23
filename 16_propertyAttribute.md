# 프로퍼티 어트리뷰트
## 내부 슬롯과 내부 메서드
- 프로퍼티 어트리뷰트를 이해하기 위해 먼저 알아보자.
- ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드
- 이중 대괄호([[...]])로 감싼 이름들이 해당된다.
- 자바스크립트 엔젠의 내부 로직으로 원칙적으로 자바스크립트는 내부 슬롯과 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않으나 일부 제공한다.
> ☝🏻 원칙적으로 [[Prototype]] 내부 슬롯의 경우 직접 접근할 수 없지만 간접적으로 접근할 수 있다.
```js
const o = {};
// o.[[Prototype]] // 직접 접근 불가
o.__proto__ // 일부 간접적으로 접근할 수 있는 수단을 제공한다.
```
## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
- 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
> ☝🏻 프로퍼티의 상태?   
>- 프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)

- 프로퍼티 어트리뷰트 : 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯
  - = [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]
> 🔥 내부 슬롯이기에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor메서드를 사용해 간접으로 확인이 가능하다
### Object.getOwnPropertyDescriptor 메서드 호출
- 첫 번째 매개변수에는 객체의 참조를 전달하고 두 번째 매개변수에는 프로퍼티 키를 문자열로 전달한다.
  - 프로퍼티 디스크립터 객체를 반환한다.
```js
const person = {
    name : 'lee'
};
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
//{value : 'lee', writable: true, enumerable: true, configurable: true}
```
- ES8에 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
```js
const person = {
    name : 'lee'
};
// 프로퍼티 동적 생성
person.age = 20;
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
name: {value : 'lee', writable: true, enumerable: true, configurable: true},
age: {value : 20, writable: true, enumerable: true, configurable: true}
}
*/
```
## 데이터 프로퍼티와 접근자 프로퍼티
### 데이터 프로퍼티
- 키와 값으로 구성된 일반적인 프로퍼티
- 데이터 프로퍼티는 프로퍼티 어트리뷰트를 가지며 이는 프로퍼티를 생성할 때 자동 정의된다.
  - [[Value]] : 키를 통한 값
  - [[Writable]] : 값의 변경 가능 여부(boolean)
  - [[Enumerable]] : 프로퍼티의 열거 가능 여부(boolean)
  - [[Configurable]] : 프로퍼티의 재정의 가능 여부(boolean)
- 프로퍼티가 생성될 때 value의 값은 프로퍼티 값으로 초기화되며 나머지의 값은 true로 초기화된다.

### 접근자 프로퍼티
- 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티
- 접근자 프로퍼티는 아래와 같은 프로퍼티 어트리뷰트를 갖는다.
  - [[Get]] : 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 > getter 함수가 호출되고 그 결과가 프로퍼티의 값으로 반환
  - [[Set]] : 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 > setter 함수가 호출되고 그 결과가 프로퍼티의 값으로 저장
  - [[Enumerable]] : 프로퍼티의 열거 가능 여부(boolean)
  - [[Configurable]] : 프로퍼티의 재정의 가능 여부(boolean)
- 접근자 함수는 getter/setter 함수라고도 부르며 접근자 프로퍼티는 이를 모두 정의할 수도 있고 하나만 정의할 수도 있다.
> ☝🏻메서드 앞에 get, set이 붙는 메서드가 있는데 이것들이 바로 getter, setter 함수이고, getter/setter 함수의 이름 fullName이 접근자 프로퍼티다.

- 접근자 프로퍼티는 자체적으로 값(프로퍼티 어트리뷰트[[Value]])을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여할 뿐이다.
```js
const person = {
  // 데이터 프로퍼티
  firstName : 'ny',
  lastName : 'kim',
  
  // 접근자 프로퍼티
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split('');
  }
};

// 데이터 프로퍼티
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
// {value: 'ny', writable: true, enumerable: true, configurable: true}

// 접근자 프로퍼티
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
// {get: f, set: f, enumerable: false, configurable: true}

```
#### 동작 원리
1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이여야 한다. 프로퍼티 키 'fullName'은 문자열이므로 유효한 프로퍼티 키다.
2. 프로퍼타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다. fullName 프로퍼티는 접근자 프로퍼티다.
4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[GET]]의 값, 즉 getter 함수를 호출하여 그 결과를 반환한다.   
프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get 프로퍼티 값과 같다.

> 프로토타입(prototype)
> - 어떤 객체의 상위(부모) 객체의 역할을 하는 객체
> - 하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속
> - 상속받은 하위 객체는 자신의 프로퍼티 또는 메서드인 것처럼 자유롭게 사용 가능
> - 프로토타입 체인은 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조를 뜻함

```js
// 구벌볍
// 일반 객체의 __proto__는 접근자 프로퍼티다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__')
// {get: f, set: f, enumerable: false, configurable: true}

// 함수 객체의 prototype은 데이터 프로퍼티다.
Object.getOwnPropertyDescriptor(function() {}, 'prototype')
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

## 프로퍼티 정의
- 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것
  - 예를 들어, 프로퍼티 값을 갱신 가능하도록 할 것인지, 프로퍼티를 열거 가능하도록 할 것인지, 프로퍼티를 재정의 가능하도록 할 것인지 정의할 수 있다.
- Object.defineProperty 메서드를 사용하여 정의할 수 있다.
```js
const person = {};

Object.defineProperty(person, 'firstName', {
  value: 'ny',
  writable: true,
  enumerable: true,
  configurable: true
});

Object.defineProperty(person, 'lastName', {
    value: 'kim'
});
```
- 여러개의 프로퍼티를 정의하려면 Object.defineProperties
```js
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티의 정의
  firstName: {
  value: 'ny',
  writable: true,
  enumerable: true,
  configurable: true
  },
  lastName: {
  value: 'kim',
  writable: true,
  enumerable: true,
  configurable: true
  },

  // 접근자 프로퍼티 정의
  fullName: {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  set(name) {
    [this.firstName, this.lastName] = name.split('');
  },
  enumerable: true,
  configurable: true
  }
});
```

## 객체 변경 방지
- 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다.
  - = 프로퍼티를 추가하거나 삭제할 수 있고 프로퍼티 값을 갱신할 수 있다.
  - = Object.defineProperty 또는 Object.defineProperties를 사용해 프로퍼티 어트리뷰트를 재정의할 수 있다.

| 구분       | 메서드                      | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
|---------|--------------------------|--------- |---------| --------- | --------- | --------- |
| 객체 확장 금지 | Object.preventExtensions | X | O       | O | O | O |
| 객체 밀봉    | Object.seal              | X | X       | O | O | X |
| 객체 동결    | Object.freeze            | X | X       | O | X | X |

### 객체 확장 금지
- Object.preventExtensions
- 확장이 금지된 객체는 프로퍼티 추가가 금지된다 > 삭제는 가능
  - 프로퍼티는 프로퍼티 동적 추가와 Object.defineProperty 메서드로 추가할 수 있으며 이 두가지 추가 방법이 모두 금지된다.
- 확장 가능한 객체 확인 여부는 Object.isExtensible 메서드로 확인할 수 있다.
```js
const person = {name: 'lee'};

console.log(Object.isExtensible(person)); // true

Object.preventExtensions(person);

console.log(Object.isExtensible(person)); // false

person.age = 20; // 무시, strict mode에서는 에러
console.log(person); // {name: 'lee'}

// 프로퍼티 추가는 금지되지만 삭제는 가능하다
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, 'age', {value: 20}); // TypeError
```

### 객체 밀봉
- Object.seal
- 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 의미
  - 밀봉된 객체는 읽기와 쓰기만 가능하다 > 갱신은 가능
- Object.isSealed 메서드로 확인할 수 있다.
```js
const person = {name: 'lee'};

console.log(Object.isSealed(person)); // false

Object.seal(person);

console.log(Object.isSealed(person)); // true

// 밀봉된 객체는 configurable이 false이다.
console.log(Object.getOwnPropertyDescriptors(person));
// {name: {value: 'Lee', writable: true, enumerable: true, configurable: false}}

// 프로퍼티 추가 금지
person.age = 20; // 무시, strict mode에서는 에러
console.log(person); // {name: 'lee'}

// 프로퍼티 삭제 금지
delete person.name; // 무시, strict mode에서는 에러
console.log(person); // {name: 'lee'}

// 프로퍼티 값 갱신은 가능
person.name = 'kim';
console.log(person); // {name: 'kim'}

// 프로퍼티 어트리뷰트 재정의 금지
Object.defineProperty(person, 'name', {configurable: true}); // TypeError
```

### 객체 동결
- Object.freeze
- 동결된 객체는 읽기만 가능하다
- Object.isFrozen 메서드로 확인할 수 있다.
```js
const person = {name: 'lee'};

console.log(Object.isFrozen(person)); // false

Object.freeze(person);

console.log(Object.isFrozen(person)); // true

// 동결된 객체는 writable과 configurable이 false이다.
console.log(Object.getOwnPropertyDescriptors(person));
// {name: {value: 'Lee', writable: false, enumerable: true, configurable: false}}

// 프로퍼티 추가 금지
person.age = 20; // 무시, strict mode에서는 에러
console.log(person); // {name: 'lee'}

// 프로퍼티 삭제 금지
delete person.name; // 무시, strict mode에서는 에러
console.log(person); // {name: 'lee'}

// 프로퍼티 값 갱신 금지
person.name = 'kim'; // 무시, strict mode에서는 에러
console.log(person); // {name: 'lee'}

// 프로퍼티 어트리뷰트 재정의 금지
Object.defineProperty(person, 'name', {configurable: true}); // TypeError
```

### 불변 객체
- 위의 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못하여 Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다.
```js
const person = {
    name: 'Lee',
    address: {city: 'seoul'}
};

// 얕은 객체 동결
Object.freeze(person);

// 직속 프로퍼티만 동결
console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결하지 못한다.
console.log(Object.isFrozen(person.address)) // false

person.address.city = 'busan';
console.log(person); // {name: 'lee', address: {city: 'busan}}
```
- 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 메서드를 호출해야 한다.
```js
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if(target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    // 모든 프로퍼티를 순회하며 재귀적으로 동결한다.
    Object.keys(target).forEach(key => deepFreeze(target[key]));
  }
  return target;
};

const persont = {
    name: 'lee',
    address: {city: 'seoul'}
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결
console.log(Object.isFrozen(person.address)) // true

person.address.city = 'busan';
console.log(person); // {name: 'lee', address: {city: 'seoul'}}
```
