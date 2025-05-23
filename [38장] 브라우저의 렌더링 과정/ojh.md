- V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경인 Node.JS 의 등장으로 웹 브라우저를 벗어나 서버 사이드 애플리케이션 개발에서도 사용할 수 있는 범용 개발 언어가 되었다만… → 역시나 가장 많이 사용되는 분야는 역시 웹 브라우저 환경에서 동작하는 웹 페이지/애플리케이션 클라이언트 사이드다.
- 대부분의 프로그래밍 언어는 VM 혹은 OS 위에서 실행되지만 웹 애플리케이션의 클라이언트 사이드 자바스크립트는 브라우저에서 HTML,CSS 와 함께 실행된다. → 즉, 브라우저 환경을 고려할 수록 효율적인 클라이언트 사이드 자바스크립트 프로그래밍이 가능하다.
  ![image.png](attachment:f9b8bcf0-e38f-488f-92a0-a6d0f3a608c2:image.png)
  ![image.png](attachment:a295c0ac-c9bd-45b4-be36-fb01be487b35:image.png)

# 38.1 요청과 응답

---

- 브라우저의 핵심 기능은 필요한 리소스를 서버에 요청하고 서버로부터 응답을 받아 브라우저에 시각적으로 렌더링하는 것이다. ← 리소스는 모두 서버에 존재하므로 필요한 리소스를 서버에 요청하고 서버가 응답한 리소스를 파싱하여 렌더링
- **❗서버에 요청을 전송하기 위해! 브라우저는 주소창을 제공한다.** 브라우저의 주소창에 URL 을 입력하고 엔터키를 누르면 DNS → IP 주소 → 서버 요청.
  ![image.png](attachment:1ddfb0b1-1cf5-4830-b81b-e61cb8c786b5:image.png)
- 🌟서버는 루트 요청에 대해 암묵적으로 index.html 을 응답하도록 기본 설정되어 있음.
- 만약 다른 정적 파일을 서버에 요청하려면 파일의 경로와 파일 이름을 URI 의 호스트 뒤에 path 를 기술하여 요청한다.
- ⚠️ 반드시 브라우저의 주소창을 통해 서버에게 정적 파일만을 요청할 수 있는 것은 아니다. 자바스크립트를 통해 동적으로 서버에 정적/동적 데이터를 요청할 수 있다.
- Network 패널에서 확인 가능. - 이미 응답 받은 경우 또 안뜸.
  ![image.png](attachment:415c05b2-62b2-488b-8417-9cfbd5da5724:image.png)
- ⭐ index.html 을 제외하고도 다른 파일들이 응답된 것이 보이는데, 이는 HTML 을 파싱하는 도중 외부 리소스를 로드하는 태그, Link 태그, img 태그, script 태그 등을 만나면 HTML 파싱을 일시 중단하고 해당 리소스를 서버에 요청하기 때문이다!

# 38.2 HTTP 1.1 과 HTTP 2.0

---

- HTTP 프로토콜은 웹에서 브라우저와 서버가 통신하기 위한 프로토콜.
- HTTP 1.1 과 2.0 의 차이점을 알아보자!

![image.png](attachment:1ce69c61-337a-4f40-b380-1047eeb03da4:image.png)

![image.png](attachment:f6ac43aa-ecb0-497d-a587-062e2d278893:image.png)

- HTTP/1.1 은 기본적으로 커넥션당 하나의 요청과 응답만을 처리한다. 즉, 여러 개의 요청을 한 번에 전송하고 응답을 받을 수 없다. → 즉, CSS 파일을 로드하는 Link 태그, 이미지 파일을 로드하는 img 태그, 자바스크립트를 로드하는 script 태그 등에 의한 리소스 요청이 개별적으로 전송되고 응답 또한 개별적으로 전송된다.
- 그렇기에 동시 전송이 불가능한 구조이기 때문에 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있다!
- 이에 반해 HTTP/2는 커넥션 당 여러 개의 요청과 응답이 가능하다. 그렇기에 페이지 로드 속도가 50% 정도 더 빠르다!!

### HTTP/2 는 뭐가 그렇게 달라요?

## ✅ 먼저, HTTP/2가 **어떻게 여러 요청을 하나의 커넥션으로 처리할 수 있는가?**

### 📌 핵심 기술: **Multiplexing (멀티플렉싱)**

> 📌 하나의 TCP 커넥션 안에서 여러 요청과 응답을 동시에 섞어서 보낼 수 있음

HTTP/1.1에서는 요청 1번 → 응답 1번 → 그다음 요청... 이렇게 직렬로 처리됨

근데 HTTP/2는?

```
plaintext
복사편집
[TCP 커넥션 1개]
요청1 ─┐
요청2 ─┤ → 중간에 쪼개져 섞여서 전송됨
요청3 ─┘
      ↓
응답3 ─┐
응답2 ─┤ → 응답도 순서 상관 없이 동시에 돌아옴
응답1 ─┘

```

➡ 이게 되니까 **CSS, JS, 이미지 요청을 전부 동시에 처리**할 수 있고

➡ **페이지 로딩 속도도 급격히 빨라지는** 거야!

---

## ✅ 그럼 HTTP/2는 뭐가 더 달라졌냐면?

| 항목                  | HTTP/1.1                            | HTTP/2                                                |
| --------------------- | ----------------------------------- | ----------------------------------------------------- |
| **요청 처리 방식**    | 직렬                                | 병렬 (멀티플렉싱)                                     |
| **헤더 압축**         | 없음 (모두 텍스트로 전송)           | 있음 (HPACK 사용)                                     |
| **커넥션 수**         | 동시 요청 위해 여러 TCP 커넥션 필요 | 하나의 TCP 커넥션으로 충분                            |
| **서버 푸시**         | 없음                                | 있음 (서버가 클라이언트 요청 전에 리소스를 미리 보냄) |
| **우선순위**          | 없음                                | 있음 (중요한 리소스 먼저 보낼 수 있음)                |
| **보안 연결 (HTTPS)** | 선택 사항                           | 대부분 HTTPS로 사용됨 (브라우저 정책상 강제)          |

---

## 🔧 조금 더 기술적으로:

### 1. 헤더 압축 (HPACK)

- HTTP/1.1에서는 모든 요청에 큰 헤더가 그대로 붙음 (예: `Cookie`, `User-Agent`)
- HTTP/2에서는 이전에 보낸 헤더는 **테이블에 저장하고, 차이만 전송**

➡ 대역폭 절약 + 속도 향상

---

### 2. 서버 푸시

- 클라이언트가 `index.html`을 요청하면
- 서버가 알아서 필요한 `main.js`, `style.css` 등을 미리 같이 보내줌

➡ 페이지 로드 더 빠름

(_단, 실제로는 일부 브라우저에서 지원 중단됨_)

---

### 3. 스트림과 프레임

- HTTP/2는 모든 요청/응답을 **프레임** 단위로 쪼갠 다음, **스트림 ID**로 식별함
- 그래서 중간에 프레임이 섞여도 다시 조립 가능

```
plaintext
복사편집
[스트림 #1] req1-part1 → req1-part2
[스트림 #2] req2-part1 → req2-part2

```

➡ 실제 데이터가 **동시에 왔다갔다** 할 수 있는 핵심 구조

---

## 🚀 그래서 체감 성능이 얼마나 빨라졌냐면?

- 구글 실험 기준:
  **HTTP/2가 HTTP/1.1보다 페이지 로드 속도 최대 50% 향상**
- 특히 리소스가 많은 웹사이트, 모바일 환경에서는 차이가 더 큼

---

## 🧠 정리

| 기능        | HTTP/1.1     | HTTP/2        | 개선 효과               |
| ----------- | ------------ | ------------- | ----------------------- |
| 요청 병렬성 | ❌ 직렬 요청 | ✅ 멀티플렉싱 | 속도 ↑, 커넥션 ↓        |
| 헤더 최적화 | ❌ 없음      | ✅ HPACK 압축 | 대역폭 ↓                |
| 서버 푸시   | ❌ 없음      | ✅ 있음       | 요청 전에 응답 가능     |
| 우선순위    | ❌ 없음      | ✅ 있음       | 중요한 리소스 먼저 받음 |

# 38.3 HTML 파싱과 DOM 생성

---

- 브라우저 요청에 의해 서버가 응답한 HTML 문서는 문자열로 이루어진 순수한 텍스트임. → 렌더링 하기 위해서는 브라우저가 이해할 수 있는 자료구조로 변환하여 메모리에 저장해야 한다.
- 브라우저의 렌더링 엔진은 아래 그림과 같은 과정을 통해 응답받은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 DOM 객체로 만든다.
  ![image.png](attachment:fd4495d8-7618-4513-8eb3-d69106309a9c:image.png)
  ![image.png](attachment:29feb093-5c1a-4ce4-af83-1e900bc475de:image.png)

# 38.4 CSS 파싱과 CSSOM 생성

---

- 렌더링 엔진은 HTML 을 처음부터 파싱하다가 link 태그나 style 태그를 만들면 DOM 생성을 일시 중단하고 →
- CSS 파일을 서버에 요청하여 로드한 CSS 파일이나 style 태그 내의 CSS 를 HTML 과 동일한 파싱 과정을 거치며 CSSOM 을 생성한다. →
- 이후 다시 HTML 파싱해서 DOM 생서 재개한다.
- link 태그의 href 어트리뷰트에 지정된 CSS 파일을 서버에 요청한다.
- 서버로부터 CSS 파일이 응답되면 HTML 과 동일한 파싱 과정 (바이트 → 문자 → 토큰 → 노드 → CSSOM) 거처 CSSOM 을 생성한다.
- 🌟 CSSOM 은 CSS 의 상속을 반영하여 생성된다.
  ![image.png](attachment:15d0665f-2bd6-41c1-a482-9e68d318fef2:image.png)

# 38.5 렌더 트리 생성

---

- HTML 과 CSS 를 파싱하여 DOM 과 CSSOM 을 생성한다. 그리고 렌더링을 위해 렌더 트리로 결합된다.
- ❗렌더 트리는 렌더링을 위한 트리로 브라우저 화면에 렌더링 되지 않는 meta, script 태그나 display : none 과 같은 노드는 포함되지 않는다.
  ![image.png](attachment:dc4e0640-adf5-4249-870d-400c7d692ab2:image.png)
- 이후 완성된 렌더 트리는 레이아웃을 계산하는 데 사용되고 이후 렌더링하는 페인팅 처리에 입력된다.
- 브라우저의 렌더링 작업은 반복적으로 이루어질 수 있다! 아래의 경우 레이아웃 계산과 페인팅이 재차 발생한다.
  - 자바스크립트에 의한 노드 추가 또는 삭제
  - 브라우저 창의 리사이징에 의한 뷰포트 크기 변경
  - HTML 요소의 레이아웃에 변경을 발생시키는 width/height, margin, padding, border, display, position, top/left/bottom/right 등의 스타일 변경
- ❗레이아웃 계산과 페인팅을 다시 실행하는 리렌더링은 비용이 많이 드는 성능에 악영향을 주는 작업이다. 가급적 리렌더링이 빈번하게 발생하지 않도록 주의할 필요가 있다.

# 38.6 자바스크립트 파싱과 실행

---

- HTML 문서를 파싱한 결과물인 DOM 은 문서의 구조와 정보 스타일을 변경할 수 있는 프로그래밍 인터페이스로 **DOM API 를** 제공한다.
- 즉, 자바스크립트 코드로 DOM API 를 사용하면 이미 생성된 DOM 을 동적으로 조작할 수 있다.
- 역시나 DOM 생성 중 sciprt 태그를 만나면 일시 중단하고 자바스크립트 코드를 파싱하기 위해 **자바스크립트 엔진에 제어권을 넘긴다.**
- 이후 자바스크립트 파싱과 실행이 종료되면 다시 제어권을 넘겨 DOM 생성을 재개한다.
- 즉 렌더링 엔진 : DOM , CSSOM, 자바스크립트 엔진: javascript
- 렌더링 엔진이 저런 것들을 생성하는 것처럼 자바스크립트 엔진도 AST (추상 구문 트리) 를 생성한다. 이를 토대로 바이트 코드를 생성하여 실행한다.
  ![image.png](attachment:189967bd-43ef-4373-96c0-38591ab7187c:image.png)
  - 토크 나이징: 문법적 의미를 갖는 최소 단위인 토큰들로 분해한다.
  - 파싱: 토큰들을 구문 분석하여 AST (추상 구문 트리)를 생성한다.
    - 🌟 AST 는 인터프리터나 컴파일러 외에도 AST 를 사용하면 typescript, babel, Prettier 같은 트랜스파일러를 구현할 수도 있다.
  - 바이트 코드 생성과 실행: 파싱의 결과물로 생성된 AST 인터프리터가 실행할 수 있는 중간 코드인 바이트 코드로 변환되고 인터프리터에 의해 실행된다.

# 38.7 리플로우와 리페인트

---

- DOM API 가 사용된 경우 DOM 이나 CSSOM 이 변경된다. 이때 다시 렌더 트리로 결합되고 변경된 렌더 트리를 기반으로 레이아웃과 페인트 과정을 거쳐 브라우저 화면에 다시 렌더링 된다. 이것이 리플로우, 리페인트
  ![image.png](attachment:c82401a3-cc40-46a8-8c22-f7e5d36e28d6:image.png)
- 리플로우는 레이아웃 계산을 다시 하는 것 → 노드 추가/삭제, 요소의 크기 위치 변경, 윈도우 리사이징 등 레이아웃에 영향을 주는 변경이 발생한 경우 실행.
- 리페인트는 재결합된 렌터 트리 기반으로 다시 페인트
- 즉 리페인트만 실행될 수 있음.

# 38.8 자바스크립트 파싱에 의한 HTML 파싱 중단

---

![image.png](attachment:95d76d60-f6d9-48cd-97ee-4437ba4fcf73:image.png)

- 저렇게 순자척으로 파싱하고 실행되기 때문에 script 위치에 따라 HTML 파싱이 블로킹되어 DOM 생성이 지연될 수 있다는 것이다. → ❗따라서 script 태그의 위치는 중요하다!
  ![image.png](attachment:365be889-74a0-4a5e-b9e2-6ce59234f63e:image.png)
- DOM API 가 id 가 apple 인 HTML 요소를 취득하고 싶었지만 아직 포함되어 있지 않은 상태이다. 따라서 제대로 동작하지 않는다. → 이런 문제를 회피하기 위해 자바스크립트를 가장 아래에 위치시키는 것이 좋은 아이디어다.
  - DOM 완성되지 않는 상태에서 자바스크립트가 DOM 을 조작하면 에러가 발생할 수 있다.
  - 자바스크립트 로딩/파싱/실행으로 인해 HTML 요소들의 렌더링에 지장받는 일이 발생하지 않아 페이지 로딩 시간이 단축된다.
  ![image.png](attachment:ac051b1a-d06e-4421-a0f4-7bdcc6939d24:image.png)
- 저렇게 해야 정상 작동한다.
- 에러가 발생할 우려도 페이지 로딩 시간도 단축된다.

# 38.9 script 태크의 async / defer 어트리뷰트

---

- 앞서 살펴본 자바스크립트 파싱에 의한 DOM 생성의 중단되는 문제를 근본적으로 해결하기 위해 HTML5 부터 script 태그에 async 와 defer 어트리뷰트가 추가되었다!
- 🌟 async 와 defer 어트리뷰트는 둘 다 외부 자바스크립트 파일을 로드하는 경우에만 사용할 수 있다. 즉 ,src 가 없는 인라인 자바스크립트에는 사용할 수 없다.
- 둘다 IE 10 이상에서 지원, defer 는 6~9 도 지원은 한다만 정상적으로 동작하지 않을수도 있음.
  ![image.png](attachment:d192a346-9500-4950-9062-c2e3c656fee5:image.png)
- HTML 파싱과 자바스크립트 파일 로드가 비동기적으로 동시에 진행되는데, 자바스크립트 실행 시점에 차이가 있다.

### async 어트리뷰트

- HTML 파싱과 자바스크립트 로드가 동시에 실행되고 로드가 완료된 직후 자바스크립트 파싱과 실행이 진행된다. 이때 HTML 파싱은 중단된다.
  ![image.png](attachment:064043c5-9c1f-4a28-bded-2d5a00f10929:image.png)
- 여러 개의 script 태그에 async 어트리뷰트를 지정하면 script 태그 순서와 상관없이 **❗ 먼저 로드된 자바스크립트 부터 실행된다. (순서 보장 X) → 따라서 순서 보장이 중요한 경우에는 사용하면 안됨.**

### defer 어트리뷰트 (연기하다, 지연되다)

- 역시나 비동기적으로 진행된다.
- 단, 자바스크립트 실행은 HTML 파싱이 완료되고 DOM 생성이 완료된 직후에 진행된다.
- 따라서 DOM 생성 이후 실행될 자바스크립트에 유용하다.
