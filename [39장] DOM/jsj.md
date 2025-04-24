# 39장 DOM

- **DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API (프로퍼티와 메서드)를 제공하는 트리 자료구조다.**

## 39.1 노드

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.

![image.png](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%201def78a2bff180318ba4f885de4d142d/image.png)

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.
- HTML 문서는 HTML 요소들의 집합으로 이뤄지며 HTML 요소는 중첩 관계를 갖는다. 이때 이 요소 간에는 중첩 관계에 의해 계층적인 부자관계가 형성되며 이를 반영하여 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성한다.
- **노드 객체들로 구성된 트리 자료구조를 DOM이라 한다.** 노드 객체의 트리로 구조화되어 있기 때문에 DOM을 DOM 트리라고 부르기도 한다.
- 노드 객체는 12개가 있으며 이 중 중요한 노드 타입은 4가지이다.
    - 문서 노드
        - DOM 트리의 최상위에 존재하는 루트 노드로 documnet 객체를 가리킨다.
        - document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩 되어 있다.
        - 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다. HTML 문서당 document 객체는 유일하다.
        - document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 한다.
    - 요소 노드
        - HTML 요소를 가리키는 객체로 HTML 요소 간의 중첩에 의해 부자 관계를 가지며 이 부자관계를 통해 정보를 구조화한다.
    - 어트리뷰트 노드
        - HTML 요소의 어트리뷰트를 가리키는 객체다.
        - 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있으며 부모 노드가 아닌 요소 노드에만 연결되어 있다.
        - 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 요소노드에 접근해야 한다.
    - 텍스트 노드
        - HTML 요소의 텍스트를 가리키는 객체다.
        - 문서의 정보를 표현하며 요소 노드의 자식노드로 자식 노드를 가질수 없는 리프노드다.
- **DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.**

## 39.2 요소 노드 취득

- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 한다. 텍스트 노드, 어트리뷰트 노드를 조작하고자 할때도 마찬가지다
- id를 이용한 요소 노드 취득
    - Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다.
    - id 값은 HTML 문서 내에서 유일한 값이어야 하며 class 어트리뷰트와 달리 공백 문자로 구분하여 여러개의 값을 가질 수 없다.
    - 중복된 id 값을 갖는 요소가 여러개 존재할 경우 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환한다.
    - 해당 id 값을 갖는 HTML 요소가 존재하지 않는 경우 null을 반환한다.
    - HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다.
- 태그 이름을 이용한 요소 노드 취득
    - Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다. DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.
    - Document.prototype의 경우 DOM의 루트 노드인 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.
    - Element.prototype의 경우 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
- class를 이용한 요소 노드 취득
    - Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다. DOM 콜렉션 객체인 HTMLCollection 객체를 반환한다.
    - Document.prototype의 경우 DOM의 루트 노드인 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.
    - Element.prototype의 경우 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
- CSS 선택자를 이용한 요소 노드 취득
    - CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법
    - Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
    - Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다.
- 특정 요소 노드를 취득할 수 있는지 확인
    - Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.
- HTMLCollection과 NodeList
    - DOM API가 여러개의 결과 값을 반환하기 위한 DOM 컬렉션 객체다. 유사 배열 객체이며 이터러블이다.
    - 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 객체다.
    (NodeList는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있다.)
    - HTMLCollection
        - 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체다.
        - 정방향 for문으로 상태를 변경하는 경우 live 객체이기에 변경된 객체가 바로 컬렉션에서 빠져서 원하는대로 index가 작동하지 않을 수 있다. 이 경우 역방향 for문이나 while문으로 대체하여 오류를 회피하자.
        - HTMLCollection 객체를 배열로 변환하는것도 방법이다.
    - NodeList
        - querySelectorAll 메서드를 사용하여 위의 HTMLCollection 객체의 부작용을 해결할 수도 있다. NodeList 객체는 상태 변경을 반영하지 않는 객체다.
        - forEach 메서드를 상속받아 상요할 수 있다.
        - **childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 갹체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.**
- **노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection 이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다.**

## 39.3 노드 탐색

![image.png](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%201def78a2bff180318ba4f885de4d142d/image%201.png)

- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단 노드 탐색 프로퍼티는 읽기 전용 접근자 프로퍼티다.
- 공백 텍스트 노드
    - HTML 요소 사이의 공백 문자는 텍스트 노드를 생성하며 이를 공백 텍스트 노드라 한다.
- 자식 노드 탐색
    - 자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.
    
    | 프로퍼티 | 설명 |
    | --- | --- |
    | Node.prototype.childNodes |  |
    | Element.prototype.children |  |
    | Node.prototype.firstChild |  |
    | Node.prototype.lastChild |  |
    | Element.prototype.firstElementChild |  |
    | Element.prototype.lastElementChild |  |
- 자식 노드 존재 확인
    - Node.prototype.hasChildNodes 메서드를 사용한다. 자식 노드가 존재하는지를 불리언 값으로 반환한다.
- 요소 노드의 텍스트 노드 탐색
    - firstChild 프로퍼티로 접근할 수 있다. 첫번째 자식 노드를 반환한다.
    (요소 노드의 텍스트 노드는 요소 노드의 자식 노드이기 때문)
- 부모 노드 탐색
    - Node.prototype.parentNode 프로퍼티를 사용한다.

## 39.4 노드 정보 취득

![image.png](39%E1%84%8C%E1%85%A1%E1%86%BC%20DOM%201def78a2bff180318ba4f885de4d142d/image%202.png)

## 39.5 요소 노드의 텍스트 조작

- nodeValue
    - 이전의 노드 탐색, 노드 정보 프로퍼티들은 모두 읽기 전용 접근자 프로퍼티이다.
    - Node.prototype.nodeValue
        - setter와 getter 모두 존재하는 접근자 프로퍼티며 참조와 할당 모두 가능하다.
        - 노드 객체의 값을 반환한다. (텍스트 노드의 텍스트)
        - 텍스트 노드의 nodeValue 프로퍼티 외에는 null을 반환한다.
        - 텍스트 변경시
            - 텍스트를 변경할 요소 노드를 취득한 다음 취득한 요소 노드의 텍스트 노드를 탐색한다.
            (firstChild 프로퍼티 사용)
            - 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.
- textContent
    - setter와 getter모두 존재하는 접근자 프로퍼티로 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.