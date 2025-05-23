- 파싱: 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 실행하기 위해 텍스트 문서의 문자열을 토큰으로 분해하고, 토큰에 문법적 의미와 구조를 반영하여 파스 트리를 생성하는 과정
- 렌더링: HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것

## 요청과 응답

브라우저가 필요한 리소스를 서버에 요청하고 응답받음

## HTTP 1.1과 HTTP 2.0

HTTP는 웹에서 클라이언트와 서버가 통신하기 위한 프로토콜

## HTML 파싱과 DOM 생성

HTML 문서는 문자열로 이루어진 순수한 텍스트 → 브라우저가 이해할 수 있는 자료구조, DOM으로 변환

## CSS 파싱과 CSSOM 생성

DOM을 생성하다가 CSS를 로드하는 link 태그나 style 태그를 만나면 일시 중단

CSS 파일을 파싱하여 CSSOM 생성

## 렌더 트리 생성

DOM과 CSSOM을 결합하여 렌더 트리 생성

브라우저 화면에 렌더링되지 않는 노드는 포함되지 않음 ex) display: none

이후 레이아웃, 페인팅, 컴포지트 과정을 거침

## 자바스크립트 파싱과 실행

HTML 파싱을 통해 생성된 DOM은 DOM API를 제공함

DOM을 생성하다가 script 태그를 만나면 일시 중단하고 자바스크립트 코드 파싱( 자바스크립트 엔진이 담당 )

파싱된 자바스크립트 코드는 AST 추상적 구문 트리를 생성함

AST를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하고 실행함

## 리플로우와 리페인트

DOM이나 CSSOM을 변경하는 DOM API가 사용된 경우 다시 렌더링 해야함

이 때 리플로우와 리페인트가 발생하며, 레이아웃 변경점이 없다면 리페인트만 일어남

## 자바스크립트 파싱에 의한 HTML 파싱 중단

각 파싱은 직렬적으로, 동기적으로 수행됨

따라서 script 태그의 위치가 중요함

## script 태그의 async/defer 어트리뷰트

자바스크립트 파싱에 의해 DOM 생성이 중단되는 문제를 해결하기 위해 도입됨

외부 자바스크립트 파일을 로드하는 경우에만 사용 가능

### async

HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 진행

자바스크립트 파싱과 실행은 동기적으로 진행

여러 async script가 존재하면 로드가 된 순서대로 파싱이 됨 → 순서 보장 X

### defer

외부 자바스크립트 파일의 로드는 비동기적으로 진행되지만, DOM 생성이 완료된 이후에 자바스크립트 파싱과 실행이 일어남
