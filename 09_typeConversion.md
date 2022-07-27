# 타입 변환
- 명시적 타입 변환 = 타입 캐스팅
  - 값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있다.
- 암묵적 타입 변환 = 타입 강제 변환
  - 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해암묵적으로 타입이자유 변환된다.
  - 타입을 변경한다해서 변수의 원시 값을 직접 변경하는 것이 아닌 기존 원시 값을 사용하여 다른 타입의 새로운 원시 값을 생성한다.
```js
var x = 10;

//명시적 타입 변환
//숫자를 문자열로 타입 캐스팅
var str = x.toString();
console.log(typeof str,str); //string 10

//암묵적 타입 변환
//숫자 타입의 x의 값을 바탕으로 새로운 문자열을 생성
var str = x + '';
console.log(typeof str,str); //string 10

//x 변수의 값이 변경된 것은 아니다.
console.log(typeof x,x); //number 10
```
## 암묵적 타입 변환
### 문자열 타입으로 변환
```js
1 + '2' = '12'
```
- 이 예제로 보면 + 연산자는 피연산자 중 하나 이상이 문자열이기 때문에 문자열 연결 연산자로 동작한다.
- 자바스크립트 엔진은 이를 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환한다.
```js
//숫자 타입
0 + ''          //'0'
-0 + ''         //'0'
1 + ''          //'1'
-1 + ''         //'-1'
NaN + ''        //'NaN'
Infinity + ''   //'Infinity'
-Infinity + ''  //'-Infinity'

//불리언 타입
true + ''       //'true'
false + ''      //'false'

//null 타입
null + ''       //'null'

//undifined 타입
undefined + ''  // 'undifined'

//심벌 타입
(Symbol()) + '' // TypeError : Cannot convert a Symbol value to a string

//객체 타입
({}) + ''       //'[object Object]'
[] + ''         //''
```
### 숫자 타입으로 변환
```js
1 - '1'        //0
1 * '10'       //10
1 / 'one'      //NaN
'1' > 0        //true
```
- 산술 연산자의 피연산자 중에서 숫자 타입이 아닌피연산자를 숫자 타입으로 암묵적 타입 변환한다.
```js
//문자열 타입
//+단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.
+ ''           //0
+ '0'          //0
+ '1'          //1
+ 'string'     //NaN

//불리언 타입
+ true         //1
+ false        //0

//null 타입
+ null         //0

//undefined 타입
+ undefined    //NaN

//심벌 타입
+ Symbol()     // TypeError : Cannot convert a Symbol value to a number

//객체 타입
+ {}            //NaN
+ []            //0
+ [10,20]       //NaN
+ (function(){})//NaN
```
> ☝🏻 빈 문자열(''), 빈 배열([]), null, false는 0으로, true는 1로 변환된다.   
객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다는 것에 주의하자.

### 불리언 타입으로 변환
- 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다.
```js
if ('') console.log('1');
if (true) console.log('2');
if (0) console.log('3');
if ('str') console.log('4');
if (null) console.log('5');

//2 4
```
- 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy값(참으로 평가되는 값) 또는 Falsy값(거짓으로 평가되는 값)으로 구분한다.
> ☝🏻 Falsy값   
false / undefined / null / 0,-0 / NaN / ''(빈 문자열)   
이 외의 값은 모두 Truthy값이다.

## 명시적 타입 변환
### 문자열 타입으로 변환
1. String 생성자 함수를 new 연산자 없이 호출하는 방법
```js
//숫자 > 문자
String(1);         //'1'
String(NaN);       //'NaN'
String(Infinity);  //'Infinity'
//불리언 > 문자
String(true);      //'true'
String(false);     //'false'
```
2. Object.prototype.toString 메서드를 사용하는 방법
```js
//숫자 > 문자
(1).toString();        //'1'
(NaN).toString();      //'NaN'
(Infinity).toString(); //'Infinity'
//불리언 > 문자
(true).toString();      //'true'
(false).toString();     //'false'
```
3. 문자열 연결 연산자를 이용하는 방법
```js
//숫자 > 문자
1 + '';         //'1'
NaN + '';       //'NaN'
Infinity + '';  //'Infinity'
//불리언 > 문자
true + '';      //'true'
false + '';     //'false'
```
### 숫자 타입으로 변환
1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
```js
//문자 > 숫자
Number('0');       //0
Number('-1');      //-1
Number('10.53');   //10.53
//불리언 > 숫자
Number(true);       //1
Number(false);      //0
```
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자로 변환 가능)
```js
//문자 > 숫자
parseInt('0');       //0
parseInt('-1');      //-1
parseInt('10.53');   //10.53
```
3. + 단항 산술 연산자를 이용하는 방법
```js
//문자 > 숫자
+'0';         //0
+'-1';        //-1
+'10.53';     //10.53
//불리언 > 숫자
+true;        //1
+false;       //0
```
4. * 산술 연산자를 이용하는 방법
```js
//문자열 > 숫자
'0' * 1;      //0
'-1' * 1;     //-1
'10.53'* 1;   //10.53
//불리언 > 숫자
true * 1;     //1
false * 1;    //0
```
### 불리언 타입으로 변환
1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
```js
//문자 > 불리언
Boolean('x');        //true
Boolean('');         //false
Boolean('false');    //true
//숫자 > 불리언   
Boolean(0);          //false
Boolean(1);          //true
Boolean(NaN);        //false
Boolean(Infinity);   //true
//null > 불리언
Boolean(null);       //false
//undefined > 불리언
Boolean(undefined);  //false
//객체 > 불리언
Boolean({});         //true
Boolean([]);         //true
```
2. 부정 논리 연산자를 두 번 사용하는 방법
```js
//문자 > 불리언
!!'x';        //true
!!'';         //false
!!'false';    //true
//숫자 > 불리언   
!!0;          //false
!!1;          //true
!!NaN;        //false
!!Infinity;   //true
//null > 불리언
!!null;       //false
//undefined > 불리언
!!undefined;  //false
//객체 > 불리언
!!{};         //true
!![];         //true
```

# 단축 평가
## &&(논리곱)
- 논리곱 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true로 반환한다.
- 때문에, 두 번째 피연산자까지 평가해 보아야 위 표현식을 평가할 수 있고 두번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정한다.
## ||(논리합)
- 논리합 연산자는 두 개 중 하나만 true로 평가되어도 true를 반환한다.
- 때문에, 두 번째 피연산자까지 평가하지 않아도 첫 번째 피연산자가 true일 경우 논리 연산의 결과를 결정한 것은 첫 번째 피연산자이다.
## 단축평가
- 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하며 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는것

### 논리곱 단축 평가를 사용한 if문 대체
```js
var done = true;
var message = '';

//주어진 조건이 true일 때
if (done) message = '완료';

//단축 평가로 대체
message = done && '완료';
console.log(message); //완료
```
### 논리합 단축 평가를 사용한 if문 대체
```js
var done = false;
var message = '';

//주어진 조건이 false 때
if (!done) message = '미완료';

//단축 평가로 대체
message = done || '미완료';
console.log(message); //미완료
```
## 옵셔널 체이닝 연산자
- ES11에 도입되었으며 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
```js
var str = '';
var length = str?.length;
console.log(length); //0

var elem = null;
var value = elem?.value;
console.log(value); //undefined
```
## null 병합 연산자
- ES11에 도입되었으며 null 병합 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고 그렇지 않으면 좌항의 피연산자를 반환한다.
```js
var foo = null ?? 'default string';
console.log(foo); //'default string'

var Foo = '' ?? 'default string';
console.log(Foo); //''
```