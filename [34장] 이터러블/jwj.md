# 34장 이터러블

## 34.1 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)을 만들기 위해 ES 사양에 정의된 규칙이다.

ES6 이전에는 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약 없이 각자 나름의 구조로 `for 문`, `for...in` 문, `forEach` 메서드 등으로 순회할 수 있었다.

ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 `for...of` 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화했다.

- 이터러블 프로토콜(iterable protocol): `Symbol.iterator`를 프로퍼티 키로 사용한 메서드를 직접 구현하거나, 프로토타입 체인을 통해 상속받은 `Symbol.iterator` 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환하는 규약을 이터러블 프로토콜이라 하며, **이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 `for...of` 문으로 순회하며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.**
- 이터레이터 프로토콜(iterator protocol): 이터러블의 `Symbol.iterator` 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이터레이터는 `next` 메서드를 소유하며 `next` 메서드를 호출하면 이터러블을 순회하며 `value`와 `done` 프로퍼티를 갖는 **이터레이터 리절트 객체**를 반환한다. 이러한 규약을 이터레이터 프로토콜이라 하며, **이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다**. 이터레이터는 이터러블의 포인터 역할을 한다.

### 이터러블

이터러블은 이터러블 프로토콜을 준수한 객체를 말한다. (직접 구현했거나 프로토타입 체인을 통해 `Symbol.iterator`를 갖는 객체)

이터러블인지 확인하는 함수는 아래와 같다.

```js
const isIterable = (v) =>
  v !== null && typeof v[Symbol.iterable] === "function";

// 배열, 문자열, Map, Set 등은 이터러블이다.
isIterable([]); // -> true
isIterable(""); // -> true
isIterable(new Map()); // -> true
isIterable(new Set()); // -> true
isIterable({}); // -> false
```

배열은 `Array.prototpye`의 `Symbol.iterator` 메서드를 상속받는 이터러블이다. 이터러블은 `for...of` 문으로 순회하거나 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

```js
const array = [1, 2, 3];

// 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in array); // true

for (const item of array) {
  console.log(item); // 1 2 3
}

console.log([...array]); // [1, 2, 3]

const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

ES9에서는 일반 객체의 스프레드 문법의 사용을 허용한다.

```js
const obj = { a: 1, b: 2 };

console.log({ ...obj });
```

### 이터레이터

이터러블의 `Symbol.iterator` 메서드를 호출하면 이터레이터를 반환한다. 이터러블의 `Symbol.iterator` 메서드가 반환한 이터레이터는 **`next` 메서드**를 갖는다.

`next` 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 **이터레이터 리절트 객체**를 반환한다.

> 이터레이터 리절트 객체: 현재 순회 중인 이터러블의 값인 `value`와 순회 완료 여부를 나타내는 `done` 프로퍼티를 갖는 객체다.

```js
const array = [1, 2, 3];

const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 34.2 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다. 다음의 표준 빌트인 객체들은 빌트인 이터러블이다.

| 빌트인 이터러블             | Symbol.iterator 메서드                    |
| --------------------------- | ----------------------------------------- |
| Array                       | Array.prototype[Symbol.iterator]          |
| String                      | String.prototype[Symbol.iterator]         |
| Map                         | Map.prototype[Symbol.iterator]            |
| Set                         | Set.prototype[Symbol.iterator]            |
| TypedArray                  | TypedArray.prototype[Symbol.iterator]     |
| arguments                   | arguments[Symbol.iterator]                |
| DOM 컬렉션 (NodeList)       | NodeList.prototype[Symbol.iterator]       |
| DOM 컬렉션 (HTMLCollection) | HTMLCollection.prototype[Symbol.iterator] |

## 34.3 `for...of` 문

`for...of` 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.

```js
for (변수선언문 of 이터러블) { ... }
```

`for...of` 문은 내부적으로 이터레이터의 `next` 메서드를 호출하여 이터러블을 순회하며 `next` 메서드가 반환한 이터레이터 리절트 객체의 `value` 프로퍼티 값을 `for...of` 문의 변수에 할당한다. `done` 프로퍼티 값이 `false`면 순회를 계속하고 `true`면 중단한다.

```js
for (const item of [1, 2, 3]) {
  console.log(item); // 1 2 3
}
```

⚠️`for...in` 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `true`인 프로퍼티를 순회하며 열거한다. 단, 심벌인 프로퍼티는 열거하지 않는다.

## 34.4 이터러블과 유사 배열 객체

유사 배열 객체는 `Symbol.iterator` 메서드가 없기 때문에 `for...of` 문으로 순회할 수 없다.

단, `arguments`, `NodeList`, `HTMLCollection`은 유사 배열 객체이면서 이터러블이다. ES6에서 이터러블이 도입되면서 유사 배열 객체였던 이 셋의 내부에 `Symbol.iterator` 메서드가 구현되었다.

배열도 ES6에서 이터러블이 되었다.

💡유사 배열 객체는 `Array.from` 메서드를 사용하면 배열로 간단히 변환하여 `for...of` 메서드로 순회할 수 있다.

## 34.5 이터레이션 프로토콜의 필요성

많은 데이터 컬렉션이 하나의 인터페이스를 공유하면 `for...of`, 스프레드 문법, 배열 디스트럭처링 할당, `Map`/`Set` 생성자와 같은 데이터 소비자에 의해 효율적으로 다양한 데이터 컬렉션인 데이터 공급자를 사용할 수 있다.

이터레이션 프로토콜은 하나의 인터페이스로서의 역할을 수행한다.

## 34.6 사용자 정의 이터러블

### 사용자 정의 이터러블 구현

```js
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1]; // "36.1. 배열 디스트럭처링 할당" 참고
    const max = 10; // 수열의 최대값

    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고
    // next 메서드는 이터레이터 리절트 객체를 반환해야 한다.
    return {
      next() {
        [pre, cur] = [cur, pre + cur]; // "36.1. 배열 디스트럭처링 할당" 참고
        // 이터레이터 리절트 객체를 반환한다.
        return { value: cur, done: cur >= max };
      },
    };
  },
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출된다.
for (const num of fibonacci) {
  console.log(num); // 1 2 3 5 8
}

// 이터러블은 스프레드 문법의 대상이 될 수 있다.
const arr = [...fibonacci];
console.log(arr); // [ 1, 2, 3, 5, 8 ]

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first, second, ...rest] = fibonacci;
console.log(first, second, rest); // 1 2 [ 3, 5, 8 ]
```

### 이터러블을 생성하는 함수

`max` 값을 외부에서 전달하게 하는 이터러블을 생성하는 함수를 작성할 수 있다.

```js
// 피보나치 수열을 구현한 사용자 정의 이터러블을 반환하는 함수. 수열의 최대값을 인수로 전달받는다.
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
  return {
    [Symbol.iterator]() {
      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          return { value: cur, done: cur >= max };
        },
      };
    },
  };
};

// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
for (const num of fibonacciFunc(10)) {
  console.log(num); // 1 2 3 5 8
}
```

### 이터러블이면서 이터레이터인 객체를 생성하는 함수

이터러블이면서 이터레이터인 객체를 생성하면 `Symbol.iterator` 메서드를 호출하지 않아도 된다.

```js
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터인 객체를 반환
  return {
    [Symbol.iterator]() {
      return this;
    },
    // next 메서드는 이터레이터 리절트 객체를 반환
    next() {
      [pre, cur] = [cur, pre + cur];
      return { value: cur, done: cur >= max };
    },
  };
};

// iter는 이터러블이면서 이터레이터다.
let iter = fibonacciFunc(10);

// iter는 이터러블이므로 for...of 문으로 순회할 수 있다.
for (const num of iter) {
  console.log(num); // 1 2 3 5 8
}

// iter는 이터러블이면서 이터레이터다
iter = fibonacciFunc(10);

// iter는 이터레이터이므로 이터레이션 리절트 객체를 반환하는 next 메서드를 소유한다.
console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: 5, done: false }
console.log(iter.next()); // { value: 8, done: false }
console.log(iter.next()); // { value: 13, done: true }
```

### 무한 이터러블과 지연 평가

무한 이터러블을 통해 무한 수열을 간단히 구현할 수 있다.

```js
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
      return { value: cur };
    },
  };
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...4181 6765
}

// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

지연 평가는 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 비로소 데이터를 생성하는 기법이다.

위 예제는 `next` 메서드가 호출되기 전까지는 데이터를 생성하지 않는 점을 이용해 지연 평가를 사용하고 있다.

지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않아 빠른 실행 속도, 메모리 절약, 무한 표현 가능의 장점이 있다.
