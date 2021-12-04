## 스코프
### 스코프란?
- 식별자(변수 이름, 함수 이름, 클래스 이름 등)가 유효한 범위.
- 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙.
- 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있게 한다.
```js
var x = 'global';// 코드의 가장 바깥 영역에 선언된 변수는 어디서든 참조할 수 있다.

function foo() {
  var x = 'local'; // 함수 내부에서 선언된 변수는 함수 내부에서만 참조할 수 있다.
  console.log(x); // local
}

foo();
console.log(x); // global
```
-  `var` 키워드로 선언된 변수는 **같은 스코프 내에서 중복 선언을 허용한다.**
```js
function foo() {
  var x = 1;
  // 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작.
  var x = 2;
  console.log(x); // 2
}
foo();
```
- `let, const`로 선언된 변수는 **같은 스코프 내에서 중복 선언을 허용하지 않는다.**
```js
function bar() {
  let x = 1;
  let x = 2; // SyntaxError: x가 이미 선언되었음.
}
bar();
```

<br>

### 스코프의 종류
- 코드는 전역(global)과 지역(local)로 구분한다.

구분 | 설명 | 스코프 | 변수
:--:|--|:--:|:--:
전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수
지역 | 함수 몸체 내부 | 지역 스코프 | 지역 변수

- 변수는 자신이 선위된 위치에 의해 스코프가 결정된다.
```js
// 전역 변수
// x, y는 어디서든 참조할 수 있다.
var x = 'global x';
var y = 'global y';

function outer() {
  var z = 'outer\'s local z'

  console.log(x); // global x 
  console.log(y); // global y
  console.log(z); // outer's local z

  function inner() {
    var x = 'inner\'s local x';

    console.log(x); // inner's local x
    console.log(y); // global y
    console.log(z); // outer's local z    
  }

  inner();
}

outer();

console.log(x); // global x
console.log(z); // ReferenceError: z는 정의되지 않았다.
```
#### 전역과 전역 스코프
- 전역 변수는 어디서든 참조할 수 있다.


#### 지역과 지역 스코프
- 자신이 선언된 함수 몸체와 하위 함수 몸체에서만 참조 할 수 있다.
- 위의 예제 코드에서 z는 outer 함수 내부에서 선언된 지역 변수이므로 outer 함수 내부와 하위 함수 inner 함수 내부에서 참조할 수 있다.

<br>

### 스코프 체인
- 함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있다.
- 스코프가 함수의 중첩에 의해 계층적 구조를 갖는다. (=**스코프 체인**)
- 모든 지역 스코프의 최상위 스코프는 전역 스코프다.
- 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 **변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색** 한다.

#### 스코프 체인에 의한 변수 검색
- 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다.
```js
var x = 'global x';
var y = 'global y';

function outer() {
  var z = 'outer\'s local z'

  function inner() {
    var x = 'inner\'s local x';

    console.log(x); 
    // x 변수를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서
    // x 변수가 선언되었는지 검색하고 선언된 변수가 존재하면 검색을 종료한다.
    console.log(y); 
    // y 변수를 참조하는 inner 함수의 지역 스코프에 y 변수가 선언되지 않았으므로 
    // 상위 스코프인 outer 함수의 지역 스코프로 이동한다.
    // outer 함수의 지역 스코프에도 y 변수가 선언되지 않았으므로
    // 전역 스코프로 이동한다. 전역 스코프에 y 변수가 존재한다.
    console.log(z); // outer's local z    
  }

  inner();
}

outer();
```
#### 스코프 체인에 의한 함수 검색
- 함수도 식별자에 해당하기 때문에 스코프를 갖는다.
- 일반 변수의 스코프와 같이 동작한다.
```js
function foo() {
  console.log('global function foo');
}

function bar() {
  function foo() {
    console.log('local function foo');
  }

  foo(); // local function foo
}
bar();
```
<br>

### 함수 레벨 스코프
- 지역 스코프는 **코드 블록이 아닌 함수**에 의해서만 생성된다.
- **블록 레벨 스코프**: 함수 몸체만이 아니라 모든 코드 블록(if, for, while, tyr\catch 등)을 스코프로 인정함.
- **함수 레벨 스코프**: 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정함.
- `var` 키워드로 선언된 변수는 **함수 레벨 스코프**를 가지고 `let, const`로 선언된 변수는 **블록 레벨 스코프**를 가진다. (자세한 내용은 15장...)
```js
var x = 1;

if (true) {
  // var 키워드로 선언된 변수는 함수 몸체만을 지역 스코프로 인정한다.
  // 따라서 이미 선언된 전역 변수 x가 있으므로 x변수는 중복선언된 후 재할당된다.
  var x = 10;
}

console.log(x); // 10
```
```js
var i = 10;

// var로 선언한 for문 내의 i는 전역 변수다.
// 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for(var i = 0; i < 5; i++){
  console.log(i); // 0 1 2 3 4 
}

console.log(i); // 5
```
<br>

### 렉시컬 스코프
```js
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1, 
// bar는 전역에서 정의된 함수이므로 전역 스코프를 상위 스코프로 결정한다.
```
- 자바스크립트는 **렉시컬 스코프**를 따른다.
- **동적스코프**
  - 함수를 **어디서 호출**했는지에 따라 상위 스코프를 결정한다.
  - 함수가 호출되는 시점에 동적으로 상위 스코프를 결정한다.
- **렉시컬 스코프(정적 스코프)**
  - 함수를 **어디서 정의**했는지에 따라 상위 스코프를 결정한다.
  - 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.
  - 함수 정의가 실행되어 생성된 함수 객체는 상위 스코프를 **기억**한다.
  - 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않는다.
