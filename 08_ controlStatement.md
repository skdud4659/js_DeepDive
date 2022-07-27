# 제어문
- 코드 블록을 실행(조건문)하거나 반복 실행(반복문) 할 때 사용하며 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

## 조건문
### if...else문
- 조건식의 평가가 true일 경우 if 문의 코드 블록이 실행되며 false일 경우 else문의 코드 블록이 실행된다.
- 조건식을 추가하여 조건에 따라 실행될 코드를 늘리고 싶으면 else if문을 사용한다.
- else if문과 else문은 옵션이다. 즉, 사용할 수도 있고 사용하지 않을 수도 있다.
- if문과 else문은 2번 이상 사용할 수 없지만 else if문은 여러 번 사용할 수 있다.
- 코드 블록 내의 문의 하나뿐이라면 중괄호를 생략할 수 있다.
```js
var num = 2;
var kind;

if (num > 0)      kind = '양수';
else if (num < 0) kind = '음수';
else              kind = '양';
```
#### 삼항 조건 연산자
- 조건식 ? 코드 블록 1 : 코드 블록 2
```js
var kind = num ? (num > 0 ? '양수' : '음수') : '양';
```
> ☝🏻 연산자에서도 정리했듯 조건에 따라 단순히 값을 결정하여 변수에 할당하는 경우 삼항 조건 연산자를 사용하는 편이 가독성이 좋다.

### switch문
- 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.
- 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동하며 이 부분은 선택 사항이다.
- 참, 거짓보다는 다양한 상황에 따라 실행할 코드 블록을 결정할 때 사용한다.
> ☝🏻 break  
: 코드 블록에서 탈출하는 역할을 하며 break 문이 없다면 case문의 표현식과 일치하지 않더라도 다음 case문으로 연이어 이동한다.
-> default문에서는 생략하는 것이 일반적이다.
```js
var month = 11;
var monthName;

switch(month) {
    case 1: monthName = 'January';
    	break;
    case 2: monthName = 'February';
    	break;
    case 3: monthName = 'March';
    	break;
    case 4: monthName = 'April';
    	break;
    case 5: monthName = 'May';
    	break;
    case 6: monthName = 'Jun';
    	break;
    case 7: monthName = 'July';
    	break;
    case 8: monthName = 'August';
    	break;
    case 9: monthName = 'September';
    	break;
    case 10: monthName = 'October';
    	break;
    case 11: monthName = 'November';
    	break;
    case 12: monthName = 'December';
    	break;
    default: monthName = 'Invalid month';
}

console.log(monthName); //November

//폴스루 (여러 개의 case문을 하나의 조건으로 사용) - 위 와 다른 예시 코드임!
switch(month) {
    case1: case3: case5: case7: case8: case10:
    	days = 31;
    	break;
  default:
    console.log('Invalid month');
```

## 반복문
- 조건식의 평가 결과가 참인 경우 코드 블록을 실행하며 그 후 조건식을 재평가하여 여전히 참인 경우 코드 블록을 다시 실행하여 거짓일 때까지 반복된다.

### for문
- 가장 일반적으로 사용한다.
```js
for (var i=0; i<2; i++) {
	console.log(i);
}
//i는 0부터 2미만의 수를 하나씩 증가시키며 console.log에 실행된다.

//중첩 for문 - 알고리즘에서 값을 비교할 때 많이 쓰임
for (var i=1; i<=6; i++) {
  for(var j=1; j<=6; j++) {
    if(i+j === 6) console.log(`[${i}, ${j}]`);
  }
}
```
### while문
- 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.
- 조건식의 평과가 언제나 참이면 무한루프가 되며 이를 탈출하기 위해서는 코드 블록 내에 if문으로 탈출 조건을 만들고 break로 탈출한다.
```js
var count = 0;

//무한루프
while(true) {
  console.log(count);
  count++;
  //count가 3이면 코드 블록을 탈출한다.
  if(count === 3) break;
} //0 1 2
```
> ☝🏻 for문은 반복 횟수가 명확할 때 주로 사용하고 while문은 반복 횟수가 불명확할 때 주로 사용한다.

### do...while문
- 코드 블록을 먼저 실행하고 조건식을 평가한다.
```js
var count = 0;
//count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count); //0 1 2
  count++;
} while (count < 3);  
```
### break문
- 코드 블록을 탈출하기 위해 사용한다.
- 중첩된 for문의 내부 for문에서 break문을 실행하면 내부 for문을 탈출하여 외부 for문으로 진입하며 이 때 내부 for문이 아닌 외부 for문을 탈출하려면 레이블 문을 사용한다.
```js
//outer라는 식별자가 붙은 레이블 for문
outer: for (var i=0; i>3; i++) {
  for (var j=0; j<3; j++) {
    //i+j===3이면 outer라는 식별자가 붙은 레이블 for문을 탈출한다.
    if(i + j === 3) break outer;
    console.log(`inner [${i}, ${j}]`);
  }
}

console.log('done!');
```
> ☝🏻 레이블 문을 사용하면 프로그램의 흐름이 복잡해져서 가독성이 나빠지고 오류를 발생시킬 가능성이 높아져 for문 외부로 탈출할 때 이외에는 잘 사용하지 않는다.

### continue문
- 현 시점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시키며 break처럼 탈출하지는 않는다.