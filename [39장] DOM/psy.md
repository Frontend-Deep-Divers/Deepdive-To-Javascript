DOM Document Object Model 은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API를 제공하는 트리 자료구조

## 노드

### HTML 요소와 노드 객체

HTML 요소는 HTML 문서를 구성하는 개별적인 요소

HTML 요소는 파싱되어 요소 노드로 변환되고, 어트리뷰트는 어트리뷰트 노드로, 텍스트 콘텐츠는 텍스트 노드로 변환됨

HTML 요소 간에는 계층이 존재하기 때문에 노드도 트리 구조로 구성됨

### 노드 객체의 타입

- 문서 노드
    - DOM 트리 최상위에 위치한 루트 노드 → document 객체
    - 전역 객체 window의 document 프로퍼티에 바인딩됨
    - 브라우저 환경에서는 하나의 전역 객체를 공유하므로 하나의 document 객체를 가짐
    - DOM 트리의 노드에 접근하기 위한 엔트리 포인트 역할을 함
- 요소 노드
    - HTML 요소를 가리키는 객체
- 어트리뷰트 노드
    - HTML 요소의 어트리뷰트를 가리키는 객체
    - 요소 노드와만 관계를 가짐
- 텍스트 노드
    - HTML 요소의 텍스트 콘텐츠를 가리키는 객체
    - 요소 노드의 자식 노드이며, 리프 노드
    - 공백 텍스트 노드도 존재함
- Comment 노드, DocumentType 노드, DocumentFragment 노드 등

### 노드 객체의 상속 구조

노드 객체는 DOM API를 제공하고, 이를 통해 부모, 형제, 자식을 탐색할 수 있으며, 어트리뷰트와 텍스트를 조작할 수도 있음

노드 객체는 브라우저 환경에서 제공하는 호스트 객체 ( 표준 빌트인 객체가 아님 )

노드 객체도 자바스크립트 객체이기 때문에 프로토타입에 의한 상속 구조를 가짐

## 요소 노드 취득

### id를 이용한 요소 노드 취득

```jsx
document.getElementById('banana');
```

HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있음

### 태그 이름을 이용한 요소 노드 취득

```jsx
document.getElementsByTagName('li');
// DOM 전체에서 탐색
Document.getElementsByTagName('li');
```

HTMLCollection 객체를 반환하는데 이는 이터러블 + 유사 배열 객체이다

### class를 이용한 요소 노드 취득

```jsx
document.getElementsByClassName('fruit');
// DOM 전체에서 탐색
Document.getElementsByClassName('fruit');
```

### CSS 선택자를 이용한 요소 노드 취득

```jsx
document.querySelector('.banana');
// DOM 전체에서 탐색
Document.querySelector('.banana');

document.querySelectorAll('.banana');
```

querySelectorAll()은 NodeList 객체를 반환하는데 이는 이터러블 + 유사 배열 객체이다

### 특정 요소 노드를 취득할 수 있는지 확인

```jsx
document.matches('.apple');
```

### HTMLCollection과 NodeList

노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체

단, NodeList는 대부분의 경우에는 정적 상태를 유지함

- HTMLCollection
    - 살아 있는 객체기 때문에 for 문으로 순회하며 노드 객체의 상태를 변경해야 할 때 주의해야 함
        - 역방향 순회 또는 while 문으로 순회하여 해결 가능
        - 배열로 변환하여 사용 가능
- NodeList
    - childNodes 프로퍼티가 반환하는 NodeList 객체만 살아 있는 객체이므로 주의가 필요
    - 역시 배열로 변환해서 사용할 것을 권장

## 노드 탐색

노드 탐색 프로퍼티는 모두 getter만 존재하는 접근자 프로퍼티

### 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성함

### 자식 노드 탐색

```jsx
Node.prototype.childNodes // NodeList로 반환, 텍스트 노드도 포함 가능
Element.prototype.children // HTMLCollection으로 반환, 요소 노드만 반환
Node.prototype.firstChild/lastChild
Element.prototype.firstElementChild/lastElementChild
```

### 자식 노드 존재 확인

```jsx
Node.prototype.hasChildNodes()
Element.prototype.childElementCount()
```

### 요소 노드의 텍스트 노드 탐색

텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티로 접근 가능

### 부모 노드 탐색

```jsx
Node.prototype.parentNode // 텍스트 노드인 경우는 없음
```

### 형제 노드 탐색

```jsx
Node.prototype.previousSibling/nextSibling
Element.prototype.previousElementSibling/nextElementSibling
```

## 노드 정보 취득

```jsx
Node.prototype.nodeType
// 1 = ELEMENT_NODE
// 3 = TEXT_NODE
// 9 = DOCUMENT_NODE

Node.prototype.nodeName
// 요소 노드라면 태그 이름 반환
// 텍스트 노드라면 #text 반환
// 문서 노드라면 #document 반환
```

## 요소 노드의 텍스트 조작

### nodeValue

텍스트 노드의 nodeValue 프로퍼티를 수정하면 텍스트 콘텐츠가 수정됨

요소 노드의 firstChild를 탐색한 후 사용

### textContent

요소 노드의 텍스트와 모든 자손 노드의 텍스트를 취득하거나 변경하는 접근자 프로퍼티

### innerText

textContent 프로퍼티와 유사하지만 CSS에 의해 비표시되는 요소 노드의 텍스트는 반환하지 않음

CSS를 고려하므로 textContent 보다 느림
