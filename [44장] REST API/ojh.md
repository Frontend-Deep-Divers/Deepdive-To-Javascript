- HTTP 를 제대로 사용하지 못하는 모습을 보고 최대한 활용할 수 있는 아키텍처로 소개함.
- REST 기본 원칙을 지킨 디자인을 RESTful 하다고 함.
- 🌟 즉, REST 는 HTTP 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API 는 REST 를 기반으로 서비스 API 를 구현한 것이다.

# 44.1 REST API 의 구성

---

- 자원, 행위, 표현
- REST 는 자체 표현 구조로 구성되어 있어, REST API 만으로 HTTP 요청의 내용을 이해할 수 있다.
  ![image.png](attachment:c36fc5a6-0cc2-40fe-b0e4-b47d13af8e3f:image.png)

# 44.2 REST API 설계 원칙

---

- 가장 중요한 2가지 원칙

### 1. URI 는 리소스를 표현해야 한다.

- URI 는 리소르를 표현하는 데 중점을 두어야 한다. 리소스를 식별할 수 있는 이름은 동사보다도 명사를 사용한다. → get 같은 거 X
  ![image.png](attachment:abb0343f-69b2-421d-999b-cb090eef4b68:image.png)

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다.
- 5가지 요청 메서드를 사용하여 CRUD 를 구현한다.
  ![image.png](attachment:91faad9a-fcc0-4801-94a9-bfcca86bce1a:image.png)

# 44.3 JSON Server 를 이용한 REST API 실습

---

- npm 으로JSON Server 를 설치하고 활용할 수 있다.
