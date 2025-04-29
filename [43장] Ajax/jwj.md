# 43장 Ajax

## 43.1 Ajax란?

Ajax(Asynchronous JavaScript and XML)은 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다. Ajax는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작한다. `XMLHttpRequest`는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.

1999년 마이크로소프트가 개발한 `XMLHttpRequest`는 2005년 구글이 발표한 Ajax를 기반으로 동작하는 구글 맵스를 통해 빛을 볼 수 있었다.

이전에는 완전한 HTML을 서버로 전송받아 웹페이지 전체 렌더링을 반복하는 방식이었다.

Ajax를 통해 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 불필요한 리렌더링을 줄일 수 있게 됐다.

## 43.2 JSON

JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷이다.

### JSON 표기 방식

자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수 텍스트다.

JSON의 키는 반드시 큰따옴표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용한다.

### `JSON.stringify`

객체 또는 배열을 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화라 한다.

```js
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환되지 않는다.
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === "number" ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "Lee",
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/
```

### `JSON.parse`

JSON 포맷의 문자열을 객체로 변환한다. 문자열을 객체로서 사용하기 위해 객체화하는 것을 역직렬화라 한다.

## 43.3 XMLHttpRequest

브라우저는 주소창이나 HTML의 `form` 태그 또는 `a` 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바스크립트를 사용하여 HTTP 요청을 전송하려면 `XMLHttpRequest` 객체를 사용한다.

`XMLHttpRequest` 객체는 HTTP 요청 전송과 응답 수신을 위한 메서드와 프로퍼티를 제공한다.

### `XMLHttpRequest` 객체 생성

`XMLHttpRequest` 객체는 `XMLHttpRequest` 생성자 함수를 호출하여 생성한다. Web API이므로 브라우저 환경에서만 사용할 수 있다.

```js
const xhr = new XMLHttpRequest();
```

### `XMLHttpRequest` 객체의 프로퍼티와 메서드

`XMLHttpRequest` 객체의 대표적인 프로퍼티와 메서드는 다음과 같다.

- 프로토타입 프로퍼티

| 프로토타입 프로퍼티 | 설명                                                                                                                      |
| :------------------ | :------------------------------------------------------------------------------------------------------------------------ |
| **readyState**      | HTTP 요청의 현재 상태를 나타내는 정수<br>• 0: UNSENT<br>• 1: OPENED<br>• 2: HEADERS_RECEIVED<br>• 3: LOADING<br>• 4: DONE |
| **status**          | HTTP 요청에 대한 응답 상태 (HTTP 상태 코드)<br>예: 200                                                                    |
| **statusText**      | HTTP 요청에 대한 응답 메시지를 나타내는 문자열<br>예: "OK"                                                                |
| **responseType**    | 응답 데이터의 타입 지정<br>예: `"document"`, `"json"`, `"text"`, `"blob"`, `"arraybuffer"`                                |
| **response**        | HTTP 요청에 대한 응답 본문 (타입은 `responseType`에 따라 달라짐)                                                          |
| responseText        | 서버가 전송한 HTTP 응답의 **문자열 형태**                                                                                 |

- 이벤트 핸들러 프로퍼티

| 이벤트 핸들러 프로퍼티 | 설명                                            |
| :--------------------- | :---------------------------------------------- |
| **onreadystatechange** | `readyState` 프로퍼티 값이 변경된 경우          |
| onloadstart            | HTTP 요청에 대한 응답을 받기 시작한 경우        |
| onprogress             | HTTP 응답을 받는 도중 주기적으로 발생           |
| onabort                | `abort()` 메서드에 의해 HTTP 요청이 중단된 경우 |
| **onerror**            | HTTP 요청 중 에러가 발생한 경우                 |
| **onload**             | HTTP 요청이 성공적으로 완료된 경우              |
| ontimeout              | HTTP 요청 시간이 초과된 경우                    |
| onloadend              | HTTP 요청이 완료된 경우 (성공/실패 모두 포함)   |

- 메서드

| 메서드               | 설명                                     |
| :------------------- | :--------------------------------------- |
| **open**             | HTTP 요청 초기화                         |
| **send**             | HTTP 요청 전송                           |
| **abort**            | 이미 전송된 HTTP 요청 중단               |
| **setRequestHeader** | 특정 HTTP 요청 헤더의 값을 설정          |
| getResponseHeader    | 특정 HTTP 응답 헤더의 값을 문자열로 반환 |

- 정적 프로퍼티

| 정적 프로퍼티    | 값  | 설명                               |
| :--------------- | :-: | :--------------------------------- |
| UNSENT           |  0  | `open()` 메서드 호출 이전 상태     |
| OPENED           |  1  | `open()` 메서드 호출 이후 상태     |
| HEADERS_RECEIVED |  2  | `send()` 메서드 호출 이후 상태     |
| LOADING          |  3  | 서버 응답 중 (응답 데이터 수신 중) |
| **DONE**         |  4  | 서버 응답 완료                     |

### HTTP 요청 전송

1. `XMLHttpRequest.prototype.open` 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 `XMLHttpRequest.prototype.setRequestHeader` 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. `XMLHttpRequest.prototype.send` 메서드로 HTTP 요청을 전송한다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("POST", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

- `XMLHttpRequest.prototype.open`

`open` 메서드로 서버에 전송할 HTTP 요청을 초기화한다.

```js
xhr.open(method, url[, async])
```

> `method`: HTTP 요청 메서드, `url`: HTTP 요청 전송 url, `async`: 디폴트 값은 `true`(비동기 방식)

- `XMLHttpRequest.prototype.send`

`send` 메서드로 초기화된 HTTP 요청을 서버로 전송한다.

`GET` 요청은 데이터를 URL의 일부분인 쿼리 문자열(query string)로 전송한다.

`POST` 요청은 데이터를 request body에 담아 전송한다.

`send` 메서드의 인수로 데이터(페이로드)를 전달할 수 있다. 객체인 경우, 직렬화한 다음 전달해야 한다.

```js
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

`GET` 요청인 경우 페이로드는 무시되고 request body는 `null`로 설정된다.

- `XMLHttpRequest.prototype.setRequestHeader`

`setRequestHeader` 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. 자주 사용되는 HTTP 요청 헤더는 `Content-type`과 `Accept`가 있다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("POST", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

`Content-type`은 request body에 담아 전송할 데이터를 MIME 타입의 정보를 표현한다. 자주 사용하는 MIME 타입은 다음과 같다.

| MIME 타입   | 서브타입                                            |
| :---------- | :-------------------------------------------------- |
| text        | text/plain, text/html, text/css, text/javascript    |
| application | application/json, application/x-www-form-urlencoded |
| multipart   | multipart/form-data                                 |

서버가 응답할 데이터의 MIME 타입을 `Accept`로 지정할 수 있다. 만약 `Accept` 헤더를 설정하지 않으면 `send` 메서드가 호출될 때 `Accept` 헤더가 */*으로 전송된다.

```js
// 서버가 응답할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("accept", "application/json");
```

### HTTP 응답 처리

응답을 처리하려면 `XMLHttpRequest` 객체가 발생시키는 이벤트를 캐치해야 한다. `XMLHttpRequest` 객체가 갖는 `onreadystatechange`, `onload`, `onerror` 같은 이벤트 핸들러 프로퍼티를 이용한다.

HTTP 요청의 현재 상태를 나타내는 `readyState` 프로퍼티 ㄱ밧이 변경된 경우 발생하는 `readystatechange` 이벤트를 캐치하여 다음과 같이 HTTP 응답을 처리할 수 있다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 상태다.
  // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

`load` 이벤트를 캐치할 수도 있다. `load` 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```
