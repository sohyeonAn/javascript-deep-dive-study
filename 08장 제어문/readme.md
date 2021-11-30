## 제어문

### 블록문
- 0게 이상의 문을 `중괄호{}`로 묶은 것.
- 코드 블록이라고 불림.
- 하나의 실행 단위.
```javascript
// 블록문
{
  var foo = 10;
}

// 제어문
var x= 11;
if(x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

<br>

### 조건문
- 주어진 조건식의 결과에 따라 블로문의 실행을 결정한다.
- `boolean` 값으로 평가되는 표현식이다.
- 조건식이 `boolean` 값으로 평가되지 않으면 `boolean`으로 강제 변환한다. (**암묵적 타입 변환**)
#### if...else문
- `if`와 `else`는 한 번만 사용할 수 있지만 `else if`는 여러 번 사용할 수 있다.
- 만약 코드 블록 내의 문이 하나뿐이라면 중괄호를 생략할 수 있다.
```javascript
if (조건식1) {
  // 조건식1이 참(true)이면 이 코드 블록이 실행된다.
} else if (조건식2){
  // 조건식2가 참(true)이면 이 코드 블록이 실행된다.
} else {
  //조건식1과 조건식2가 모두 거짓(false)이면 이 코드 블록이 실행된다.
}

// 중괄호 생략
var num = 2;
var kind;
if(num > 0) kind = '양수';
else if(num < 0) kind = '음수';
else kind = '영';
console.log(kind); // 양수

// 삼항 연산자로 바꿔쓰기
// 0은 false로 취급된다.
var kind2 = num ? (num > 0 ? '양수' : '음수') : '영';
console.log(kind2); // 양수
```

#### switch문
- 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 `case` 문으로 실행 흐름을 옮긴다.
- 표현식과 일치하는 `case`문이 없다면 실행 순서는 `defalut` 문으로 이동한다.
- `default` 문은 사용할 수도 있고 사용하지 않을 수도 있다. (옵션)
- `if...else`문은 논리적 참/거짓으로 실행할 코드 블록을 결정하는 반면, `switch`문은 **다양한 상황(case)** 에 따라 실행할 코드 블록을 결정한다.
- `break` 는 코드블록을 탈출하는 역할을 한다. 만약 `break`가 없다면 case문의 표현식과 일치하지 않더라도 실행 흐름이 다음 case문으로 연이어 이동한다. (=폴스루/fall through)
```javascript
switch (표현식) {
  case 표현식1:
    // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    // switch 문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
    // switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
}
```
```javascript
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'Jan';
  case 2: monthName = 'Feb';
  case 3: monthName = 'Mar';
  case 4: monthName = 'Apr';
  case 5: monthName = 'May';
  case 6: monthName = 'Jun';
  case 7: monthName = 'Jul';
  case 8: monthName = 'Aug';
  case 9: monthName = 'Sep';
  case 10: monthName = 'Oct';
  case 11: monthName = 'Nov';
  case 12: monthName = 'Dec';
  default: monthName = 'Invalid month';
}

console.log(monthName); // Invalid month, break가 없기때문.

switch (month) {
  case 1: monthName = 'Jan';
    break;
  case 2: monthName = 'Feb';
    break;
  case 3: monthName = 'Mar';
    break;
  case 4: monthName = 'Apr';
    break;
  case 5: monthName = 'May';
    break;
  case 6: monthName = 'Jun';
    break;
  case 7: monthName = 'Jul';
    break;
  case 8: monthName = 'Aug';
    break;
  case 9: monthName = 'Sep';
    break;
  case 10: monthName = 'Oct';
    break;
  case 11: monthName = 'Nov';
    break;
  case 12: monthName = 'Dec';
    break;
  default: monthName = 'Invalid month';
}
console.log(monthName); // Nov
```
```js
// 폴스루를 이용하여 윤년인지 판단하기
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch (month) {
  case 1: case 3: case 5: case 7: case 8: case 10: case 12:
    days = 31;
    break;
  case 4: case 6: case 9: case 11:
    days = 30;
    break;
  case 2:
    // 윤년 계산 알고리즘
    // 1. 연도가 4로 나누어 떨어지는 해는 윤년이다.
    // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해는 평년이다.
    // 3. 연도가 400으로 나누어떨어지는 해는 윤년이다.
    days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
    break;
  default:
    console.log('Invalid month'); 
}
console.log(days); // 29
```
- 만약 `if...else`문으로 해결할 수 있다면 `switch`문보다 `if...else`를 사용하는 편이 일반적으로 더 좋지만 가독성을 높이기 위해 `switch`를 사용할 수도 있다.

<br>

### 반복문
- 조건식의 평가 결과가 참인 경우 코드 블록 실행한다.
- 그 후에 조건식을 **다시** 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다.
- 조건식이 거짓(false)일 때까지 반복한다.

#### for문
```js
// for문의 형태
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  조건식이 참인 경우 반복 실행될 문 (코드 블록);
}
```
- `for`문의 실행 순서
1. `for`문을 실행하면 먼저 변수 선언문이 실행된다. 변수 선언문은 단 한 번만 실행된다.
2. 변수 선언문의 실행이 종료되면 조건식이 실행된다. 
3. 조건식의 평가 결과가 `true`이면 증감문이 아니라 코드 블록으로 실행 흐름이 이동한다.
4. 코드 블록의 실행이 종료되면 증감식이 실행된다.
5. 증감식이 종료되면 다시 조건식이 실행된다. 
6. 조건식의 평가 결과가 `true`이면 코드 블록이 다시 실행된다.
7. 4-6 을 반복하다가 조건식의 평가 결과가 `false`이면 `for` 문이 종료된다.
```js
// for (변수 선언문; 조건식; 증감식)
for(var i = 0; i < 2; i++){
  console.log(i);
}
```
```plainText
결과: 
0
1
```
- `for`문의 변수 선언문, 조건식, 증감식은 모두 옵션이지만 어떤 식도 사용하지 않으면 **무한 루프**(코드 블록을 무한히 반복 실행)가 된다.
```js
// 무한루프
for( ; ; ){...}
```
- `for`문을 중첩해 사용할 수 있다. (=중첩 `for`문)
```js
// 주사위를 던져 두 눈의 합이 6이 되는 모든 경우의 수
for(var i = 1; i <= 6 ; i++){
  for(var j = 1; j <= 6; j++){
    if(i + j === 6) console.log(`${i}, ${j}`);
  }
}
```
```plainText
결과:
1, 5
2, 4
3, 3
4, 2
5, 1
```
#### while문
- 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 한다.
```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
// while (조건식) { 코드 블록 }
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}
```
- 조건식의 평가 결과가 언제나 참이면 **무한루프**가 된다. 무한루프를 탈출하기 위해서는 코드 블록 내에 **탈출 조건** 을 만들고 `break`문을 사용해야 한다.
```js
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```
#### do...while 문
- `while`문과 다르게 **코드 블록을 우선 실행**한 후에 조건식을 평가한다. 따라서 코드 블록은 무조건 **한 번 이상 실행**된다.
```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 실행한다.
// do { 코드 블록 } while (조건식);
do {
  console.log(count); // 0 1 2
  count++;
} while (count < 3);
```

#### break 문
- 레이블 문, 반복문, switch 문의 코드 블록 외에 `break`문을 사용하면 `SyntaxError`가 발새한다.
- 레이블 문: 식별자가 붙은 문.
  - 레이블 문을 사용하면 내부 for문을 탈출하고 외부 for문을 탈출하는 과정을 거치지 않고 바로 외부 for문을 탈출할 수 있다.
  - 프로그램의 흐름이 복잡해지고 가독성이 나빠져 일반적으로 권장되지 않는다. 
```js
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for문을 탈출한다.
    if (i + j === 3) break outer;
    console.log(`inner ${i}, ${j}`);
  }
}
console.log('Done!');
```

#### continue 문
- 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 흐름을 이동 시킨다.\
- if 문 내에서 실행해야 할 코드가 길다면 들여쓰기가 한 단계 깊어지므로 `continue`를 쓰는 편이 가독성이 좋다. 
```js
var string = 'Hello World';
var search = 'l';
var count = 0;

// continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
for (var i = 0; i < string.length; i++){
  // 'l' 이면 카운트를 증가 시킨다.
  if (string[i] === search) {
    count++;
    ...
  }
}

// continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
for (var i = 0; i < string.length; i++){
  // 'l' 아니면면 카운트를 증가시키지 않는다.
  if (string[i] !== search) continue;
  
  count++;
  ...
} 
```