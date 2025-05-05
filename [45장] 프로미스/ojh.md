- 비동기 처리를 위해 기존에 콜백 함수를 사용했지만 → 이는 콜백 지옥으로 인해 가독성이 나쁘고 중간에 발생한 에러 처리가 곤란했다.
- 여러 비동기 처리하는 것도 곤란
- ES6 에서 비동기 처리를 위한 패턴인 프로미스를 도입했다.

# 45.1 비동기 처리를 위한 콜백 패턴의 단점

---

**1️⃣ 콜백 지옥**

- 비동기 함수란, 함수 내부에서 비동기로 동작하는 코드를 포함한 함수이다.
- ❗비동기 함수는 함수 내의 비동기로 동작하는 코드가 완료되지 않았다 하더라도 즉시 종료되어, 비동기 함수 내부의 코드에서 결과를 외부로 반환하거나 할당하는 동작이 제대로 행해지지 않는다.
  ![image.png](attachment:b6c755dd-6bb4-4142-b31f-a93e1accdbfb:image.png)
  ![image.png](attachment:7f8bc81f-8d1c-470d-81ef-a1b889e477e9:image.png)
  ![image.png](attachment:1d042868-b259-4586-a338-5ed3ab8eb019:image.png)
- **🌟 이처럼 비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프 변수에 할당할 수도 없다. → 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 하기 때문에 콜백 함수에 전달하는 것이 일반적이다.**
  ![image.png](attachment:d1598284-aed5-49d0-9266-288e07c199d6:image.png)
- ❗**이처럼 콜 함수를 통해 비동기 처리 결과를 후속 처리해야 하는데, 후속 처리하는 함수가 또 비동기 함수를 호출하는 등 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상을 콜백 지옥이라고 한다.**

![image.png](attachment:c48c032f-f4e9-4387-9138-3222cc5ab182:image.png)

![image.png](attachment:1ecdfe60-91a9-415a-980b-680279f6bfcc:image.png)

**2️⃣ 에러 처리의 한계**

- ⚠️비동기 처리를 위한 콜백 패턴의 가장 큰 문제점은 에러 처리가 곤란하다는 것이다.
  ![image.png](attachment:c5b0e9b8-7045-439b-8f3c-5053bb0bc8e8:image.png)
- 위 코드에서 에러는 캐치되지 못한다.
- setTimeout 은 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 콜 스택에서 제거된다. 이후 타이머가 만료되면 콜백 함수는 태스크 큐로 푸시되고 콜 스택이 비워졌을 때 이벤트 루프로 인해 콜 스택에 푸쉬된다.
- 에러는 호출자 방향으로 전파된다? → 즉, 콜 스택의 아래 방향으로 전파된다. 하지만 앞에서 살펴본 것과 같이 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니다. 그렇기에 catch 되지 않는다.
- **비동기 처리를 위한 콜백 패턴은 콜백 지옥과 에러 처리가 곤란한 문제가 있어서 프로미스가 도입 되었다.**

# 45.2 프로미스의 생성

---

- ES6 에 도입된 호스트 객체가 이닌 **표준 빌트인 객체**이다!
- ❗**Promise 생성자 함수는 비동기 처리를 수행할 콜백함수를 인자로 전달받는다.**
  - **resolve** : 성공했을 때 호출할 함수
  - **reject** : 실패했을 때 호출할 함수
  ![image.png](attachment:e8d47bdb-f168-4a3c-aee1-fa8037d93c0d:image.png)
- 비동기 함수인 PromiseGet 은 함수 내부에서 프로미스를 생성하고 반환한다.
- **만약 비동기 처리가 성공하면 resolve 함수에 인수로 전달하면서 호출하고, 비동기 처리가 실패하면 에러를 reject 함수에 인수로 전달하면서 호출한다.**
- 프로미스는 현재 비동기 처리 상태를 아래와 같이 나타낸다.
  ![image.png](attachment:6fc99a17-bab7-43c7-9bf0-c7b338b2a12c:image.png)
- 기본적으로는 pending 에서 이후 처리 결과에 따라 변경된다.
  - **비동기 처리 성공 :** resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경한다.
  - **비동기 처리 실패 :** reject 함수를 호출해 프로미스를 rejected 상태로 변경한다.
- 이처럼 프로미스의 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정된다.
  ![image.png](attachment:f7d5d95f-e2ee-4225-aaaa-ec839b5007b9:image.png)
- fullfilled 와 rejected 상태를 settled 라고 한다. ❗**결과 상관 없이 비동기 처리가 수행된 상태**
- 한 번 settled 가 되면 다른 상태로 변경 불가.
  ![image.png](attachment:79961cc0-0139-4965-a1dd-42363bef36ee:image.png)
- **프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체이다.**

# 45.3 프로미스의 후속 처리 메서드

---

- 프로미스의 결과를 처리할 수 있는 후속 메서드 then, catch, finally 가 존재한다.
- **❗후속 처리 메서드는 프로미스를 반환하면 비동기로 동작한다. → 이 덕분에 프로미스 체이닝이 가능한 것임.**

### 1️⃣ Promise.prototype.then

- **두 개의 콜백 함수를 인수로 전달받는다. (❗❗ 몰랐음;; 하나만 있는 줄….)**
  - **첫 번째 콜백 함수는 프로미스가 fulfilled 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 받는다.**
  - **두 번재 콜백 함수는 프로미스가 rejected 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.**
- 즉, 첫 번째는 성공했을 때 호출되는 성공 처리 콜백 함수이며, 두 번째 콜백 함수는 비동기 처리가 실패했을 때 호출되는 실패 처리 콜백 함수다.
  ![image.png](attachment:a83bf013-2c37-4d46-bfaf-47e227cbea0e:image.png)
- ❗언제나 프로미스를 반환한다. 만약 then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고, 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject 하여 프로미스를 생성해 반환한다.

### 2️⃣ Promise.prototype.catch

- 한 개의 콜백 함수를 인수로 전달 받고, catch 메서드의 콜백 함수는 프로미스가 rejected 인 경우에만 호출된다.
  ![image.png](attachment:cff4d114-a0fd-4db4-b067-e93520aa9073:image.png)
- 역시나 언제나 프로미스를 반환한다.

### 3️⃣ Promise.prototype.finally

- 한 개의 콜백 함수를 인수로 전달받는다.
- 성공 실패 상관없이 무조건 호출된다.
  ![image.png](attachment:26c73796-1725-4b45-b4c3-001fce533864:image.png)
- 역시나 언제나 프로미스를 반환한다.

# 45.4 프로미스의 에러 처리

---

- 콜백 패턴은 에러 처리가 복잡하다.
- 프로미스는 후속 처리 메서드를 통해 처리할 수 있다.
- then 의 두 번째 콜백 함수, 혹은 catch 를 사용해 처리할 수 있다.
  ![image.png](attachment:7e337e64-c1e7-469a-b767-287893f5e4a9:image.png)
- catch 메서드를 호출하면 내부적으로 then(undefined, onRejected) 를 호출한다.
- ❗ then 메서드의 두 번째 콜백 함수는 **첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고** 코드가 복잡해져서 가독성이 좋지 않다.
- 🌟 **catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면, 비동기 처리 중에 발생한 에러 뿐 아니라, then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.**
- 가독성을 위해 에러 처리를 위해서는 then 대신 catch 를 사용하자.
  ![image.png](attachment:448e514f-a067-4a49-8da1-2d59193599c2:image.png)

# 45.5 프로미스 체이닝

---

- 비동기 처리를 위한 콜백 패턴은 콜백 지옥이 발생한다.
- 프로미스는 then , catch, finally 를 사용하여 콜백 지옥을 해결한다.
  ![image.png](attachment:eeb5695a-f3e3-496f-ae21-a13fd5af55b2:image.png)
- 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출 가능하다. 이를 프로미스 체이닝이라고 한다.
  ![image.png](attachment:eafff506-2e17-4136-b39c-1b0450f1de3e:image.png)
- ❗만약 후속 처리 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하더라도 그 값을 암묵적으로 resolve 또는 reject 처리하여 프로미스를 생성해 반환한다.
- 콜백 지옥이 발생하지만 결국 콜백 패턴을 사용하긴 하는 것이다.
- 🌟**결국 콜백 패턴은 가독성이 좋지 않기 때문에 , ES8 에서 async await 키워드가 도입되어 마치 동기 처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있게 된었다.**
  ![image.png](attachment:e753bc05-afca-42dc-9577-9b0ea29ad0fa:image.png)
- async / await 도 결국 프로미스 기반으로 동작하기 때문에 잘 이해해야 한다.

# 45.6 프로미스의 정적 메서드

---

- Promise 는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있다. 5가지의 정적 메서드를 제공한다.

### 1️⃣ Promise.resolve / Promise.reject

- 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.
- **Promise.resolve 메서드는 인수로 전달받은 값을 resolve 하는 프로미스를 생성한다.**
  ![image.png](attachment:a6e0b0cf-2e43-4e7b-86ac-9fcd7a2902a1:image.png)
- **Promise.reject 메서드는 인수로 전달받은 값을 reject 하는 프로미스를 생성한다.**
  ![image.png](attachment:0c3811f4-e75d-40d6-b7fc-a55e3f7e1200:image.png)

### 2️⃣ Promise.all

- **Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다. → API 호출할 때 종종 사용해봤음.**
  ![image.png](attachment:9d9eca4b-c897-4de7-a573-2adf66c18cdc:image.png)
- 위 호출은 순차적으로 처리하여 총 6초가 걸린다. → 그런데 서로 간섭하는 데 굳이 개별적으로 처리할 필요가 있을까? → 이럴 때 쓰라고 만든게 Promise.all 이다.
  ![image.png](attachment:70ade54a-b096-4da2-8868-4013dc48927b:image.png)
- **Promise.all 메서드는 프로미스를 요소 가지는 배열 등의 이터러블을 인수로 전달받는다. → ❗전달 받은 모든 프로미스가 모두 fullfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다. → 그렇기에 마지막 프로미스의 처리 시간 보다 조금 더 길다.**
- Promise.all 은 결과를 모두 배열에 저장해 새로운 프로미스를 반환하기에 순서를 보장한다.
- ⚠️**만약 하나라도 reject 가 되면 모두 즉시 종료한다.**
  ![image.png](attachment:eb4712b2-6925-4000-92a5-61730645756d:image.png)
- Promise.all 메서드는 인수로 전달받은 이터러블의 요소가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스를 래핑한다.
  ![image.png](attachment:138f2752-7df2-4824-8eb5-9e6fb95d02e1:image.png)

### 3️⃣ Promise.race

- Promise.all 와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
- 모든 프로미스가 fullfilled 상태가 되는 것을 기다리는 것이 아니라 가장 먼저 처리 된 결과를 resolve 하는 새로운 프로미스를 반환한다.
- 하나라도 rejected 가 되면 에러를 reject 하는 새로운 프로미스를 즉시 반환한다.
  ![image.png](attachment:60a488fe-83a6-468a-a114-4d7089d9bf74:image.png)
  ![image.png](attachment:d8741547-616c-4158-a1d7-aba27ac42288:image.png)

### 4️⃣ Promise.allSettled

- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
- ❗**모든 프로미스가 settled 상태가 되면 처리 결과를 배열로 반환한다.**
- ES11 에서 도입된 메서드로 IE 를 제외한 대부분의 모던 브라우저에서 지원한다.
  ![image.png](attachment:79fbc543-bdfa-410b-8abe-8ed3e082a721:image.png)
- 모든 처리 결과가 담겨 있는데, 해당 객체는 아래와 같다.
  ![image.png](attachment:45c9e334-71e3-4281-9a6f-1bef6cbb4142:image.png)
  ![image.png](attachment:73f9a39b-5345-420f-b0d1-16d442a19550:image.png)

# 45.7 마이크로태스크 큐

---

![image.png](attachment:4ae295f2-0c6c-4ead-9c44-a8af1a526558:image.png)

- 1 → 2 → 3으로 동작할 것 같지만 ❗ 실제로는 2 → 3 → 1 순서로 동작한다.
- 프로미스의 후속 처리 콜백 메서드의 콜백 함수는 태스크 큐가 아닌 마이크로 태스크 큐에 저장되기 때문이다.
  - 마이크로 태스크 큐: 프로미스의 후속 처리 메서드의 콜백 함수 일시 저장
  - 태스크 큐 : 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러
- 🌟 **우선순위가 마이크로 태스크 큐가 더 높다!**

# 45.8 fetch

---

- XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API 이다.
- 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.
- 비교적으로 최근에 추가된 Web API 로서 인터넷 익스플로러를 제외한 대부분의 모던 브라우저에서 제공한다.
- fetch 함수에는 HTTP 요청을 전송할 URL 과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달한다.
  ![image.png](attachment:dc466e0d-270c-4cf1-a807-5acc1c09c1ca:image.png)
- **fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.**
- **url 만 인수로 전달하면 get 요청을 전송한다.**
  ![image.png](attachment:dec99200-2bbc-4eab-8e6b-3fd4ec022c5d:image.png)
- Response.prototype 에는 Response 객체에 포함되어 있는 HTTP 응답 몸체를 위한 다양한 메서드를 제공한다.
- Response.prototype.json() 을 메서드를 사용하여 역직렬화 한다. (applicaton/json 일 때)
  ![image.png](attachment:caa19b60-b659-4454-bd75-38a82056bcda:image.png)
- ❗아래 코드는 error 가 출력될 것 같지만 ok 가 출력된다!!
  ![image.png](attachment:c61872d7-043a-4b16-9e86-e580cac6560a:image.png)
- **🌟 fetch 함수가 반환하는 프로미스는 기본적으로 404 NOT Found 500 과 같은 HTTP 에러가 발생해도 에러를 reject 시키지 않고 불리언 타입의 ok 상태를 false 로 설정한 Response 객체를 resolve 한다.**
- **오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject 한다.**
- 따라서 아래와 같이 명시적으로 처리해줘야 한다.
  ![image.png](attachment:31acf00b-6b0f-4b63-a1dc-670a3b9fb2cf:image.png)
- **🌟 참고로 axois 는 모든 HTTP 에러를 reject 하는 프로미스를 반환한다. 따라서 모든 에러를 catch 에서 처리할 수 있다.**
- 인터셉터, 요청 설정 등 더 다양한 기능을 지원하기도 한다.
  ![image.png](attachment:2db96c69-928b-4916-9420-5bbd279ae5eb:image.png)
  - GET 요청
    ![image.png](attachment:0dc11b30-75c1-4358-bf51-7c687f32441f:image.png)
  - POST 요청
    ![image.png](attachment:2d9f304e-443e-43ca-892d-a65bf03b01a6:image.png)
  - PATCH 요청
    ![image.png](attachment:59ad7036-51fd-40aa-921a-14fe4b9a22f4:image.png)
  - DELETE 요청
    ![image.png](attachment:4a468d6e-f1f9-4d87-b674-c99c7af4c2c9:image.png)
- 더욱 자세한 fetch 에 대한 내용은 MDN ‘Using Fetch 페이지’ 참고를 바람.
