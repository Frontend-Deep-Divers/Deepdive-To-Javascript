- **Asynchronous Javascript and XML**
- 🌟 **Ajax란, 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹 페이지를 동적으로 갱신하는 프로그래밍을 의미한다.**
- Ajax 는 브라우저가 제공하는 웹 API 인 XMLHttpRequest 객체를 기반으로 동작한다. HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.
- 뭐 2005년 구글 맵스를 통해 개발 언어로서 자바스크립트의 가능성을 확인.
- 이전의 웹 페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML 을 서버로부터 전송받아 웹 페이지를 처음부터 다시 렌더링하는 방식으로 동작했다. → 화면 전환되면 새로운 HTML 전송받아 다시 렌더링.
  ![image.png](attachment:a72ff031-39f5-4993-8014-17691c0fce24:image.png)
- 이러한 전통적인 방식의 단점 (SSR, MPA)
  - 이전 웹 페이지와 차이가 없어 변경할 필요가 없는 부분까지 포함된 완전한 HTML 을 서버로부터 매번 다시 전송받아 불필요한 데이터 통신이 발생한다.
  - 변경할 필요가 없는 부분까지 처음부터 다시 렌더링하여, 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.
  - 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.
- Ajax 의 등장으로 서버로부터 웹 페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹 페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해졌다.
  ![image.png](attachment:20b12099-0456-445e-a1b8-243974834911:image.png)
- 다음과 같은 장점이 있다.
  - 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송 받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
  - 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 순간적으로 깜박이는 현상이 발생하지 않는다.
  - 클라이언트와 서버의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

# 43.2 JSON

---

- JSON ( Javascript Object Notation ) 은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.
- 자바스크립트에 종속되지 않은 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

**1️⃣ JSON 표기 방식**

- 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.
- 반드시 “ “ 큰 따옴표로 묶어야 한다. 문자열.
  ![image.png](attachment:f1685c89-49c1-499a-a8cb-149dfdfeee6f:image.png)

**2️⃣ JSON.stringify**

- 객체를 JSON 포맷 문자열로 변환한다.
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야 하는데 이를 직렬화라고 한다.
  ![image.png](attachment:04ee7593-2130-433f-a222-76cf38cecd29:image.png)
  ![image.png](attachment:f30dc204-33ba-444b-a7ee-362b04f38002:image.png)

3️⃣ **JSON.parse**

- JSON 포맷의 문자열을 객체로 변환한다.
- 서버에서 클라이언트에게 전송된 JSON 데이터는 문자열이고, 이를 활용하기 위해 객체화 하는 것을 역직렬화라고 한다.
  ![image.png](attachment:28db2cc2-f55d-442c-9824-08684ea02bb6:image.png)

# 43.3 XMLHttpRequest

---

- 브라우저는 주소창이나 HTML 의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다.
- **form 과 a 태그**
  ### 🔹 브라우저 기본 HTTP 요청 기능 요약
  | 태그          | 동작 방식                                      | HTTP 메서드       | 비고                                        |
  | ------------- | ---------------------------------------------- | ----------------- | ------------------------------------------- |
  | `<a href="">` | 링크 클릭 시 지정된 URL로 이동                 | `GET`             | 주로 페이지 이동용                          |
  | `<form>`      | 버튼 클릭(또는 `submit`) 시 form 데이터를 전송 | `GET` 또는 `POST` | action, method, input 필드에 따라 동작 결정 |
- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.
- Web API 인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

**1️⃣ XMLHttpRequest 객체 생성**

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다. 브라우저에서 제공하는 Web API 이므로 브라우저 환경에서만 정상적으로 실행된다.
  ![image.png](attachment:1d7bf33f-56d7-4271-ba21-c19557f36539:image.png)

**2️⃣ XMLHttpRequest 객체의 프로퍼티와 메서드**

- XMLHttpRequest 객체는 다양한 프로퍼티와 메서드를 제공한다.
- **프로토타입 프로퍼티**
  ![image.png](attachment:9392cb26-cbb2-41c7-bce2-07e750672a5f:image.png)
- **이벤트 핸들러 프로퍼티**
  ![image.png](attachment:1fa5cb5e-15fe-4744-9722-b778e6333f9c:image.png)
- **메서드**
  ![image.png](attachment:668be034-3b98-4322-a15c-e0b468a98a80:image.png)
- **정적 프로퍼티**
  ![image.png](attachment:9950a6f9-00f6-406d-af32-31fbcfc68e0f:image.png)

**3️⃣ HTTP 요청 전송**

![image.png](attachment:3200c9b2-a70e-4d03-951f-20989cc307f5:image.png)

### XMLHttpRequest.prototype.open

- HTTP 요청을 초기화 한다.
  ![image.png](attachment:31018f2d-8072-4cfb-8a9a-68aae8266395:image.png)
- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법.
  ![image.png](attachment:dac93e1b-d6b5-482f-b200-e6ae40d8cbd9:image.png)

### XMLHttpRequest.prototype.send

- open 으로 초기화된 HTTP 요청을 서버에 전송한다.
  ![image.png](attachment:f33d46e6-619e-40ce-a13c-9123f56aab9b:image.png)
- send 메서드는 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다. → ❗ **만약 객체를 전달하는 경우 반드시 JSON.stringify 메서드로 직렬화해야 한다.**
- 요청 메서드가 GET 인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null 로 설정된다.

### XMLHttpRequest.prototype.setRequestHeader

- 특정 HTTP 요청의 헤더 값을 설정한다.
- 반드시 open 이후에 호출해야 한다.
- 🌟 **HTTP 요청 헤더인 Content-type 과 Accept**
  ### [Content-type]
  - 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.
  - **MIME (Multipurpose Internet Mail Extensions) : 인터넷에서 전송되는 다양한 데이터를 식별하는 형식**
    ![image.png](attachment:2127914d-851c-4036-97fe-ee87c9d54df8:image.png)
  - 예시 코드
    ![image.png](attachment:07471b17-ae8e-4a33-acc2-7f4c8926f7c3:image.png)
    ![image.png](attachment:07471b17-ae8e-4a33-acc2-7f4c8926f7c3:image.png)
  ### [Accept]
  - 클라이언트가 서버에 요청할 때, 서버가 응답할 데이터의 MIME 타입을 Accept 로 지정할 수 있다.
  - 설정하지 않으면 send 메서드 호출될 때 Accept 헤더가 _ / _ 으로 전송된다.
    ![image.png](attachment:91675dac-7c05-4cbf-9f7e-9a9e4d9550c7:image.png)

**4️⃣ HTTP 응답 처리**

- 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.
- onreadstatechange, onload, onerror 같은 이벤트 핸들러를 프로퍼티로 갖는다.
- readState 프로퍼티 값이 변경된 경우 발생하는 이벤트를 onreadstatechange 로 캐치하여 처리할 수 있다.
  ![image.png](attachment:de2a7540-ea37-4c48-9c86-adc79b2e9037:image.png)
- send 메서드를 통해 HTTP 요청을 서버에 전송하면 서버는 응답을 반환한다. 하지만 언제 응답이 클라이언트에 도달할지는 알 수 없기에 요청의 현재 상태를 계속 확인해야 한다.
- readstatechange 이벤트는 readstate 프로퍼티가 변경될 때마다 발생한다.
- ❗이 대신 load 이벤트를 캐치해도 된다. → load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다. → 즉 , 따로 readstate 가 무엇인지 확인할 필요가 없다…
  ![image.png](attachment:5716d145-4f16-42c7-912f-38d9c416b58d:image.png)
