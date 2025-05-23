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
        - DOM 트리의 최상위에 존재하는 루트 노드로 document 객체를 가리킨다.
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

## 39.6 DOM 조작

- DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.
- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하므로 성능에 영향을 준다.
- innterHTML
    - Element.prototype.innerHTML 프로퍼티는 setter과 getter 모두 존재하는 접근자 프로퍼티로 요소 노드의 HTML 마크텁을 취득하거나 변경한다.
    - 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.
    - 요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.
    - 이때 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하면 크로스 사이트 스크립팅 공격에 취약하므로 위험하다. HTML 마크업 내에 자바 스크립트 악성코드가 포함되어 있다면 파싱 과정에서 그대로 실행될 가능성이 있기 때문이다.
    - HTML5 는 innerHTML 프로퍼티로 삽입된 script 요소 내의 자바 스크립트 코드를 실행하지 않으므로 HTML5를 지원하는 브라우저에서 동작하지는 않는다.
    - innerHTML 프로퍼티의 다른 단점은 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML문자열을 파싱하여 DOM을 변경한다는 것이다.
    - innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하면 유지되어도 좋은 기존의 자식 노드까지 모두 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 DOM에 반영하는데, 이는 효율적이지 않다.
    - 또 다른 단점은 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다는 단점도 있다.
- insertAdjacentHTML 메서드
    - Element.prototype.insertAdjacentHTML 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.
    - 인수로 전달한 HTML 마크업 문자열을 파싱하고 그 결과로 생성된 노드를 첫번째 인수로 전달한 위치에 삽입하여 DOM에 반영한다.
    - 이 메서드는 기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가하므로 기존의 자식 노드를 모두 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 추가하는 innerHTML 프로퍼티보다 효율적이고 빠르다. 허나 크로스 사이트 스크립팅 공격에 취약하다는 점은 동일하다.
- 노드 생성과 추가
    - 요소 노드 생성
        - Document.prototype.createElement 메서드는 요소 노드를 생성해서 반환한다.
        - 이 메서드로 생성한 요소 노드는 기존 DOM에 추가되지 않고 홀로 존재하는 상태로 이후에 생성된 요소 노드를 DOM에 추가하는 처리가 별도로 필요하다.
        - 또한 이 메서드로 생성한 요소 노드는 아무런 자식 노드를 가지고 있지 않다.
    - 텍스트 노드 생성
        - Document.prototype.createTextNode 메서드는 텍스트 노드를 생성하여 반환한다.
        - 이 메서드로 생성한 텍스트 노드는 요소 노드의 자식 노드로 추가되지 않고 홀로 존재하므로 이후 생성된 텍스트 노드를 요소 노드에 추가하는 처리가 별도로 필요하다.
    - 텍스트 노드를 요소 노드의 자식 노드로 추가
        - Node.prototype.appendChild 메서드는 인수로 전달한 노드를 이 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.
    - 요소 노드를 DOM에 추가
        - 새롭게 생성한 요소 노드를 DOM에 추가한다. 요소 노드를 DOM에 추가하면 리플로우와 리페인트가 실행된다.
- 복수의 노드 생성과 추가
    - 여러개의 요소 노드를 생성하여 DOM에 추가할 때 각각 생성해서 DOM에 추가하면 리플로우와 리페인트가 각각 실행되므로 기존 DOM에 요소 노드를 반복하여 추가하는 것은 비효율적이다.
    - 컨테이너 요소를 미리 생성하여 추가해야할 요소 노드를 컨테이너 요소에 자식 노드로 추가하고 컨테이너 요소를 추가하고자 하는 요소에 자식으로 추가한다면 DOM은 한번만 변경된다.
    - 하지만 불필요한 컨테이너 요소가 DOM에 추가되므로 DocumentFragment 노드를 통해 해결할 수 있다. 이 노드는 노드 객체의 일종으로 부모 노드가 없어서 기존 DOM과는 별도로 존재하기에 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용된다.
- 노드 삽입
    - 마지막 노드로 추가
        - Node.prototype.appendChild 메서드는 자신을 호출한 노드의 마지막 자식 노드로 인수로 전달받은 노드를 DOM에 추가한다. 추가할 위치를 지정할 수는 없고 언제나 마지막 자식 노드다.
    - 지정한 위치에 노드 삽입
        - Node.prototype.insertDefuore(newNode, childNode) 메서드는 첫번째 인수로 전달받은 노드를 두번째 인수로 전달받은 노드 앞에 삽입한다.
- 노드 이동
    - DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치로 노드를 이동시킨다.
- 노드 복사
    - Node.prototype.cloneNode 메서드는 노드의 사본을 생성하여 반환한다.
- 노드 교체
    - Node.prototype.replaceChild 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.
- 노드 삭제
    - Node.prototype.removeChild 메서드는 인수로 전달한 노드를 DOM에서 삭제한다.

## 39.7 어트리뷰트

- 어트리뷰트 노드와 attributes 프로퍼티
    - HTML 요소는 여러개의 어트리뷰트를 가질 수 있다.
    - HTML 요소의 HTML어트리뷰트는 시작 태그에 `어트리뷰트 이름 = “어트리뷰트 값”` 형식으로 정의한다.
    - 글로벌 어트리뷰트와 이벤트 핸들러 어트리뷰트는 모든 HTML 요소에서 공통적으로 사용할 수 있지만 특정 HTML 요소에만 사용 가능한 어트리뷰트도 있다.
    - 요소 노드의 모든 어트리뷰트 노드는 요소 노드의 Element.prototype.attributes 프로퍼티로 취득할 수 있다.
- HTML 어트리뷰트 조작
    - 요소 노드의 attributes 프로퍼티는 읽기 전용 접근자 프로퍼티다.
    - Element.prototype.getAttribute/setAttribute 메서드를 사용하면 요소 노드에서 메서들르 통해 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있어서 편리하다.
- HTML 어트리뷰트 vs DOM 프로퍼티
    - 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 DOM프로퍼티가 존재한다. 이들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.
    - DOM 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로 참조와 변경이 가능하다.
    - **HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이며 이는 변하지 않는다.**
    - **요소 노드는 상태를 가지고 있으며 최신 상태와 초기 상태를 관리해야 한다. 요소 노드의 초기 상태는 어트리뷰트 노드가, 최신 상태는 DOM 프로퍼티가 관리한다.**
    - 어트리뷰트 노드
        - HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다.
    - DOM 프로퍼티
        - 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리한다. DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.
    - HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계
        - 대부분은 1:1로 대응되지만  1:1로 대응되지 않거나 어트리뷰트 이름과 프로퍼티 키가 반드시 일치하지 않는 경우도 있다.
    - DOM 프로퍼티 값의 타입
        - gettAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.
    - data 어트리뷰트와 dataset 프로퍼티
        - data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바 스크립트 간의 데이터를 교환할 수 있다.
        - data 어트리뷰트는 data- 접두사 다음에 이름을 붙여서 사용하며 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.
        - dataset프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다. 이로 data 어트리뷰트의 값을 취득하거나 변경할 수 있다.
    

## 39.8 스타일

- 인라인 스타일 조작
    - [HTMLElement.prototype.style](http://HTMLElement.prototype.style) 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로 요소 노드의 인라인 스타일을 취득하거나 추가, 변경한다.
    - style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환하는데 이 객체는 다양한 CSS 프로퍼티에 대응되는 프로퍼티를 가지고 있고 이 프로퍼티에 값을 할당하면 해당 CSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.
- 클래스 조작
    - . 으로 사직하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음 HTML요소의 class 어트리뷰트 값을 변경하여 HTML 요소의 스타일을 변경할 수도 있다.
    - 다만 class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아니라 className과 classList인데 자바 스크립트에서 class는 예약어이기 때문이다.
    - className
        - Element.prototype.className 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경한다.
        - className 프로퍼티를 참조하면 class 어트리부트 값을 문자열로 반환하고, 요소 노드의 className 프로퍼티에 문자열을 할당하면 class 어트리뷰트 값을 할당한 문자열로 변경한다.
    - classList
        - Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
        - DOMTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로 유사 배열 객체면서 이터러블이다.
- 요소에 적용되어 있는 CSS 스타일 참조
    - style 프로퍼티는 인라인 스타일만 반환한다. 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 style 프로퍼티로 참조할 수 없기에 getComputedStyle 메서드를 사용하여 HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조한다.
    - window.getComputedStyle 메서드는 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환한다.
    
    ## 39.9 DOM 표준
    
    - HTML과 DOM 표준은 현재 WHATWG라는 단체가 단일 표준을 내놓기로 하였다.