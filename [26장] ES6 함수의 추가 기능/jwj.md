# 26장 ES6 함수의 추가 기능

## 26.1 함수의 구분

ES6 이전까지 자바스크립트의 함수는 다양한 목적으로 사용되곤 했다.

- 일반적인 함수로 호출
- `new`와 함께 호출해 인스턴스를 생성하는 생성자 함수로 호출
- 객체에 바인딩되어 메서드로서 호출

편리하긴 하지만 실수를 유발하고 성능 면에서도 좋지 않았다.

ES6 이전의 모든 함수는 `callable`이면서 `constructor`다.

`constructor`라는 것은 함수가 `prototype` 프로퍼티를 가지며, 프로토타입 객체도 생성한다는 것을 의미한다. 이는 성능 면에서 유리하지 않다.

이를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 구분했다.

| ES6 함수의 구분     | constructor | prototype | super | arguments |
| ------------------- | ----------- | --------- | ----- | --------- |
| 일반 함수 (Normal)  | ⭕          | ⭕        | ❌    | ⭕        |
| 메서드 (Method)     | ❌          | ❌        | ⭕    | ⭕        |
| 화살표 함수 (Arrow) | ❌          | ❌        | ❌    | ❌        |

## 26.2 메서드

**ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.**

메서드는 non-constructor다. 때문에 `prototype` 프로퍼티도 없고 프로토타입도 생성하지 않는다.

표준 빌트인 객체의 프로토타입 메서드와 정적 메서드도 모두 non-constructor다.

**ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 `[[HomeObject]]`를 갖는다.** `super` 참조(수퍼클래스의 메서드 참조)가 이루어지기 위해서 필요하다. ES6가 아닌 메서드는 `super` 키워드를 사용할 수 없다.

메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전 방식은 사용하지 않는 것이 좋다.

## 26.3 화살표 함수

내부 동작이 기존 함수보다 간략하다.

화살표 함수는 콜백 함수 내부에서 `this`가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

### 화살표 함수 정의

- 함수 정의

  함수 표현식으로만 정의할 수 있다.

  ```js
  const add = (x, y) => x + y;
  add(1, 2);
  ```

- 매개변수 선언

  매개변수가 여러 개인 경우 소괄호 안에 매개변수를 선언한다.

  매개변수가 한 개인 경우 소괄호를 생략할 수 있다.

  매개변수가 없는 경우 소괄호를 생략할 수 없다.

- 함수 몸체 정의

  함수 몸체가 하나의 문으로 구성된다면 중괄호를 생략할 수 있다. 이때 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다. 표현식이 아닌 문이면 에러를 발생시킨다.

  ```js
  // concise body
  const power = (x) => x ** 2;
  power(2); // 4
  ```

  객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸야 한다.

  화살표 함수도 즉시 실행 함수로 사용할 수 있다.

  ```js
  const person = ((name) => ({
    sayHi() {
      return `Hello, ${name}`;
    },
  }))("Jeon");
  ```

  화살표 함수를 고차 함수 인수로 전달할 수 있다. 표현이 간결하고 가독성이 좋다.

  ```js
  [1, 2, 3].map((v) => v * 2);
  ```

  💡화살표 함수는 콜백 함수로서 정의할 때 유용하다.

### 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.

2. 중복된 매개변수 이름을 선언할 수 없다.

3. 화살표 함수는 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않는다.

따라서 화살표 함수 내부에서 `this`, `arguments`, `super`, `new.target`을 참조하면 스코프 체인을 통해 `this`, `arguments`, `super`, `new.target`을 참조한다.

### `this`

콜백 함수 내부의 `this`는 외부 함수의 `this`와 다른 객체를 가리킬 가능성이 있다.

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    // ①
    return arr.map(function (item) {
      return this.prefix + item; // ②
      // -> TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
```

해결 방법은 다음과 같다.

1. `add` 메서드를 호출한 `prefixer` 객체를 가리키는 `this`를 일단 회피시킨 후 콜백 함수 내부에서 사용한다.

```js
const that = this;
return arr.map(function (item) {
  return that.prefix + " " + item;
});
```

2. `Array.prototype.map`의 두 번째 인수에 `this`를 전달한다.

```js
return arr.map(function (item) {
  return this.prefix + " " + item;
}, this);
```

3. `bind` 메서드를 사용하여 `add` 메서드를 호출한 `prefixer` 객체를 가리키는 `this`를 바인딩한다.

```js
return arr.map(
  function (item) {
    return this.prefix + " " + item;
  }.bind(this);
);
```

ES6에서는 화살표 함수를 사용하면 이 문제를 쉽게 해결할 수 있다.

화살표 함수는 함수 자체의 `this` 바인딩을 갖지 않고, 화살표 함수 내부에서 `this`를 참조하면 상위 스코프의 `this`를 그대로 참조한다. 이를 lexical this라고 한다. 렉시컬 스코프와 같이 `this`가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.

화살표 함수는 함수 자체의 `this` 바인딩이 없어 `call`, `apply`, `bind` 메서드를 사용해도 화살표 함수 내부의 `this` 바인딩을 교체할 수 없다.

💡메서드는 메서드 축약 표현으로 선언하는 것이 좋다. (`this` 바인딩이 자연스럽지 않다.)

💡프로토타입 객체에 프로퍼티를 동적으로 추가하고 싶을 땐 일반 함수를 할당한다. 만약 ES6 메서드를 동적 추가하고 싶으면 객체 리터럴을 통해 `constructor` 프로퍼티와 생성자 함수 간의 연결을 재설정해주어야 한다.

```js
function Person(name) {
  this.name = name;
}

Person.prototype = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
  constructor: Person,
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};
```

클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할 수도 있다.

```js
// Bad
class Person {
  // 클래스 필드 정의 제안
  name = "Lee";
  sayHi = () => console.log(`Hi ${this.name}`);
  // 클래스 필드 정의 제안은 아래 코드와 동일하다.
  //   constructor(name) {
  //     this.name = name;
  //     this.sayHi = () => console.log(`Hi ${this.name}`);
  //   }
}
```

⚠️클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 따라서 메서드 정의 시엔 메서드 축약 표현을 사용하는 것이 좋다.

### `super`

화살표 함수 내부에서 `super`를 참조하면 `this`와 마찬가지로 상위 스코프의 `super`를 참조한다.

### `arguments`

화살표 함수는 함수 자체의 `arguments` 바인딩이 없다. `this`와 마찬가지로 상위 스코프의 `arguments`를 참조한다.

따라서 화살표 함수로 가변 인자 함수를 구현해야 할 땐 반드시 Rest 파라미터를 사용해야 한다.

## 26.4 Rest 파라미터

### 기본 문법

Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세 개의 점 `...`을 붙여서 정의한 매개변수를 말한다. **Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.**

```js
function foo(...rest) {
  console.log(rest); // [ 1, 2, 3 ]
}

foo(1, 2, 3);
```

일반 매개변수와 함께 사용할 수도 있다. 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당된다.

```js
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest); // [ 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

Rest 파라미터는 반드시 마지막 파라미터여야 한다. 그렇지 않으면 에러를 발생시킨다.

Rest 파라미터는 단 하나만 선언할 수 있다.

Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 `length` 프로퍼티에 영향을 주지 않는다.

### Rest 파라미터와 `arguments` 객체

`arguments`는 유사 배열 객체이므로 배열 메서드를 사용하려면 `call`이나 `apply` 메서드를 사용해 배열로 변환해야 한다.

```js
function sum() {
  // 유사 배열 객체인 arguments 객체를 배열로 변환한다.
  var array = Array.prototype.slice.call(arguments);

  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

💡Rest 파라미터를 사용하면 가변 인자 함수의 인수 목록을 배열로 직접 받을 수 있다.

## 26.5 매개변수 기본값

자바스크립트 엔진은 매개변수의 개수와 인수의 개수를 체크하지 않아 의도치 않은 결과를 발생시킬 수 있다.

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```js
function sum(x = 0, y = 0) {
  return x + y;
}
```

매개변수 기본값은 매개변수에 인수를 전달하지 않았을 때와 `undefined`를 전달한 경우에만 유효하다.

Rest 파라미터에는 기본값을 지정할 수 없다.

매개변수 기본값은 `length` 프로퍼티와 `arguments` 객체에 아무런 영향을 주지 않는다.
