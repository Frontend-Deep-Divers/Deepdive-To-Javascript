# 45장 프로미스

비동기 처리의 전통적인 방식인 콜백 패턴은 콜백 헬로 인해 가독성이 좋지 않고 비동기 처리 중 발생하는 에러의 처리가 어려우며 여러 개의 비동기 처리르 한 번에 처리하는 데도 한계가 있다.

ES6에서 비동기 처리를 위한 또 다른 패턴으로 프로미스(Promise)가 도입되었다. **전통적인 콜백 패턴이 가진 단점을 보완하고 비동기 처리 시점을 명확하게 표현할 수 있는 장점이 있다**.

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

### 콜백 헬

비동기 함수는 함수 내부에 비동기로 동작하는 코드를 포함한 함수를 말한다.

비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않더라도 즉시 종료된다. 따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프에 변수에 할당하면 기대한 대로 동작하지 않는다.

```js
let g = 0;

// 비동기 함수인 setTimeout 함수는 콜백 함수의 처리 결과를 외부로 반환하거나
// 상위 스코프의 변수에 할당하지 못한다.
setTimeout(() => {
  g = 100;
}, 0);
console.log(g); // 0
```

그러므로 내부의 비동기로 동작하는 코드의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다. 이때 비동기 함수에 콜백 함수를 전달하여 콜백 함수가 이를 처리하게 하는 것이 일반적이다.

필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 실패하면 호출될 콜백 함수를 전달한다.

```js
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 한다.
      successCallback(JSON.parse(xhr.response));
    } else {
      // 에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리를 한다.
      failureCallback(xhr.status);
    }
  };
};

// id가 1인 post를 취득
// 서버의 응답에 대한 후속 처리를 위한 콜백 함수를 비동기 함수인 get에 전달해야 한다.
get("https://jsonplaceholder.typicode.com/posts/1", console.log, console.error);
/*
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere ...",
  "body": "quia et suscipit ..."
}
*/
```

이처럼 **콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생**하는데, 이를 콜백 헬이라 한다.

다음은 콜백 헬이 발생하는 전형적인 사례다. 각 단계에서 발생한 비동기 처리 결과를 가지고 연속으로 비동기 함수를 호출하고 있다.

```js
get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

### 에러 처리의 한계

콜백 패턴의 문제점 중 가장 심각한 것은 에러 처리가 곤란하다는 것이다.

```js
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.error("캐치한 에러", e);
}
```

에러는 호출자(caller) 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다. 하지만 콜백 함수를 호출한 건 `setTimeout` 함수가 아니므로 `setTimeout` 함수의 콜백 함수가 발생시킨 에러는 `catch` 블록에서 캐치되지 않는다.

## 45.2 프로미스의 생성

프로미스는 **비동기 처리 상태와 처리 결과를 관리하는 객체**다.

`Promise` 생성자 함수를 `new` 연산자와 함께 호출하면 프로미스(`Promise` 객체)를 생성한다. `Promise`는 ES 사양에 정의된 표준 빌트인 객체다.

`Promise` 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데, 이 콜백 함수는 `resolve`와 `reject` 함수를 인수로 전달받는다.

```js
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
  if (/* 비동기 처리 성공 */) {
    resolve('result');
  } else { /* 비동기 처리 실패 */
    reject('failure reason');
  }
});
```

전달받은 콜백 함수 내부에서 비동기 처리를 수행하며, 비동기 처리가 성공하면 `resolve` 함수를 호출하고, 실패하면 `reject` 함수를 호출한다.

앞서 본 `get` 함수는 `Promise`를 사용하여 아래와 같이 구현할 수 있다.

```js
// GET 요청을 위한 비동기 함수
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수를 호출한다.
        reject(new Error(xhr.status));
      }
    };
  });
};

// promiseGet 함수는 프로미스를 반환한다.
promiseGet("https://jsonplaceholder.typicode.com/posts/1");
```

프로미스는 비동기 처리 상태에 대한 상태 정보를 갖는다.

| 프로미스의 상태 정보 | 의미                                  | 상태 변경 조건                   |
| -------------------- | ------------------------------------- | -------------------------------- |
| `pending`            | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
| `fulfilled`          | 비동기 처리가 수행된 상태(성공)       | `resolve` 함수 호출              |
| `rejected`           | 비동기 처리가 수행된 상태(실패)       | `reject` 함수 호출               |

생성된 직후에는 `pending` 상태를 가지며, 이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 프로미스의 상태가 변경된다.

- 비동기 처리 성공 시: `resolve` 함수를 호출해 프로미스를 `fulfilled` 상태로 변경
- 비동기 처리 실패 시: `reject` 함수를 호출해 프로미스를 `rejected` 상태로 변경

이처럼 프로미스의 상태는 `resolve` 또는 `reject` 함수를 호출하는 것으로 결정된다.

`fulfilled`와 `reject` 상태를 모두 `settled` 상태라고 한다. `settled` 상태는 `pending` 상태가 아닌 비동기 처리가 수행된 상태를 말한다. `settled` 상태가 되면 더는 다른 상태로 변화할 수 없다.

프로미스는 비동기 처리 결과(`result`)도 상태로 갖는다. `fulfilled` 상태에서는 `value`를, `rejected` 상태에서는 `error`를 갖는다.

## 45.3 프로미스의 후속 처리 메서드

프로미스의 비동기 처리 상태 변화에 따른 후속 처리를 할 때, `then`, `catch`, `finally` 메서드를 사용할 수 있다.

프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다. 이때 후속 처리 메서드의 콜백 함수에 프로미스의 처리 결과가 인수로 전달된다.

모든 후속 처리 메서드는 프로미스를 반환하며, 비동기로 동작한다.

### `Promise.prototype.then`

두 개의 콜백 함수를 인수로 전달받는다.

- 첫 번째 콜백 함수는 프로미스가 `fulfilled` 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
- 첫 번째 콜백 함수는 프로미스가 `rejected` 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.

```js
// fulfilled
new Promise((resolve) => resolve("fulfilled")).then(
  (v) => console.log(v),
  (e) => console.error(e)
); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(
  (v) => console.log(v),
  (e) => console.error(e)
); // Error: rejected
```

`then` 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고, 콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 `resolve` 또는 `reject`하여 프로미스를 생성해 반환한다.

### `Promise.prototype.catch`

한 개의 콜백 함수를 인수로 전달받는다. `catch` 메서드의 콜백 함수는 프로미스가 `rejected` 상태인 경우만 호출된다.

```js
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) =>
  console.log(e)
); // Error: rejected
```

`then(undeifned, onRejected)`과 동일하게 동작한다. 언제나 프로미스를 반환한다.

```js
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(undefined, (e) =>
  console.log(e)
); // Error: rejected
```

### `Promise.prototype.finally`

한 개의 콜백 함수를 전달받는다. `finally` 메서드의 콜백 함수는 프로미스의 상태에 관계없이 무조건 한 번 호출된다.

```js
new Promise(() => {}).finally(() => console.log("finally")); // finally
```

프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다. `finally` 메서드도 `then/catch` 메서드와 마찬가지로 언제나 프로미스를 반환한다.

## 45.4 프로미스의 에러 처리

프로미스의 후속 처리 메서드를 이용하여 에러를 처리할 수 있다. `then` 메서드 이후 `catch` 메서드를 사용하는 것이 안정적이다.

```js
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseGet(wrongUrl)
  .then((res) => console.log(res))
  .catch((err) => console.error(err)); // Error: 404
```

## 45.5 프로미스 체이닝

프로미스의 후속 처리 메서드를 이용해 콜백 헬을 해결할 수 있다.

후속 처리 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하더라도 그 값을 암묵적으로 `resolve` 또는 `reject`하여 프로미스를 생성해 반환한다.

프로미스 체이닝 역시 콜백 패턴을 사용하므로 이는 `async`/`await`를 통해 해결할 수 있다. `async`/`await`는 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

```js
const url = "https://jsonplaceholder.typicode.com";

(async () => {
  // id가 1인 post의 userId를 취득
  const { userId } = await promiseGet(`${url}/posts/1`);

  // 취득한 post의 userId로 user 정보를 취득
  const userInfo = await promiseGet(`${url}/users/${userId}`);

  console.log(userInfo);
})();
```

## 45.6 프로미스의 정적 메서드

`Promise`는 5가지 정적 메서드를 제공한다.

### `Promise.resolve` / `Promise.reject`

이 두 가지 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.

```js
// 배열을 resolve하는 프로미스를 생성
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```

다음 예제와 동일하게 동작한다.

```js
const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [1, 2, 3]
```

### `Promise.all`

여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.

```js
const requestData1 = () =>
  new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () =>
  new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () =>
  new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.all([requestData1(), requestData2(), requestData3()])
  .then(console.log) // [ 1, 2, 3 ] ⇒ 약 3초 소요
  .catch(console.error);
```

`Promise.all` 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 그리고 전달받은 모든 프로미스가 모두 `fulfilled` 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다. 하나라도 `rejected` 상태가 되면 나머지 프로미스가 `fulfilled` 상태가 되는 것을 기다리지 않고 즉시 종료한다.

종료하는 데 걸리는 시간은 가장 늦게 `fulfilled` 상태가 되는 프로미스의 처리 시간보다 조금 더 길다.

처리 결과를 차례대로 배열에 저장해 그 배열을 `resolve`하는 새로운 프로미스를 반환한다. 즉, 처리 순서가 보장된다.

인수로 전달받은 이터러블의 요소가 프로미스가 아닌 경우 `Promise.resolve` 메서드를 통해 프로미스로 래핑한다.

```js
// GET 요청을 위한 비동기 함수
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수를 호출한다.
        reject(new Error(xhr.status));
      }
    };
  });
};

const githubIds = ["jeresig", "ahejlsberg", "ungmo2"];

Promise.all(
  githubIds.map((id) => promiseGet(`https://api.github.com/users/${id}`))
)
  // [Promise, Promise, Promise] => Promise [userInfo, userInfo, userInfo]
  .then((users) => users.map((user) => user.name))
  // [userInfo, userInfo, userInfo] => Promise ['John Resig', 'Anders Hejlsberg', 'Ungmo Lee']
  .then(console.log)
  .catch(console.error);
```

### `Promise.race`

`Promise.all` 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 가장 먼저 `fulfilled` 상태가 된 프로미스의 처리 결과를 `resolve`하는 새로운 프로미스를 반환한다.

```js
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

프로미스가 `rejected` 상태가 되면 `Promise.all` 메서드와 동일하게 처리된다.

### `Promise.allSettled`

ES11에 도입되었다. 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 전달받은 프로미스가 모두 `settled` 상태가 되면 처리 결과를 배열로 반환한다.

```JS
Promise.allSettled([
  new Promise(resolve => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) => setTimeout(() => reject(new Error('Error!')), 1000))
]).then(console.log);
/*
  // 프로미스가 fulfilled 상태인 경우
  {status: "fulfilled", value: 1},
  // 프로미스가 rejected 상태인 경우
  {status: "rejected", reason: Error: Error! at <anonymous>:3:60}
]
*/
```

## 45.7 마이크로태스크 큐

마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장된다. 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러는 태스크 큐에 일시 저장된다.

마이크로태스크 큐는 태스크 큐보다 우선순위가 높다. 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다. 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.

## 45.8 `fetch`

HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다. `XMLHttpRequest` 객체보다 사용법이 간단하고 프로미스를 지원해 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.

`fetch` 함수에는 인수로 HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달한다. URL만 전달하면 GET 요청을 전송한다.

`fetch` 함수는 HTTP 응답을 나타내는 `Response` 객체를 래핑한 `Promise` 객체를 반환한다.

`Response` 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공한다.

`Response.prototype`에는 `Response` 객체에 포함되어 있는 HTTP 응답 몸체를 위한 다양한 메서드를 제공한다. 예를 들어, 프로미스가 래핑하고 있는 MIME 타입이 `application/json`인 HTTP resposne body를 취득하려면 `Response.prototype.json` 메서드를 사용한다. 이 메서드는 response body를 취득해 역직렬화한다.

```js
fetch("https://jsonplaceholder.typicode.com/todos/1")
  // response는 HTTP 응답을 나타내는 Response 객체이다.
  // json 메서드를 사용하여 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화한다.
  .then((response) => response.json())
  // json은 역직렬화된 HTTP 응답 몸체이다.
  .then((json) => console.log(json));
// {userId: 1, id: 1, title: "delectus aut autem", completed: false}
```

⚠️`fetch` 함수가 반환하는 프로미스는 불리언 타입의 `ok` 상태를 `false`로 설정한 `Response` 객체를 `resolve`한다. (`404 Not Found`나 `500 Internal Server Error`와 같은 HTTP 에러를 `reject`하지 않는다.) 오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 `reject`한다.

```js
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL이 지정되었기 때문에 404 Not Found 에러가 발생한다.
fetch(wrongUrl)
  // response는 HTTP 응답을 나타내는 Response 객체다.
  .then((response) => {
    if (!response.ok) throw new Error(response.statusText);
    return response.json();
  })
  .then((todo) => console.log(todo))
  .catch((err) => console.error(err));
```

💡axios는 모든 HTTP 에러를 reject하는 프로미스를 반환한다.

다음은 `fetch` 함수를 이용해 HTTP 요청을 전송하는 예제다.

```js
const request = {
  get(url) {
    return fetch(url);
  },
  post(url, payload) {
    return fetch(url, {
      method: "POST",
      headers: { "content-Type": "application/json" },
      body: JSON.stringify(payload),
    });
  },
  patch(url, payload) {
    return fetch(url, {
      method: "PATCH",
      headers: { "content-Type": "application/json" },
      body: JSON.stringify(payload),
    });
  },
  delete(url) {
    return fetch(url, { method: "DELETE" });
  },
};
```
