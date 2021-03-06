## 전역 변수의 문제점
### 변수의 생명 주기
- 변수는 생성되고 소멸되는 생명 주기가 있다.
  - 변수에 생명주기가 없다면 한번 선언된 변수는 프로그램을 종료하지 않는 한 영원히 메모리 공간을 점유한다.
- 변수는 자신이 등록된 스코프가 소멸될 때까지 유효하다.
  - 누군가 스코프를 참조하고 있으면 참조된 스코프는 소멸하지 않는다.

#### 지역 변수의 생명 주기
- 지역 변수의 생명 주기는 **함수의 생명 주기**와 일치한다.
- 함수 내부에서 선언한 변수는 함수가 호출된 직후에 함수의 코드가 실행되기 이전에 자바스크립트 엔진에 의해 먼저 실행된다.
- 함수 내부에서 선언된 지역변수는 함수가 호출되어 실행되는 동안에만 유효하다.
- 호이스팅은 스코프를 단위로 동작한다.
```js
function foo() {
  var x = 'local';
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x가 정의되지 않음.
```
```js
var x = 'global';

function foo() {
  console.log(x); // undefined
  var x = 'local';
}

foo();
console.log(x); // global
```
- `foo`함수 내부에서 선언된 지역 변수 `x`는 함수가 호출된 직후에 생성되어 `undefined`로 초기화 된다.

#### 전역 변수의 생명 주기
- 특별한 진입점이 없고 코드가 로드되자마자 곧바로 해석되고 실행된다.
- `var` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 된다.
  - `var` 키워드로 선언한 전역 변수의 생명주기는 전역 객체의 생명 주기와 같다.

##### 전역객체?
전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체다. 브라우저에서는 window, 서버사이드환경에서는 global객체를 의미한다. 표준 빌트인 객체(Object, String, Number, Function, Array ...)를 프로퍼티로 가지고 있다.

<br>

### 전역 변수의 문제점
- 모든 코드가 전역 변수를 참조하고 변경할 수 있는 **암묵적 결합**을 허용한다.
- 생명 주기가 길기 떄문에 메모리 리소스를 오랜 기간 소비한다.
- 스코프 체인 상에서 종점에 존재하기 때문에 검색 속도가 가장 느리다.
- 다른 파일 내에서 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다.

<br>

### 전역 변수의 사용을 억제하는 방법
#### 즉시 실행 함수
- 모든 코드를 즉시 실행 함수로감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.
```js
(functioin (){
    var foo = 10; 
    // ...
}());

console.log(foo); // ReferenceError: foo is not defined.
```

#### 네임스페이스 객체
- 전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가한다.
- 이 방법은 식별자 충돌을 방지하는 효과는 있으나 네임스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용하지는 않다.
```js
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = 'Lee';

console.log(MYAPP.name); // Lee
```

#### 모듈 패턴
- 클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다.
- 전역 변수의 억제는 물론 캡슐화까지 구현할 수 있다.
##### 캡슐화??
객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다. 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 한다. (=정보 은닉)

```js
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    }
  };
}());

console.log(Counter.num); // undefined
console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```
- 반환되는 객체의 프로퍼티에는 외부에 노출하고 싶은 변수나 함수를 담아 반환한다.
- 반환하는 객체에 추가하지 않으면 외부에서 접근할 수 없다.

<br>

#### ES6 모듈
- ES6 모듈은 **파일 자체의 독자적인 모듈 스코프**를 제공하기 때문에 전역 변수를 사용할 수 없다.
- `script` 태그에 `type="module"` 어트리뷰트를 추가하면 로드된 파일은 모듈로서 동작한다.
  - 모듈의 확장자는 `mjs`를 권장한다.
- 구형 브라우저에서는 동작하지 않고 트랜스파일링이나 번들링이 필요하기 때문에 아직까지는 ES6 모듈보다는 Webpack 등의 모듈 번들러를 사용하는 것이 일반적이다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```