# 17장 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

`new` 연산자와 `Object` 생성자 함수를 호출하면 빈 객체를 생성해 반환한다.

객체 생성 후 프로퍼티를 추가해주면 된다.

```js
const person = new Object();
person.name = "Jeon";
```

생성자 함수는 `new` 연산자와 함께 호출하여 객체, 즉 인스턴스를 생성하는 함수를 말한다.

- `String`
- `Number`
- `Boolean`
- `Function`
- `Array`
- `Date`
- `RegExp`
- `Promise`

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String("Lee");
console.log(typeof strObj); // object
console.log(strObj); // String {"Lee"}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(numObj); // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj); // Boolean {true}

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function("x", "return x * x");
console.log(typeof func); // function
console.dir(func); // ƒ anonymous(x)

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr); // object
console.log(arr); // [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp); // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date); // Mon May 04 2020 08:36:33 GMT+0900 (대한민국 표준시)
```

## 17.2 생성자 함수

객체 리터럴은 동일한 프로퍼티를 갖는 객체 여러 개를 생성할 수는 없다.

생성자 함수를 사용하면 클래스의 인스턴스를 생성하듯 동일한 프로퍼티를 갖는 객체 여러개를 생성할 수 있다.

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

> `this`: 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. `this`가 가리키는 값(`this` 바인딩)은 함수 호출 방식에 따라 동적으로 결정된다.

자바스크립트에서는 자바와 같이 인스턴스를 생성하는 형식이 정해져 있는게 아니라 그냥 `new` 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. `new`가 없으면 일반 함수로 동작한다.

```js
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

자바스크립트에서는 `new` 연산자와 함께 생성자 함수를 호출하면 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환한다.

1. 인스턴스 생성과 this 바인딩

   암묵적으로 빈 객체를 생성한 뒤 이 객체(인스턴스)를 this에 바인딩한다. 이러한 프로세스는 런타임 이전에 실행된다.

2. 인스턴스 초기화

   함수 몸체 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

3. 인스턴스 반환

   모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.

   ⚠️명시적으로 반환하면 암묵적인 반환이 무시되므로 유의해야 한다.

```js
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

함수는 `new`와 함께 호출하면 생성자 함수로서 동작한다.

함수 객체는 일반 객체와는 다르다. 함수로서 동작하기 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]]` 같은 내부 메서드를 추가로 갖는다.

**일반 함수로 호출되면 `[[Call]]`, 생성자 함수로 호출되면 `[[Construct]]`가 호출된다.**

함수는 Call 메서드는 필수로 가지지만, Construct 메서드는 필수로 갖지 않는다.

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 **함수 정의 방식에 따라 함수를 두 가지로 구분**한다.

- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

```js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function () {},
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo(); // -> foo {}
new bar(); // -> bar {}
new baz.x(); // -> x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만을 메서드로 인정한다.
const obj = {
  x() {},
};

new obj.x(); // TypeError: obj.x is not a constructor
```

자바스크립트의 메서드는 일반적인 메서드의 정의와 좀 다른데, 이는 ES 사양에서 메서드는 ES6의 메서드 축약 표현만을 의미하기 때문이다.

그냥 일반 함수에 `new`를 붙여서 호출하면 생성자 함수로 동작하는 특성 때문에, 생성자 함수가 `new` 없이 호출되는 것을 방지하는 실수를 저지를 수도 있다.

ES6에서는 `new.target`을 지원한다. `new.target`은 `this`와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.

`new` 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킨다. `new` 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 `undefined`다.

```js
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

`new.target`을 지원하지 않는 IE에서는 아래와 같은 스코프 세이프 생성자 패턴을 사용해야 한다.

```js
// Scope-Safe Constructor Pattern
function Circle(radius) {
  // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고
  // this에 바인딩한다. 이때 this와 Circle은 프로토타입에 의해 연결된다.

  // 이 함수가 new 연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 window를 가리킨다.
  // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
  if (!(this instanceof Circle)) {
    // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

다행히 대부분의 빌트인 생성자 함수는 `new` 연산자와 함께 호출되었는지 확인 후 적절한 값을 반환한다.

`Object`와 `Function` 생성자 함수는 호출 시 `new`의 여부와 관계없이 동일하게 동작한다.

```js
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function("x", "return x ** x");
console.log(f); // ƒ anonymous(x) { return x ** x }

f = Function("x", "return x ** x");
console.log(f); // ƒ anonymous(x) { return x ** x }
```

하지만 `String`, `Number`, `Boolean` 생성자 함수는 `new`와 함께 호출하면 래퍼 객체를 생성하여 반환하지만 `new` 없이 호출하면 문자열, 숫자, 불리언 원시 값을 반환한다.

```js
const strObj = new String(123);
console.log(strObj, typeof strObj); // [String: '123'] object

const numObj = new Number("123");
console.log(numObj, typeof numObj); // [Number: 123] object

const boolObj = new Boolean("true");
console.log(boolObj, typeof boolObj); // [Boolean: true] object

const str = String(123); // 숫자 123을 문자열 "123"으로 변환
console.log(str, typeof str); // 123 string 출력

const num = Number("123"); // 문자열 "123"을 숫자 123으로 변환
console.log(num, typeof num); // 123 number 출력

const bool = Boolean("true"); // 문자열 "true"를 불리언 true로 변환
console.log(bool, typeof bool); // true boolean 출력
```
