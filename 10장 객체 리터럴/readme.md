## 객체 리터럴
### 객체란?
- 자바스크립트는 **객체 기반**의 언어.
- 원시 값을 제외한 나머지는 모두 객체다.
- **프로퍼티**와 **메서드**로 구성된 집합체.
  - 프로퍼티: 객체의 상태를 나타내는 값. 키(key)와 값(value)로 구성.
  - 메서드: 프로퍼티를 참조하고 조작할 수 있는 동작(behavior)

<br>

### 객체 리터럴에 의한 객체 생성
- 가장 일반적이고 간단한 객체 생성 방법.
- 중괄호(`{...}`) 내에 0개 이상의 프로퍼티 정의
- 닫는 중괄호 뒤에 세미콜론(`;`)을 붙임.
```js
var person = {
  name: 'sohyeon',
  sayHello: function () {
    console.log(`Hello! I'm ${thisname}.`);
  }
};

console.log(typeof person); // object
console.log(person); // {name: 'sohyeon', sayHello: f}

// 중괄호 안에 프로퍼티를 정의하지 않으면 빈 객체가 생성
var empty = {};
console.log(typeof empty); // object
```
<br>

### 프로퍼티
```js
var person = {
  // 프로퍼티 키: name, 프로퍼티 값: 'sohyeon'
  name: 'sohyeon',
  age: 26
};
```
- 프로퍼티 키: 프로퍼티 값에 접근할 수 있는 이름. 빈 문자열을 포함한 모든 문자열 또는 심벌 값을 사용할 수 있음.
- 프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값
- 프로퍼티를 나열할 떄는 쉼표(`,`)로 구분.
- 일반적으로 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않지만 사용해도 좋다.
- 네이밍 규칙을 준수하는 프로퍼티 키를 사용할 것을 권장.
```js
var person = {
  firstName = 'sohyeon', // 네이밍 규칙을 준수

  // 네이밍 규칙을 준수 하지 않음.
  // 따옴표가 없는 경우 '-' 연산자가 있는 표현식으로 해석됨.
  //last-name: 'An' --> SyntaxError: unexpected token - 
  'last-name': 'An'
};

console.log(person); // {firstName: 'sohyeon', last-name: 'An'}

var foo = {
  '' : '' // 빈 문자열도 프로퍼티 키로 사용 가능.
};
console.log(foo); // {'': ''} 

var foo2 = {
  0: 1, // 프로퍼티 키로 숫자 리터럴을 사용하면 내부적으로 문자열로 변환된다.
  1: 2
};
console.log(foo2); // {0: 1, 1: 2}
```
- 문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적 생성 가능.
- 프로퍼티 키로 사용할 표현식을 대괄호(`[...]`)으로 묶어야 함.
```js
var obj = {};
var key = 'hello';

obj[key] = 'world';
console.log(obj); // {hello: 'world'}

```
- 예약어를 프로퍼티 키로 사용할 수 있으나 권장하지 않음.
```js
var foo = {
  var: '',
  function: ''
};
console.log(foo); // {var: '', function: ''}
```
- 프로퍼티 키를 **중복 선언**하면 나중에 선언한 프로퍼티로 덮어써짐. (오류 X)
```js
var foo = {
  name: 'Lee',
  name: 'An'
};
console.log(foo); // {name: 'An'}
```

<br>

### 메서드
- 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 **메서드**라 부름.
```js
var circle = {
  radius: 5,
  getDiameter: function () { // 메서드
    return 2 * this.radius;
  }
};
console.log(circle.getDiameter()); // 10
```
<br>

### 프로퍼티 접근
- 마침표 프로퍼티 접근 연산자(`.`)를 사용
- 대괄호 프로퍼티 접근 연산자(`[]`)을 사용
  - 프로퍼티 키는 반드시 **따옴표로 감싼 문자열**이어야 함.
```js
var person = {
  name: 'An',
  1: 10
};

console.log(person.name); // 'An'
consoel.log(person['name']); // 'An'
console.log(person[name]); // ReferenceError: name is not defined
console.log(person[age]); // undefined, 존재하지 않는 프로퍼티에 접근하면 undefined를 반환

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략 할 수 있다.
console.log(person.1); // SyntaxError
console.log(person.'1'); // SyntaxError
console.log(person[1]); // 10 
console.log(person['1']); // 10
```
<br>

### 프로퍼티 값 갱신
- 이미 존재하는 프로퍼티에 값을 할당하면 값이 갱신됨.
```js
var person = {
  name: 'Lee'
};
person.name = 'An';
console.log(person); // {name: 'An'}
```
<br>

### 프로퍼티 동적 생성
- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 새로 생성되어 추가된다.
```js
var person = {
  name: 'Lee'
};

person.age = 26;
console.log(person); // {name: 'An', age: 26}
```
<br>

### 프로퍼티 삭제
- `delete` 연산자로 프로퍼티를 삭제.
- 이 때 `delete` 연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 함.
```js
var person = {
  name: 'Lee'
};

person.age = 26;

delete person.age;
delete person.address; // address 프로퍼티가 없으므로 무시됨. (에러 X)

console.log(person); // {name: 'Lee'}
```
<br>

### ES6에서 추가된 객체 리터럴의 확장 기능
#### 프로퍼티 축약 표현
```js
var x = 1, y = 2;
var obj = {
  x: x,
  y: y
};
console.log(obj); // {x: 1, y: 2}
```
- 프로퍼티 값으로 변수를 사용하는 경우, 변수 이름과 프로퍼티 키가 동일한 이름이면 프로퍼티 키를 생략 가능.
```js
var x = 1, y = 2;
var obj = { x, y };
console.log(obj); // {x: 1, y: 2}
```
#### 계산된 프로퍼티 이름
- 문자열 또는 문자열로 타입 변환할 수 있는 값을 사용해 프로퍼티 키 동적 생성 가능.
- 대괄호(`[...]`)로 묶어야 함.
```js
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

/*
// 위와 동일한 역할
var obj = {
  [`${prefix}-${++i}`] : i,
  [`${prefix}-${++i}`] : i, 
  [`${prefix}-${++i}`] : i 
};
*/
console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```
#### 메서드 축약 표현
```js
var obj = {
  name: 'An',
  sayHi : function (){
    console.log('Hi!' + this.name);
  }

  /*
  // 축약 표현
  sayHi() {
    console.log('Hi!' + this.name);
  }
  */
};
obj.sayHi(); // Hi! An
```
