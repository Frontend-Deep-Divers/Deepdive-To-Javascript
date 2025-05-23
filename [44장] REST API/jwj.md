# 44장 REST API

REST(Representational State Transfer)는 HTTP/1.0과 1.1의 스펙 작성에 참여, 아파치 HTTP 서버 프로젝트의 공동 설립자인 로이 필딩(Roy Fielding)의 2000년 논문에서 처음 소개되었다. HTTP의 장점을 최대한 활용할 수 있는 아키텍처로서 소개되었고 HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하고 있다. REST 원칙을 잘 지킨 서비스 디자인을 "Restful"하다고 한다.

**REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.**

## 44.1 REST API의 구성

REST API는 자원, 행위, 표현의 3가지 요소로 구성된다. REST는 자체 표현 구조(self-descriptiveness)로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

| 구성 요소                | 내용                           | 표현 방법          |
| ------------------------ | ------------------------------ | ------------------ |
| 자원 (_resource_)        | 자원                           | URI(엔드포인트)    |
| 행위 (_verb_)            | 자원에 대한 행위               | HTTP 요청 메서드   |
| 표현 (_representations_) | 자원에 대한 행위의 구체적 내용 | 페이로드 (Payload) |

## 44.2 REST API 설계 원칙

1. URI는 리소스를 표현해야 한다.

   URI는 리소스를 표현하는 데 중점을 두어야 한다. 리소스 식별자는 동사보다는 명사를 사용한다.

2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

   HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다. 주로 5가지 요청 메서드를 사용하여 CRUD를 구현한다. 리소스에 대한 행위는 URI에 표현하지 않으며 리소스를 취득하는 경우 `GET`, 삭제하는 경우 `DELETE`를 사용하여 리소스에 대한 행위를 명확히 표현한다.

   | HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
   | ---------------- | -------------- | --------------------- | -------- |
   | GET              | index/retrieve | 모든/특정 리소스 취득 | ✕        |
   | POST             | create         | 리소스 생성           | ⭕       |
   | PUT              | replace        | 리소스의 전체 교체    | ⭕       |
   | PATCH            | modify         | 리소스의 일부 수정    | ⭕       |
   | DELETE           | delete         | 모든/특정 리소스 삭제 | ✕        |
