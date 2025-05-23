# 43장 Ajax

## 43.1 Ajax란?

- 자바 스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다.

![image.png](43%E1%84%8C%E1%85%A1%E1%86%BC%20Ajax%201e5f78a2bff180978648eb68b154f8b5/image.png)

- 이전의 웹 페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로 전송받아 웹 페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다. 따라서 화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링했다.
- 이런 방식에는 다음과 같은 단점이 있었다.
    - 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
    - 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.
    - 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.
- Ajax의 등장으로 서버로부터 웹 페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹 페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경이 필요한 부분만 한정적으로 렌더링하는 방식이 가능해졌다.

![image.png](43%E1%84%8C%E1%85%A1%E1%86%BC%20Ajax%201e5f78a2bff180978648eb68b154f8b5/image%201.png)

- Ajax는 전통 방식과 비교했을 때 다음과 같은 장점이 있다.
    - 변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
    - 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
    - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## 43.2 JSON

- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바 스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용할 수 있다.
- JSON 표기 방식
    - JSON은 자바 스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.
    - JSON의 키는 반드시 큰따옴표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 문자열은 반드시 큰 따옴표로 묶어야 한다.
- JSON.stringify
    - JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 반환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야하는데 이를 직렬화라 한다.
    - 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.
- JSON.parse
    - JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에 전송된 JSON 데이터는 문자열이다.이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화라고 한다.

## 43.3 XMLHttpRequest

- 브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바 스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.
- XMLHttpRequest 객체 생성
    - XMLHttpRequest 생성자 함수를 호출하여 생성한다. 이 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.
    
    ```jsx
    const xhr = new XMLHttpRequest();
    ```
    
- XMLHttpRequest 객체의 프로퍼티와 메서드
    - XMLHttpRequest 객체의 프로토타입 프로퍼티
    - XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티
    - XMLHttpRequest 객체의 메서드
    - XMLHttpRequest 객체의 정적 프로퍼티
- HTTP 요청 전송
    - [XMLHttpRequest.prototype.open](http://XMLHttpRequest.prototype.open) 메서드로 HTTP 요청을 초기화한다.
    - 필요에 따라 XMLHttpRequest.prototype.setRequsetHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
    - XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.