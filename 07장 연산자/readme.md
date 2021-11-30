## 연산자

- 연산자는 하나 이상의 표현식을 산술, 할당, 비교, 논리, 타입 , 지수 연산 등을 수행해 하나의 값을 만든다.
- 연산의 대상을 피연산자(operand)라 한다.

### 산술 연산자
- 피연산자를 대상으로 수학적 계산을 수행, 새로운 **숫자** 값을 만든다.
- 산술 연산이 불가능한 경우 `NaN`을 반환. 
- 이항 산술 연산자
  - 피연산자가 2개.
  - 피연산자의 값을 변경할 수는 없다.
  - `+, -, *, /, %`
- 단항 산술 연산자
  - 피연산자가 1개.
  - `++, --, +, -`
  - `++, --`는 피연산자의 값을 변경하며 위치에 의미가 있다.
  - `+`는 어떠한 효과도 없다. 
  - `-`는 양수를 음수로, 음수를 양수로 반전시킨다.
  - `+, -`를 숫자 타입이 아닌 타입에 사용하면 숫자 타입으로 변환해 계산한 값을 반환한다. (실제 피연산자 변경X)
```javascript
var x = 5, result;

result = x++; // 선할당 후증가
console.log(result, x) // 5, 6

result = ++x; // 선증가 후할당
console.log(result, x); // 7, 7
```
 ```javascript
var x = '1';
console.log(+x); // 1, 문자열을 숫자로 
console.log(-x); // -1
console.log(x); // "1", 피연산자 변경 X

x = true;
console.log(+x); // 1
console.log(-x); // -1
x = false;
console.log(+x); // 0
console.log(-x); // 0
x = 'Hello';
console.log(+x); // NaN, 문자열을 숫자로 변환할 수 없다.
 ```

- 문자열 연결 연산자
  - `+` 연산자는 피연산자 중 하나 이상이 **문자열**인 경우 **문자열 연결 연산자**로 동작한다.
```javascript
'1' + 2; // '12', 문자열 연결 연산
```

- 암묵적 타입 변환 / 타입 강제 변환
```javascript
1 + true; // 2, true를 1로 강제 변환
1 + false; // 1, false를 0으로 강제 변환
1 + null; // 1, null을 0으로 강제 변환
+undefined; // NaN, undefined는 숫자로 변환되지 않는다.
1 + undefined; // NaN
```

### 할당 연산자
- 우항에 있는 피연산자의 평가 결과를 좌항 변수에 할당한다.
```javascript
var x;
x = 10;
console.log(x); // 10
x += 5;
console.log(x); // 15

var str = 'Hello';
str += 'World';
console.log(str); // 'Hello World'
```
- 할당문은 **표현식인 문**이므로 다른 변수에 할당할 수 있다. (연쇄 할당 가능)
```javascript
var a, b, c;
a = b = c = 0;
console.log(a, b, c); // 0 0 0
```

### 비교 연산자
- 좌항과 우항의 피연산자를 비교하고 결과를 불리언(boolean) 값으로 반환한다.
- 동등/일치 비교 연산자

비교 연산자 | 의미 | 예시 | 설명
:--:|:--:|:--:|--
== | 동등 비교 | x == y | x와 y의 **값**이 같음.
=== | 일치 비교 | x === y | x와 y의 **타입과 값**이 같음.
!= | 부동등 비교 | x != y | x와 y의 **값**이 다름.
!== | 불일치 비교 | x !== y | x와 y의 **타입과 값**이 다름.

- **동등 비교** 연산자는 좌항과 우항의 피연산자 타입이 다르더라도 타입을 일치시킨 후에 비교한다. (암묵적 타입 변환)
```javascript
5 == 5; // true
5 == '5' // true, 암묵적 타입 변환
```
- 안티 패턴이 존재하기 때문에 **일치 비교(===) 연산자를 사용하는 것이 좋다.**
```javascript
// 안티 패턴
'0' == ''; // false
0 == ''; // false
0 == '0'; //true
false == 'false'; // false
false == '0'; // true
false == null; // false
false == undefined; // false
```
- `NaN`는 자신과 일치하지 않는 유일한 값이다. 숫자가 NaN인지 조사하려면 `isNaN` 함수를 사용해야 한다.
```javascript
NaN === NaN; // false
isNaN(NaN); // true
isNaN(10); // false
isNaN(undefined + 1); // true
```
- 양의 0과 음의 0을 비교하면 `true` 이다. 정확한 비교를 위해 `Object.is` 메서드를 써야한다.
```javascript
0 === -0; // true
0 == -0; // true
-0 === +0; // true
Object.is(-0, +0); // false
Object.is(NaN, NaN); // true
```

### 삼항 조건 연산자
- 조건식 ? 조건식이 `true` 일 때 반환 값 : 조건식이 `false` 일 때 반환 값
- 첫번째 피연산자가 `true`로 평가되면 두번째 피연산자 반환.
- 첫번째 피연산자가 `false`로 평가되면 세번째 피연산자 반환.
- 만약 조건식의 결과가 boolean 값이 아니면 boolean 값으로 **암묵적 타입 변환**이 일어난다.
- if...else 문과 달리 값처럼 사용할 수 있다.
```javascript
var x = 2;
// 2 % 2 = 0, 0은 false로 암묵적 타입 변환된다.
var result = x % 2 ? '홀수' : '짝수';
console.log(result); // '짝수'

result = if(x % 2){result = '홀수';} else {result = '짝수';} // SyntaxError
```
- 조건에 따라 수행해야 할 문이 하나일 경우 사용하면 좋다.

<br>

### 논리 연산자
논리 연산자 | 의미
:--:|--
\|\| | 논리합(OR)
&& | 논리곱(AND)
! | 부정(NOT)

- `&&` 연산에서는 하나라도 `false`가 존재하면 결과가 `false`이다.
- `||` 연산에서는 하나라도 `true`가 존재하면 결과가 `true`이다.
```javascript
true && false; // false
false && false; // false

true || false; // true
false || false; // false

!true; // false

//암묵적 타입 변환
!0; // true
!'hello'; // false
```

### typeof 연산자
- 피연산자의 데이터 타입을 문자열로 반환한다.
- 실제 데이터 타입과 정확히 일치하지는 않는다.
```javascript
typeof ''; // 'string'
typeof 1; // 'number'
typeof NaN; // 'number'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Symbol(); // 'symbol'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
typeof new Date(); // 'object'
typeof /test/gi; // 'object'
typeof function () {}; // 'function'
```
< 주 의 >
- `null`의 `typeof` 값이 `null`이 아닌 `object`로 반환되기 때문에 값이 `null`인지 확인할 때는 **일치 연산자(===)** 르 사용한다.
- 선언하지 않은 식별자를 `typeof`로 연산하면 `ReferenceError`가 아니라 `undefined`를 반환한다.


### 지수 연산자
- ES7에서 도입됐다.
```javascript
2 ** 2; // 2^2 = 4
2 ** 2.5; // 5.65695...
2 ** 0; // 1
2 ** = 2; // 4

// ES7 이전에는 Math.pow를 사용했다.
Math.pow(2, 2);
Math.pow(2, 2.5);

// 음수 거듭제곱의 밑으로 사용하려면 괄호로 묶어야 한다.
(-5) ** 2; // 25
-5 ** 2; /// SyntaxError: ...must be used to disambiguate operator precedence (괄호가 필요하다)
```

### 연산자 우선순위
- 우선순위를 외우기 보다 괄호를 사용하여 우선순위를 명시적으로 조절하는 것이 좋다.

우선순위 | 연산자
:--:|--
1 | ()
2 | new(매개변수 존재), ., [](프로퍼티 접곤), ()(함수호출), ?.(옵셔널 체이닝 연산자)
3 | new(매개변수 미존재)
4 | x++, x--
5 | !x, +x, -x, ++x, --x, typeof, delete
6 | **
7 | *, /, %
8 | +, -
9 | <, <=, >, >=, in, instanceof
10 | ==, !=, ===, !==
11 | ??(null 병합 연산자)
12 | &&
13 | 
14 | ? ... : ...
15 | 할당 연산자(=, +=, -=, ...)
16 | ,
