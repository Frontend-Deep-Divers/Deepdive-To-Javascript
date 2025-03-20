# 18장 함수와 일급 객체

## 18.1 일급 객체

다음과 같은 조건의 객체를 일급 객체라 한다.

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. **함수의 매개변수에 전달할 수 있다.**
4. **함수의 반환값으로 사용할 수 있다.**

자바스크립트의 함수는 일급 객체다.

함수가 일급 객체라는 건 함수를 객체와 동일하게 사용할 수 있다는 의미다.

## 18.2 함수 객체의 프로퍼티

### `arguments` 프로퍼티

`arguments` 객체를 값으로 가진다.

이 객체는 **함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체**이며, 함수 내부에서 지역 변수처럼 사용된다. 즉, 함수 외부에선 참조할 수 없다.

현재 일부 브라우저에만 지원하며 ES3부터 폐지되었다. 함수 내부에서 `arguments` 객체를 참조하도록 한다.

```js
function mul(x, y) {
  console.log(arguments); // 받아온 인수를 출력
  return x * y;
}
```

초과된 인수는 버려지지 않고 이 `arguments` 객체에 저장된다.

💡`arguments` 객체는 매개변수 개수가 정해지지 않은 가변 인자 함수를 구현할 때 유용하다.

```js
function sum() {
  let res = 0;

  // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있다.
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6

// 배열로 변환하여 순회
function sum() {
  // arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3, 4, 5)); // 15

// ES6 Rest parameter를 이용한 순회
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

### `caller` 프로퍼티

`caller`는 ES 사양에 포함되지 않은 비표준 프로퍼티다. 이후 표준화될 예정도 없다.

함수 자신을 호출한 함수를 가리킨다.

```js
function foo(func) {
  return func();
}

function bar() {
  return bar.caller;
}

// 브라우저에서 실행한 결과
console.log(foo(bar)); // caller : function foo(func) { ... }
console.log(bar()); // caller : null
```

### `length` 프로퍼티

함수를 정의할 때 선언한 매개변수의 개수를 가리킨다. `arguments`의 값과 다를 수 있다.

### `name` 프로퍼티

함수 이름을 나타낸다. 이전까지 비표준이다 ES6에서 정식 표준이 되었다.

ES5와 ES6에서 동작이 다르다. 익명 함수 표현식은 ES5에서 빈 문자열을 값으로 갖지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다. 그외에는 동일한 듯.

⚠️함수 이름과 함수 식별자가 다름에 유의하자. 호출한 땐 식별자로 한다.

### `__proto__` 접근자 프로퍼티

모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 갖는다. 프로토타입 객체를 가리킨다. `__proto__` 프로퍼티는 프로토타입 객체에 접근하기 위한 접근자 프로퍼티다.

```js
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false
```

### `property` 프로퍼티

`property` 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, constructor만이 소유한다. 일반 객체, 생성자 함수로 호출할 수 없는 non-constructor에는 프로퍼티가 없다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty("prototype"); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty("prototype"); // -> false
```

`prototype` 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

```js
// 생성자 함수 정의
function Person(name) {
  this.name = name;
}

// Person 생성자 함수의 prototype 프로퍼티에 메서드 추가
Person.prototype.sayHello = function () {
  return `안녕하세요, 제 이름은 ${this.name}입니다.`;
};

// Person 생성자 함수의 prototype 프로퍼티에 프로퍼티 추가
Person.prototype.species = "인간";

// 생성자 함수의 prototype 객체 확인
console.log(Person.prototype);
// 출력 결과: {sayHello: ƒ, species: "인간", constructor: ƒ}
```
