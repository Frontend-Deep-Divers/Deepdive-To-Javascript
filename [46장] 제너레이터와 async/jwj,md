# 46장 제너레이터와 async/await

## 46.1 제너레이터란?

ES6에서 도입된 제너레이터(generator)는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.

제너레이터와 일반 함수의 차이는 다음과 같다.

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도(yield)할 수 있다.

   일반 함수와 달리, 함수 호출자가 함수 실행을 일시 중지하거나 재개시킬 수 있다.

2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.

   일반 함수와 달리, 제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.

3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

   제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

`function*` 키워드로 선언한다. 그리고 하나 이상의 `yield` 표현식을 포함한다.

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethod() {
    yield 1;
  }
}
```

애스터리스크의 위치는 `function` 키워드와 함수 이름 사이라면 어디든 상관없다.

제너레이터 함수는 화살표 함수로 정의할 수 없다.

제너레이터 함수는 `new` 연산자와 함께 생성자 함수로 호출할 수 없다.

## 46.3 제너레이터 객체

**제너레이터 함수를 호출하면 일반 함수처럼 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.**

이는 제너레이터 객체가 `Symbol.iterator` 메서드를 상속받는 이터러블이면서, `value`, `done` 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 `next` 메서드를 소유하는 이터레이터임을 의미한다.

하지만 이터레이터에는 없는 `return`, `throw` 메서드를 갖는다.

- `next` 메서드를 호출하면 제너레이터 함수의 `yield` 표현식까지 코드 블록을 실행하고 `yield`된 값을 `value` 프로퍼티 값으로, `false`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- `return` 메서드를 호출하면 인수로 전달받은 값을 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- `throw` 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 `undefined`를 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End!")); // {value: "End!", done: true}
// console.log(generator.throw("Error!")); // {value: undefined, done: true}
```

## 46.4 제너레이터의 일시 중지와 재개

`next` 메서드를 호출하면 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

이를 반복하며 제너레이터 함수가 끝까지 실행되면 이터레이터 리절트 객체의 `value` 프로퍼티에 제너레이터 함수의 반환값이 할당되고 `done` 프로퍼티에는 `true`가 할당된다.

이터레이터의 `next` 메서드와 달리 제너레이터의 `next` 메서드에는 인수를 전달할 수 있다. 전달한 인수는 제너레이터 함수의 `yield` 표현식을 할당받는 변수에 할당된다.

⚠️`yield` 표현식을 할당받는 변수에 `yield` 표현식의 평가 결과가 할당되지 않는 것에 주의해야 한다.

```js
function* genFunc() {
  // 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
  // 이때 yield된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  // x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다.
  const x = yield 1;

  // 두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당된다.
  // 즉, const x = yield 1;은 두 번째 next 메서드를 호출했을 때 완료된다.
  // 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
  // 이때 yield된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  const y = yield x + 10;

  // 세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당된다.
  // 즉, const y = yield (x + 10);는 세 번째 next 메서드를 호출했을 때 완료된다.
  // 세 번째 next 메서드를 호출하면 함수 끝까지 실행된다.
  // 이때 제너레이터 함수의 반환값 x + y는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  // 일반적으로 제너레이터의 반환값은 의미가 없다.
  // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다.
  return x + y;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이며 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
// 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 첫 번째 yield된 값 1이 할당된다.
let res = generator.next();
console.log(res); // {value: 1, done: false}

// next 메서드에 인수로 전달한 10은 genFunc 함수의 x 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
res = generator.next(10);
console.log(res); // {value: 20, done: false}

// next 메서드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당된다.
res = generator.next(20);
console.log(res); // {value: 30, done: true}
```

함수 호출자는 `next` 메서드를 통해 `yield` 표현식까지 함수를 실행시켜 제너레이터 객체가 관리하는 상태(`yield`된 값)를 꺼내올 수 있고, `next` 메서드에 인수를 전달해서 제너레이터 객체에 상태(`yield` 표현식을 할당받는 변수)를 밀어넣을 수 있다. 이러한 특성을 이용해 비동기 처리를 동기 처리처럼 구현할 수 있다.

## 46.5 제너레이터의 활용

### 이터러블의 구현

이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간편하게 이터러블을 구현할 수 있다.

제너레이터를 사용하여 무한 피보나치 수열을 생성하는 함수를 구현해보자.

```js
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한 이터러블이므로 done 프로퍼티를 생략한다.
      return { value: cur };
    },
  };
})();

// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

### 비동기 처리

제너레이터 함수의 특성을 활용해 프로미스의 후속 처리 메서드 `then`/`catch`/`finally` 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```js
// node-fetch는 node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.
// https://github.com/node-fetch/node-fetch
const fetch = require("node-fetch");

// 제너레이터 실행기
const async = (generatorFunc) => {
  const generator = generatorFunc(); // ②

  const onResolved = (arg) => {
    const result = generator.next(arg); // ⑤

    return result.done
      ? result.value // ⑨
      : result.value.then((res) => onResolved(res)); // ⑦
  };

  return onResolved; // ③
};

async(function* fetchTodo() {
  // ①
  const url = "https://jsonplaceholder.typicode.com/todos/1";

  const response = yield fetch(url); // ⑥
  const todo = yield response.json(); // ⑧
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
})(); // ④
```

1. `async` 함수가 호출되면 `fetchTodo` 호출하여 제너레이터 객체 생성하고 `onResolved`를 반환한다. `async` 함수가 반환한 `onResolved` 함수를 즉시 호출하여 제너레이터 객체의 `next` 메서드를 처음 호출한다.
2. 첫 `yield` 문까지 실행되고, `done` 프로퍼티의 값이 `false`인 경우(`fetchTodo`가 끝까지 실행되지 않은 경우), `value` 프로퍼티 값(첫 번쨰 `yield`된 `fetch` 함수가 반환한 프로미스가 `resolve`한 `Response` 객체)을 `onResolved` 함수에 인수로 전달하면서 재귀 호출한다.
3. `Response` 객체를 `next` 메서드에 인수로 전달하면서 `next` 메서드를 두 번째로 호출한다. 이때 `Response` 객체는 `fetchTodo`의 `response` 변수에 할당되고 두 번째 `yield` 문까지 실행된다.
4. `done` 프로퍼티 값이 `false`(`fetchTodo`가 끝까지 실행되지 않은 경우) `value` 프로퍼티 값(두 번째 `yield`된 `response.json` 메서드가 반환한 프로미스가 `resolve`한 `todo` 객체)을 `onResolved` 함수에 인수로 전달하면서 재귀 호출한다.
5. `todo` 객체를 `next` 메서드에 인수로 전달하면서 `next` 메서드를 세 번째로 호출한다. 이때 `todo` 객체가 `fetchTodo`의 `todo` 변수에 할당되고 `fetchTodo`가 끝까지 실행된다.
6. `done` 프로퍼티의 값이 `true`가 되면 `value` 프로퍼티 값(`fetchTodo`의 반환값인 `undefined`)를 그대로 반환하고 처리를 종료한다.

`async`/`await`를 사용하면 제너레이터 실행기를 사용할 필요가 없고, 필요하다면 직접 구현하는 것보다 co 라이브러리를 사용하는 것이 권장된다.

```JS
const fetch = require('node-fetch');
// https://github.com/tj/co
const co = require('co');

co(function* fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = yield fetch(url);
  const todo = yield response.json();
  console.log(todo);
  // { userId: 1, id: 1, title: 'delectus aut autem', completed: false }
});
```

## 46.6 `async`/`await`

ES8부터 도입되었다.

프로미스 기반으로 동작하며 프로미스의 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.

```js
const fetch = require("node-fetch");

async function fetchTodo() {
  const url = "https://jsonplaceholder.typicode.com/todos/1";

  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}

fetchTodo();
```

### `async` 함수

`await` 키워드는 반드시 `async` 함수 내부에서 사용해야 한다. `async` 함수는 언제나 프로미스를 반환한다. 명시적으로 그렇지 않더라도 암묵적으로 반환값을 `resolve`하는 프로미스를 반환한다.

```js
// async 함수 선언문
async function foo(n) {
  return n;
}
foo(1).then((v) => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) {
    return n;
  },
};
obj.foo(4).then((v) => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) {
    return n;
  }
}
const myClass = new MyClass();
myClass.bar(5).then((v) => console.log(v)); // 5
```

클래스의 `constructor`는 `async` 함수가 될 수 없다.

### `await` 키워드

프로미스가 `settled` 상태가 될 때까지 대기하다가 `setteld` 상태가 되면 프로미스가 `resolve`한 처리 결과를 반환한다. 반드시 해당 키워드는 프로미스 앞에서 사용해야 한다.

모든 키워드에 `await` 키워드를 사용하는 것은 주의해야 한다. 비동기 처리가 서로 연관없이 개별적으로 수행될 땐 앞선 비동기 처리가 완료될 때까지 대기하며 순차적으로 처리할 필요가 없다.

```js
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.
```

```js
async function foo() {
  const res = await Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
  ]);

  console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요된다.
```

앞선 비동기 처리의 결과로 다음 비동기 처리를 수행해야 할 땐, 모든 프로미스에 `await` 키워드를 써서 순차적으로 처리할 수 밖에 없다.

```js
async function bar(n) {
  const a = await new Promise((resolve) => setTimeout(() => resolve(n), 3000));
  // 두 번째 비동기 처리를 수행하려면 첫 번째 비동기 처리 결과가 필요하다.
  const b = await new Promise((resolve) =>
    setTimeout(() => resolve(a + 1), 2000)
  );
  // 세 번째 비동기 처리를 수행하려면 두 번째 비동기 처리 결과가 필요하다.
  const c = await new Promise((resolve) =>
    setTimeout(() => resolve(b + 1), 1000)
  );

  console.log([a, b, c]); // [1, 2, 3]
}

bar(1); // 약 6초 소요된다.
```

### 에러 처리

`async`/`await`에서 에러 처리는 `try...catch` 문을 사용할 수 있다.

```js
const fetch = require("node-fetch");

const foo = async () => {
  try {
    const wrongUrl = "https://wrong.url";

    const response = await fetch(wrongUrl);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err); // TypeError: Failed to fetch
  }
};

foo();
```

위 `foo` 함수는 HTTP 통신에서 발생한 네트워크 에러뿐 아니라 `try` 코드 블록 내 모든 문에서 발생한 일반적인 에러까지 모두 캐치할 수 있다.

`catch` 문을 사용해서 에러 처리하지 않으면 `async` 함수는 발생한 에러를 `reject`하는 프로미스를 반환한다. 따라서 `async` 함수 호출 후 `Promise.prototype.catch` 메서드를 사용해 에러를 캐치할 수도 있다.
