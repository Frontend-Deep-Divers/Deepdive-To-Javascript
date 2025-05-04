## 콜백 패턴의 단점

- 콜백 헬: 비동기 처리 결과를 외부에 반환할 수 없고, 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 함
- 에러 처리의 한계: catch 블록으로 잡을 수 없음

## Promise

비동기 처리 상태와 처리 결과를 관리하는 객체

- new Promise() 호출을 함과 동시에 Executor 함수가 실행됨
    - Promise 객체가 현재 실행 컨텍스트에 기록됨
    - Executor 함수 실행 컨텍스트가 만들어지고 실행
    - 콜백 함수는 무조건 마이크로태스크 큐로 들어감 ( 바로 resolve() 를 해도 )
- then() catch() finally() 모두 Promise를 반환함 → thenPromise 라고 가정
    - 반환된 Promise의 onFulfilled, onRejected 에 콜백 함수가 등록됨
    - 콜백 함수가 Promise를 반환하면 ~~그 Promise 객체를 그대로 반환하고~~ 그 Promise 객체의 상태에 따라 thenPromise의 상태가 결정됨
    - 다른 값을 반환하면 암묵적으로 그 값을 resolve() 또는 reject()하는 새 Promise를 생성되고 이 Promise의 상태에 따라 thenPromise의 상태가 결정됨

ThenPromise는 InitialPromise와 InnerPromise의 상태를 따름

```jsx
const InitialPromise = new Promise(res => res(1));

InitialPromise.then(() => { // ThenPromise
	return new Promise(...) // InnerPromise
})
```

then(undefined)가 반환하는 thenPromise는 initialPromise의 상태를 그대로 따르는 새 Promise임

- resolve() reject() 메소드는 Promise 객체의 상태를 수정함과 동시에 콜백 함수를 마이크로태스크 큐에 넣어주는 내부 구현을 가짐
    - Promise 객체는 자바스크립트 엔진에 의해 특별 관리됨 → 더 이상 쓸모없는 Promise 객체는 GC 대상이 됨
    - 체이닝된 Promise는 여러 콜백함수를 마이크로태스크 큐에 넣게 됨
- Promise.all() 은 Promise 이터러블을 받아서 모든 Promise가 resolve가 된 경우 결과를 순서에 맞춰 배열에 저장한 다음 새 Promise를 반환함. 하나라도 reject되면 reject됨
    - Promise 객체가 이터러블에 포함되어 있지 않다면, 해당 원소를 resolve하는 Promise를 생성해 진행함
- Promise.race() 는 역시 하나라도 reject면 reject지만, 모두 fulfilled 상태가 되면 그 중 가장 빨리 resolve된 값을 resolve 함

fetch 는 Promise를 반환함

404, 500 등의 HTTP 에러를 reject 하지 않음

요청이 완료되지 않은 경우만 reject함

axios 는 HTTP 에러에도 reject함
