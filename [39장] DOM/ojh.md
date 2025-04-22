- DOM 은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

# 39.1 노드

---

**1️⃣ HTML 요소와 노드 객체**

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.
  ![image.png](attachment:efab21ff-86dd-4634-9aca-40761584860f:image.png)
- HTML 요소는 렌더링 엔진에 의해 DOM 을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 **어트리뷰트는 어트리뷰트 노드**로, HTML 요소의 **텍스트 콘텐츠는 텍스트 노드**로 변환된다.
  ![image.png](attachment:5de632ef-dec1-4a08-9d87-4774c49dc42f:image.png)
- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, 중첩 관계를 가진다. 텍스트 뿐 아니라 다른 HTML 요소도 포함될 수 있다.
- 계층적인 부자 관계가 형성되며, 이러한 HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성한다.
- 노드 객체들로 구성된 트리 자료구조가 DOM 이다. DOM 트리라고도 한다.

**2️⃣ 노드 객체의 타입**

![image.png](attachment:cfc23527-6484-4003-a665-c1daf3976572:image.png)

- 노드 객체는 총 12 종류의 타입이 있고, 그 중 아래 4가지가 중요하다.

  ### **문서 노드**

  - 루트 노드로서 document 객체를 가르킨다. 브라우저가 렌더링한 HTML 문서 전체를 가르키는 객체로서 전역 객체 🌟 **window 의 document 프로퍼티에 바인딩되어 있다. 따라서 문서 노드는 window.document 또는 document 로 참조할 수 있다.**
  - **❗브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window 를 공유한다.** 따라서 모든 자바스크립트 코드는 전역 객체 window 의 document 프로퍼티에 바인딩되어 있는 하나의 document 객체를 바라본다. HTML 문서당 document 객체는 유일하다.
  - ❗DOM 트리에 접근하기 위한 진입점 역할을 담당한다.

  ### 요소 노드

  - HTML 요소를 가르키는 객체다.
  - 요소 간 중첩에 의해 부자 관계를 가지며, 관계를 통해 정보를 구조화한다.
  - 문서의 구조를 표현

  ### 어트리뷰트 노드

  - HTML 요소의 어트리뷰트를 가르키는 객체다.
  - HTML 요소의 요소 노드와 연결되어 있다.
  - 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.
  - ❗즉 어트리뷰트는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 따라서 접근하기 위해서는 먼저 요소 노드에 접근해야 한다.

  ### 텍스트 노드

  - HTML 요소의 텍스트를 가르키는 객체다.
  - 요소 노드가 문서의 구조를 표현하면, 텍스트 노드는 문서의 정보를 표현한다.
  - 요소 노드의 자식이며, 리프 노드이다.
  - DOM 트리의 최종단이다.
  - ❗즉, 텍스트 노드에 접근하기 위해서는 부모 노드인 요소 노드에 접근해야 한다.

- 외에도 주석을 위한 Comment 노드, DOCTYPE 을 위한 DocumentType 노드, 복수의 노드를 생성하여 추가할 때 사용하는 DocumentFragment 노드 등 총 12개가 있다.

**3️⃣ 노드 객체의 상속 구조**

- DOM API 를 사용하여 노드 객체는 자신의 부모, 형제, 자식을 탐색하고 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.
- DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가되는 호스트 객체이다.
- **❗하지만 그래도 노드 객체도 자바스크립트 객체이기 때문에 프로토타입에 의한 상속 구조를 가진다.**
  ![image.png](attachment:505d9fad-8beb-43e1-85e7-6fd144a03dcc:image.png)
- 노드 객체는 Object ,EventTarget, Node 인터페이스 상속
- 각각 위와 같이 상속 받는다.
- 예를 들어 input 요소를 파싱하여 객체화한 input 요소 노드 객체는 아래의 체이닝을 가진다.
  ![image.png](attachment:b8d147bc-143e-4e47-9da8-1bb2d7f82734:image.png)
  ![image.png](attachment:a35ddb18-c0a0-451c-bdbc-ac3e658fb4bc:image.png)
- input 요소 노드 객체도 다음과 같이 다양한 특성을 갖는 객체이며, 이러한 특성을 나타내는 기능들을 상속을 통해 제공받는다.
  ![image.png](attachment:686947d4-389d-4470-a4b1-7e3920d6007e:image.png)
- 노드 객체의 상속 구조는 개발자 도구의 Elements 패널 우측의 Properties 패널에서 확인할 수 있다.
  ![image.png](attachment:c7df5698-9b14-4c3e-b940-0d62b3817f34:image.png)
- ❗**노드 객체는 공통된 기능일 수록 프로토타입 체인의 상위와 개별적으로 고유한 기능일 수록 하위에 체인을 구축하여 필요한 기능, 프로퍼티와 메서드를 상속하는 구조를 가진다.**
- 프론트엔드 개발자에게 HTML 은 단순히 태그와 어트리뷰트를 선언적으로 배치하여 뷰를 구성하는 것 이상의 의미를 가진다. DOM 과 HTML 을 연관지어 바라봐야 한다.

# 39.2 요소 노드 취득

---

- ❗**HTML 구조나 내용 또는 스타일 등을 동적으로 조작하기 위해서는 먼저 요소 노드를 취득해야 한다.**
- 텍스트 노드는 요소 노드의 자식이고, 어트리뷰트 노드는 요소 노드와 연결되어 있기 때문에 노드나 어트리뷰트를 조작하고자 할 때도 마찬가지다.
- DOM 은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.

**1️⃣ id 를 이용한 요소 노드 취득**

- **Document.prototype.getElementById**
- 인수로 전달할 id 값을 갖는 하나의 노드를 탐색하여 반환
- 반드시 document 를 통해 호출되어야 한다.
- id 값을 유일해야 한다. ← ❗다만 존재하더라도 에러가 발생하는 것은 아니다.
- HTML 문서에는 중복된 id 값을 갖는 여러 요소가 존재할 수 있고, 이러한 경우 첫 번째 노드만 반환한다.
  ![image.png](attachment:ba4cea17-5e82-4557-acb3-02aed628be08:image.png)
- 만약 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 null 을 반환한다.
- ⚠️**HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.**
  ![image.png](attachment:bc9611e3-f0e0-4f1a-8c3c-5797bc68bc2a:image.png)
- 단, id 와 동일한 이름의 전연 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다.
  ![image.png](attachment:7ba1b66d-7825-4967-9f73-d3a4aa0a0faf:image.png)

**2️⃣ 태그 이름을 이용한 요소 노드 취득**

- **Document.prototype/Element.prototype.getElementsByTagName 메서드**
- 인수로 전달한 태그 이름을 가진 모든 요소 노드들을 탐색하여 반환한다.
- 여러 개의 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.
- HTMLCollection 객체는 유사 배열 객체 이면서, 이러터블이다.
  ![image.png](attachment:761c9918-79ad-4cee-b2c7-8aace2771d83:image.png)
- HTML 문서의 모든 요소를 취득하려면 인수로 \*를 전달한다.
  ![image.png](attachment:f1ea650a-6937-4ce7-aa11-20dddcf8dc79:image.png)
- getElementsByTagName 메서드는 Document.prototype 과 Element.prototype 이 존재하는데 document 는 전체 DOM 문서 중에서 탐색하고 element 는 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
  ![image.png](attachment:048f2c04-b889-43a1-a9ea-e3ced89f0f43:image.png)
- 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsBtTagName 메서드는 빈 HTMLCollection 객체를 반환한다.

**3️⃣ class를 이용한 요소 노드 취득**

- **Document.prototype/Element.prototype.getElementsByClassName 메서드**
- 클래스 어트리뷰트 값으로 모든 요소 노드들을 탐색하여 반환한다. ❗ 인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class 를 지정할 수 있다.
- HMLCollection 객체를 반환한다.
  ![image.png](attachment:691588eb-a246-450b-b91c-eef44009f241:image.png)
- getElementsByTagName과 같이 2 종류가 존재하며, DOM 전체 요소 노드 탐색과 특정 요소 노드를 통해 호출하여 탐색하는 2가지가 존재한다.
  ![image.png](attachment:a0c71d1f-3dca-4717-9e34-a34a696526bf:image.png)
- 만약 인수로 전달된 class 값을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체를 반환한다.

**4️⃣ CSS 선택자를 이용한 요소 노드 취득**

- CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용되는 문법.
  ![image.png](attachment:51ee8151-3b2c-4f13-8016-c5f56b65b78c:image.png)
- **Document.prototype/Element.prototype.querySelector 메서드**
- 인수로 전달한 CSS 선택자를 만족시키는 **하나의 요소 노드**를 탐색하여 반환한다.
  - 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환
  - 존재하지 않는 경우 null 반환
  - CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.
  ![image.png](attachment:c5f485f5-0bf3-478b-9e7c-77fac66295b3:image.png)
- **Document.prototype/Element.prototype.querySelectorAll 메서드**
- 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다.
- ❗반환 값은 NodeList 객체이다. 역시 유사배열 객체이면서 이터러블이다.
  - 존재하지 않는 경우 빈 NodeList 객체 반환
  - CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.
  ![image.png](attachment:914a5cc1-86ae-410f-8efa-e6309c38354c:image.png)
- 모든 요소 노드를 취득하려면 querySelectorAll 의 인수를 \* 로 전달한다.
- 역시 2 종류가 존재한다. DOM 전체 요소 노드 탐색과 특정 요소 노드를 통해 호출하여 탐색하는 2가지가 존재한다.
- **❗❗ CSS 선택자 문법을 사용하는 메서드는 getElementById, getElementsBy\*** 메서드 보다 다소 느린 것으로 알려져 있다. → 하지만 좀 더 구체적인 조건으로 요소 노드를 취득하고 일관된 방식으로 취득할 수 있다는 장점이 있다.\*\*
- **🌟 따라서 id 어트리뷰트가 있는 요소 노드를 취득하는 경우에는 getElementById 를 그 외에는 querySelector 나 querySelectorAll 메서드를 사용하는 것을 권장한다.**

**5️⃣ 특정 요소 노드를 취득할 수 있는지 확인**

- **Element.prototype.matches 메서드**
- 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.
  ![image.png](attachment:051c0396-eb98-44d0-9302-eb63cdb17d4a:image.png)
- 이벤트 위임을 사용할 때 유용하다?

**6️⃣ HTMLCollection 과 NodeList**

- 둘 다 DOM API 가 결과 값을 반환하기 위한 DOM 컬렉션 객체이다. 둘 다 이터러블이며 유사 배열 객체이다. for … of , … 등 가능
- **🌟 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체라는 것이다. → 언제나 live 객체로 동작한다.**
- ❗단, NodeList 는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작한다….?ㄴ

### HTMLCollection

- getElementsBy … 메서드로 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체이다.
- 살아있는 객체.
  ![image.png](attachment:f4e9fb10-d2a3-4494-a593-c20f530b6cb8:image.png)
  - 제대로 모든 요소가 blue 로 변하지 않았다…
  - 실제로 확인해보면 두 번째 요소만 class 값이 변경되지 않는다.
  ![image.png](attachment:ba435fe7-619e-4ed8-80ac-ea374fc457df:image.png)
  - 저렇게 되는 이유는 바로바로 변경이 반영되어 첫 번째 요소가 바뀌는 순간 길이가 2로 줄어 두 번째 요소가 아닌 세 번째 요소를 변경시키기 때문.
- 위와 같이 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 for 문으로 순회하면서 상태를 변경할 때 주의해야 한다.
  - for 문을 역방향으로 순회하거나
  - while 문을 사용하여 남지 않을 때까지 반복하거나
  - … 연산자로 배열로 만들어 forEach 를 통해 순회하면서 변경하면 된다.

### NodeList

- querySelector, querySelectorAll 을 통해 반환되는 객체로
- 실시간으로 노드 객체의 상태 변경을 반영하지 않는다.
  ![image.png](attachment:7c6f7099-04e8-44f2-bdf2-4ef0907c2eef:image.png)
  - NodeList.prototype.forEach 는 Array.prototype.forEach 와 사용 방법이 동일하다. 외에도 item, entries, keys, values 메서드를 제공한다.
- NodeList 객체는 대부분의 경우 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작한다. 하지만 childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 반영되는 live 객체로 동작하니 주의가 필요하다.
  ![image.png](attachment:3369568a-620a-4981-9ba0-4ec9653ad51b:image.png)
- 이처럼 HTMLCollection 이나 NodeList 객체는 예상과 다르게 동작할 때가 있어 다루기 까다롭고 실수하기가 쉽다. → **🌟 따라서 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection 이나 NodeList 를 사용하려면 객체를 배열로 변환하여 사용하는 것을 권장한다.**
- 변환하면 배열 고차 함수도 사용할 수 있다. … 나 Array.from 이던 쉽게 바꿀 수 있다.
  ![image.png](attachment:ae476929-afdb-451f-962d-90e7b419b166:image.png)

# 39.3 노드 탐색

---

- 요소 노드를 취득한 이후 , 취득한 노드 기점으로 DOM 트리의 노드를 옮겨 다니며, 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있다.
  ![image.png](attachment:c5745466-ba65-4432-bbcd-0339c9bdd85a:image.png)
- 위에서 먼저 ul#fruits 요소 노드를 취득한 후, 자식 노드를 모두 탐색하거나 자식 노드 중 하나만 탐색할 수 있다.
- 혹은 li.banana 가 요소 노드를 취득한 후, 형제나 부모 노드를 탐색할 수 있다.
- 이처럼 DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.
  ![image.png](attachment:b41266ec-af5a-4444-8e97-c59eccf556b4:image.png)
- **❗parentNode, previousSibling, firstChild, childNodes 프로퍼티는 Node.prototype 이 제공하고,**
- **❗ 프로퍼티 키에 Element 가 포함된 previousElementSibling, nextElementSibling 과 children 프로퍼티는 Element.prototype 이 제공한다.**
- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 읽기 전용 접근자 프로퍼티이기 때문에, 값을 할당하면 아무 에러없이 무시된다.
  ![image.png](attachment:b976206d-9c9c-4ac5-8b2c-1d7c1c93832c:image.png)

**1️⃣ 공백 텍스트 노드**

- HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다. →
  **공백 텍스트 노드**
      ![image.png](attachment:c45c8371-61c7-490b-996e-953fa1e6bab8:image.png)

      ![image.png](attachment:14e99b3f-3172-4fb0-8a83-8d42dadcdb9b:image.png)
- 위와 같이 공백을 포함한 DOM 트리가 생성된다. → ❗**따라서 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 한다.**
- 아래와 같이 하면 공백 텍스트 노드가 생성은 안되겠다만 가독성이 좋지 않으므로 X
  ![image.png](attachment:0c617e41-6b91-4a6d-8a8d-c93eb2496abf:image.png)

**2️⃣ 자식 노드 탐색**

- 자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.
  ![image.png](attachment:10819f21-33eb-461d-9ab3-e664ff2cb7be:image.png)
  ![image.png](attachment:ffd3f539-f9a5-4405-874a-81f04aecab93:image.png)

**3️⃣ 자식 노드 존재 확인**

- Node.prototype.hasChildNodes 메서드를 사용한다.
- 자식 노드가 존재하면 true 없으면 false 반환.
- ❗단, 텍스트 노드도 포함하여 자식 노드 존재 여부를 확인한다.
  ![image.png](attachment:d7afa325-be88-4064-bac8-36b3762c5b76:image.png)
- 그렇기에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 hasChildNodes 메서드 대신 children.length 또는 Element 인터페이스의 childElementCount 프로퍼티를 사용한다.
  ![image.png](attachment:d713c04e-2b79-4aee-991c-191297091dcb:image.png)

**4️⃣ 요소 노드의 텍스트 노드 탐색**

- 요소 노드의 텍스트 노드는 요소 노드의 자식 노드다.
- 따라서 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
- firstChild 프로퍼티는 첫 번째 자식 노드를 반환한다.
- 텍스트 노드 혹은 요소 노드이다.
  ![image.png](attachment:ea732e3d-81a2-4b7f-99e3-e588a0a82e61:image.png)

**5️⃣ 부모 노드 탐색**

- Node.prototype.parentNode 프로퍼티를 사용한다.
- 텍스트 노드는 DOM 트리의 리프 노드이기 때문에 텍스트 노드가 부모인 경우는 없다.
  ![image.png](attachment:9cf907d6-099e-4801-ba29-ff557b0aafbf:image.png)

**6️⃣ 형제 노드 탐색**

- 형제 노드를 탐색하기 위해서는 아래와 같은 프로퍼티를 사용해야 한다.
- 어트리뷰트 노드는 그냥 연결만 되어 있는 거니 형제 노드 아님.
- 텍스트 노드나 요소 노드를 반환한다.
  ![image.png](attachment:bc9e2803-4a52-47a4-bbc5-871e1777a957:image.png)
  ![image.png](attachment:124e6690-3f08-43ca-81d2-5cd17234af0d:image.png)

# 39.4 노드 정보 취득

---

- 노드 객체에 대한 정보를 취득하려면 아래와 같은 노드 프로퍼티를 사용한다.
  ![image.png](attachment:7f1e2276-f48e-4974-897b-0d01ccccdb33:image.png)
  ![image.png](attachment:58f28b3c-d000-41f5-9d15-0bb8f90aa879:image.png)

# 39.5 요소 노드의 텍스트 조작

---

**1️⃣ nodeValue**

- 지금까지는 노드 탐색, 노드 정보 등 모두 읽기 전용 접근자 프로퍼티였다.
- **Node.prototype.nodeValue 프로퍼티는 setter 와 getter 가 모두 존재하는 접근자 프로퍼티다.**
- 참조 할당 모두 가능.
- nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. → 텍스트 노드의 텍스트 → 따라서 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null 을 반환한다.
  ![image.png](attachment:99fdd0f6-6b84-496f-90ef-8893ba3349f9:image.png)
- 텍스트 노드가 아닌 객체는 null 을 반환하므로 의미가 없다.
- 따라서 요소 노드의 텍스트를 변경하려면 다음과 같은 순서의 처리가 필요하다.
  - 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드를 탐색한다. 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색한다.
  - 탐색한 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.
  ![image.png](attachment:5ab8c232-f1b3-403d-b532-aca43af5b3d4:image.png)

**2️⃣ textContent**

- Node.protototype.textContent 프로퍼티는 setter 와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.
- ❗요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환한다.
- 시작 태그와 종료 태그 사이 내의 텍스트를 모두 반환한다.
  ![image.png](attachment:9a494a92-62b7-4069-bb84-812c89f30251:image.png)
- nodeValue 프로퍼티도 텍스트를 취득할 수 있지만, 코드가 더 복잡해진다.
  ![image.png](attachment:d5d17a49-8257-4557-ba9a-fd188ed00d57:image.png)
- 만약 요소 노드의 콘텐츠 영역에 자식 요소 노드가 없고 텍스트만 존재한다면 firstChild.nodeValue 와 textContent 는 같은 결과를 반환한다.
  ![image.png](attachment:c5bf251d-113c-4ff9-9e7f-6fc7a0105bce:image.png)
- 요소 노드의 textConent 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다. **❗이때 할당한 문자열에 HTML 마크업이 있더라도 문자열 그대로 인식되어 텍스트 취급된다. 즉 파싱되지 않는다.**
  ![image.png](attachment:1f601ed7-eb51-429f-85cc-101d917ee7a1:image.png)
- 유사한 동작을 하는 innerText 프로퍼티가 있는데 아래와 같은 이유로 사용하지 않는 것이 좋다.
  - innerText 프로퍼티는 CSS 에 순종적이다. 예를 들어, innerText 프로퍼티는 **CSS 에 의해 비표시로 지정된 요소 노드의 텍스트를 반환하지 않는다.** (ex. visibility:hidden)
  - innerText 프로퍼티는 CSS 를 고려해야 하므로 textContent 프로퍼티보다 느리다.
