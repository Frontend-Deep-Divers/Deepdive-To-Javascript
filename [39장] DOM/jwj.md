# 39장 DOM

DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

## 39.1 노드

### HTML 요소와 노드 객체

HTML 요소는 HTML 문서를 구성하는 개별 요소다.

```html
<div class="greeting">Hello</div>
```

렌더링 엔진이 이를 파싱하여 DOM을 구성하는 요소 노드 객체로 변환한다. 이때 HTML 요소의 속성은 속성 노드로, 텍스트 콘텐츠는 텍스트 노드로 변환된다.

HTML 요소의 콘텐츠 영역에는 다른 HTML 요소도 포함될 수 있다.

### 노드 객체의 타입

노드 객체는 종류가 있고 상속 구조를 갖는다. 노드 객체는 총 12개의 종류(노드 타입)가 있다. 이 중에서 중요한 4개의 노드 타입이 있다.

- 문서 노드

문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 `document` 객체를 가리킨다. `document` 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리는 객체로 전역 객체 `window`의 `document` 프로퍼티에 바인딩되어 있다. 문서 노드는 `window.document` 또는 `document`로 참조할 수 있다.

브라우저 환경의 모든 자바스크립트 코드는 `script` 태그에 의해 분리되어 있어도 하나의 전역 객체 `window`를 공유한다. 때문에 모든 자바스크립트 코드는 `document` 프로퍼티에 바인딩된 하나의 `document` 객체를 바라본다. 즉, HTML 문서당 하나의 유일한 `document` 객체가 존재한다.

때문에 문서 노드는 DOM 트리의 엔트리 포인트 역할을 한다.

- 요소 노드

HTML 요소를 가리키는 객체다. 요소 노드는 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다. 요소 노드는 문서의 구조를 표현한다고 할 수 있다.

- 속성 노드

HTML 요소의 속성을 가리키는 객체다. HTML 요소의 요소 노드와 연결되어 있다. 요소 노드에만 연결되어 있다. 속성 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 속성 노드에 접근해 속성을 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다.

- 텍스트 노드

HTML 요소의 텍스트를 가리키는 객체다. 텍스트 노드는 문서의 정보를 표현한다고 할 수 있다. 텍스트 노드는 요소 노드의 자식 노드이며, 동시에 자식을 가질 수 없는 리프 노드다. 즉, 텍스트 노드는 DOM 트리의 최종단이다. 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 한다.

이 밖에도 `Comment` 노드, `DOCTYPE`을 위한 `DocumentType` 노드, 복수의 노드를 생성하여 추가할 때 사용하는 `DocumentFragment` 노드 등이 있다.

### 노드 객체의 상속 구조

DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 이를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 속성과 텍스트를 조작할 수 있다.

DOM을 구성하는 노드 객체는 브라우저 환경에서 제공하는 호스트 객체다.

모든 노드 객체는 `Object`, `EventTarget`, `Node` 인터페이스를 상속받는다. 추가적으로 문서 노드는 `Document`, `HTMLDocument` 인터페이스를 상속받고 속성 노드는 `Attr`, 텍스트 노드는 `CharacterData` 인터페이스를 각각 상속받는다.

요소 노드는 `Element` 인터페이스를 상속받고, 추가적으로 `HTMLElement`와 태그의 종류별로 세분화된 `HTMLHtmlElement`, `HTMLHeadElement` 등의 인터페이스를 상속받는다.

`input` 요소를 파싱하여 객체화한 `input` 요소 노드 객체는 `HTMLInputElement`, `HTMLElement`, `Element`, `Node`, `EventTarget`, `Object`의 `prototype`에 바인딩되어 있는 프로토타입 객체를 상속받는다. 프로토타입 체인상의 모든 프로퍼티나 메서드를 사용할 수 있다.

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이벤트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

노드 객체의 상속 구조는 개발자 도구의 `Elements` 패널 우측의 `Properties` 패널에서 확인할 수 있다.

노드 객체에는 노드 타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능, 고유한 기능이 있다.

모든 노드 객체는 공통으로 이벤트를 발생시킬 수 있는데, 이벤트에 관련된 기능(`EventTarget.addEventListner`, `EventTarget.removeEventListener` 등)은 **`EventTarget` 인터페이스가 제공**한다. 또한 모든 노드 객체는 트리의 노드로서 공통으로 트리 탐색 기능(`Node.parentNode`, `Node.childNodes`, `Node.previousSibling`, `Node.nextSibling`)이나 노드 정보 제공 기능(`Node.nodeType`, `Node.nodeName`)이 필요하다. 이 기능은 **`Node` 인터페이스가 제공**한다.

HTML 요소가 객체화된 요소 노드 객체는 HTML 요소가 갖는 공통 기능이 있다. `input`, `div` 요소 노드 객체는 모두 스타일을 나타내는 `style` 프로퍼티가 있다. **HTML 요소가 갖는 공통 기능은 `HTMLElement` 인터페이스가 제공**한다.

하지만 요소 노드 객체는 HTML 요소의 종류의 종류에 따라 고유한 기능도 있다. 예를 들어, `input` 요소 노드 객체는 `value` 프로퍼티가 필요하지만 `div` 요소 노드 객체는 `value` 프로퍼티가 필요하지 않다. 때문에 필요 기능을 제공하는 인터페이스(`HTMLInputElement`, `HTMLDivElement` 등)가 HTML 요소의 종류에 따라 각각 다르다.

DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일을 동적으로 조작할 수 있다.

## 39.2 요소 노드 취득

HTML의 구조나 내용 또는 스타일을 조작하기 위해서는 먼저 요소 노드를 취득해야 한다.

### `id`를 이용한 요소 노드 취득

**`Document.prototype.getElementById`** 메서드는 인수로 전달한 `id` 속성 값을 갖는 하나의 요소 노드를 탐색하여 반환한다. `getElementById` 메서드는 `Document.prototype`의 프로퍼티다. 따라서 반드시 문서 노드인 `document`를 통해 호출해야 한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 'banana'인 요소 노드를 탐색하여 반환한다.
      // 두 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById("banana");

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = "red";
    </script>
  </body>
</html>
```

`id` 값은 HTML 문서 내에서 유일한 값이어야 하며, `class` 속성과는 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없다. 단, 중복된 `id` 값을 갖는 HTML 요소가 여러 개 존재해도 에러는 발생하지 않는다.

이러한 경우 `getElementById` 메서드는 인수로 전달된 `id` 값을 갖는 첫 번째 요소 노드만 반환한다.

인수로 전달된 `id` 값을 갖는 HTML 요소가 존재하지 않는 경우 `null`을 반환한다.

HTML 요소에 `id` 속성을 부여하면 `id` 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.

단, `id` 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수가 노드 객체가 재할당되지 않는다.

### 태그 이름을 이용한 요소 노드 취득

**`Document.prototype`/`Element.prototype.getElementsByTagName`** 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색해 반환한다.

여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `HTMLCollection` 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      // 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elems = document.getElementsByTagName("li");

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
      [...$elems].forEach((elem) => {
        elem.style.color = "red";
      });
    </script>
  </body>
</html>
```

`HTMLCollection` 객체는 유사 배열 객체이면서 이터러블이다.

HTML 문서의 모든 요소 노드를 취득하려면 인수로 `*`를 전달한다.

`getElementsByTagName` 메서드는 `Document`와 `Element` 프로토타입에 정의된 메서드가 있다. 전자는 `document`를 통해 호출해 DOM 전체에서 요소 노드를 탐색해 반환한다. 하지만 후자는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색해 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      const $lisFromDocument = document.getElementsByTagName("li");
      console.log($lisFromDocument); // HTMLCollection(4) [li, li, li, li]

      // #fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두
      // 탐색하여 반환한다.
      const $fruits = document.getElementById("fruits");
      const $lisFromFruits = $fruits.getElementsByTagName("li");
      console.log($lisFromFruits); // HTMLCollection(3) [li, li, li]
    </script>
  </body>
</html>
```

인수로 전달된 태그 이름을 갖는 요소가 없으면 빈 `HTMLCollection` 객체를 반환한다.

### `class`를 이용한 요소 노드 취득

`Document.prototype`/`Element.prototype.getElementsByClassName` 메서드는 인수로 전달한 `class` 속성 값을 갖는 모든 요소 노드들을 탐색해 반환한다. 마찬가지로 `HTMLCollection` 객체를 반환한다.

`getElementsByTagName` 메서드와 마찬가지로 두 개의 프로토타입에 정의된 메서드가 있다.

인수로 전달된 태그 이름을 갖는 요소가 없으면 빈 `HTMLCollection` 객체를 반환한다.

### CSS 선택자를 이용한 요소 노드 취득

CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

```css
/* 전체 선택자: 모든 요소를 선택 */
* {
  ...;
}
/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p {
  ...;
}
/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo {
  ...;
}
/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo {
  ...;
}
/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type="text"] {
  ...;
}
/* 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p {
  ...;
}
/* 자식 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p {
  ...;
}
/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul {
  ...;
}
/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul {
  ...;
}
/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover {
  ...;
}
/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택
   일반적으로 content 프로퍼티와 함께 사용된다. */
p::before {
  ...;
}
```

**`Document.prototype`/`Element.prototype.querySelector`** 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색해 반환한다.

- 여러 개인 경우 첫 번째를 반환
- 존재하지 않으면 `null`을 반환
- 문법에 맞지 않는 경우 `DOMException` 에러 발생

**`Document.prototype`/`Element.prototype.querySelectorAll`** 메서드는 만족시키는 모든 요소 노드를 탐색해 반환한다. 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `NodeList` 객체를 반환한다. `NodeList`는 유사 배열 객체이면서 이터러블이다.

- 존재하지 않으면 빈 `NodeList` 객체를 반환
- 문법에 맞지 않는 경우 `DOMException` 에러 발생

HTML 문서의 모든 요소 노드를 취득하려면 인수로 `*`를 전달한다.

`querySelector(All)` 메서드는 `getElementBy***` 메서드보다 다소 느린 것으로 알려져 있다. 그렇지만 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있다.

💡따라서 `id` 값이 있는 요소는 `get~Id` 메서드로, 그 외의 경우에는 `querySelector(All)` 메서드를 사용하는 것을 권장한다.

### 특정 요소 노드를 취득할 수 있는지 확인

**`Element.prototype.matches`** 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.

해당 메서드는 이벤트 위임을 사용할 때 유용하다. 추후 다시 살펴보자.

### HTMLCollection과 NodeList

DOM 컬렉션 객체인 `HTMLCollection`과 `NodeList`는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체다.

모두 유사 배열 객체이면서 이터러블이다. 따라서 `for...of` 문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단히 배열로 변환할 수 있다.

두 객체의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는(live) 객체라는 것이다.

`HTMLCollection`은 언제나 live 객체다. `NodeList`는 대부분의 경우 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있다.

- `HTMLCollection`

노드 객체의 상태 변화를 실시간으로 하기 때문에, 살아 있는(live) 객체라고 부르기도 한다.

⚠live 객체이므로 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 `HTMLCollection` 객체를 `for` 문으로 순회하면서 노드 객체를 상태 변경할 때 주의해야 한다.

이 문제는 역방향 순회로 회피하거나 또는 `while` 문을 사용하여 회피할 수 있다.

더 간단하게는 `HTMLCollection` 객체를 사용하지 않는 것이다. 유사 배열 객체이면서 이터러블인 `HTMLCollection` 객체를 배열로 변환하면 부작용을 발생시키기 때문에 배열의 고차 함수를 사용할 수 있다.

```js
// 유사 배열 객체이면서 이터러블인 HTMLCollection을 배열로 변환하여 순회
[...$elems].forEach((elem) => (elem.className = "blue"));
```

- `NodeList`

`HTMLCollection` 객체의 부작용을 해결하기 위해 `querySelectorAll` 메서드를 사용할 수도 있다. `NodeList`는 non-live 객체다.

`NodeList`는 `NodeList.prototype.forEach` 메서드를 상속받아 사용할 수 있다. `Array`의 그것과 사용방법이 동일하다. 이외에도 `item`, `entries`, `keys`, `values` 메서드를 제공한다.

⚠`childNodes` 프로퍼티가 반환하는 `NodeList` 객체는 `HTMLCollection` 객체와 같이 live 객체로 동작하므로 주의해야 한다.

💡가장 좋은 방법은 `HTMLCollection`이나 `NodeList`를 사용하지 않고 스프레드 문법이나 `Array.from` 메서드로 배열로 변환하여 배열 고차 함수를 사용하는 것이다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
    const { childNodes } = $fruits;

    // 스프레드 문법을 사용하여 NodeList 객체를 배열로 변환한다.
    [...childNodes].forEach((childNode) => {
      $fruits.removeChild(childNode);
    });

    // $fruits 요소의 모든 자식 노드가 모두 삭제되었다.
    console.log(childNodes); // NodeList []
  </script>
</html>
```

## 39.3 노드 탐색

요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리를 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있다.

DOM 트리 상의 노드를 탐색할 수 있도록 `Node`, `Element` 인터페이스는 트리 탐색 프로퍼티를 제공한다.

`parentNode`, `previousSibling`, `firstChild`, `childNodes` 프로퍼티는 `Node.prototype`이 제공하고, 프로퍼티 키에 `Element`가 포함된 `previousElementSibling`, `nextElementSibling`과 `children` 프로퍼티는 `Element.prototype`이 제공한다.

노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단, 노드 탐색 프로퍼티는 setter없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티다. 값을 할당하면 아무런 에러 없이 무시된다.

### 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 개행 등의 공백 문자는 텍스트 노드를 생성한다. 이를 공백 텍스트 노드라 한다.

HTML 문서에 공백 문자는 파싱되어 공백 텍스트 노드를 생성한다.

보통 `Node` 객체의 메서드는 텍스트 공백을 포함한다.

### 자식 노드 탐색

자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.

| 프로퍼티                                | 설명                                                                                                                                                                        |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Node.prototype.childNodes**           | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환한다. `childNodes` 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다. |
| **Element.prototype.children**          | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환한다. `children` 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않는다. |
| **Node.prototype.firstChild**           | 첫 번째 자식 노드를 반환한다. `firstChild` 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.                                                                          |
| **Node.prototype.lastChild**            | 마지막 자식 노드를 반환한다. `lastChild` 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.                                                                            |
| **Element.prototype.firstElementChild** | 첫 번째 자식 요소 노드를 반환한다. `firstElementChild` 프로퍼티는 요소 노드만 반환한다.                                                                                     |
| **Element.prototype.lastElementChild**  | 마지막 자식 요소 노드를 반환한다. `lastElementChild` 프로퍼티는 요소 노드만 반환한다.                                                                                       |

### 자식 노드 존재 확인

`Node.prototype.hasChildNodes` 메서드를 사용한다. 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.

텍스트 노드가 아닌 요소 노드 존재를 확인하려면 `children.length` 또는 `Element` 인터페이스의 `childElementCount` 프로퍼티를 사용한다.

### 요소 노드의 텍스트 노드 탐색

`firstChild` 프로퍼티로 접근할 수 있다.

### 부모 노드 탐색

`Node.prototype.parentNode` 프로퍼티를 사용한다.

### 형제 노드 탐색

속성 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않는다.

| 프로퍼티                                     | 설명                                                                                                                                                                         |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Node.prototype.previousSibling**           | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환한다. `previousSibling` 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다. |
| **Node.prototype.nextSibling**               | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환한다. `nextSibling` 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다.     |
| **Element.prototype.previousElementSibling** | 부모 노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환한다. `previousElementSibling` 프로퍼티는 요소 노드만 반환한다.                             |
| **Element.prototype.nextElementSibling**     | 부모 노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환한다. `nextElementSibling` 프로퍼티는 요소 노드만 반환한다.                                 |

## 39.4 노드 정보 취득

다음과 같은 노드 정보 프로퍼티를 사용한다.

| 프로퍼티                    | 설명                                                                                                                                                                                                                                                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Node.prototype.nodeType** | 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다. 노드 타입 상수는 `Node`에 정의되어 있다.<br> • `Node.ELEMENT_NODE`: 요소 노드 타입을 나타내는 상수 `1`을 반환<br> • `Node.TEXT_NODE`: 텍스트 노드 타입을 나타내는 상수 `3`을 반환<br> • `Node.DOCUMENT_NODE`: 문서 노드 타입을 나타내는 상수 `9`를 반환 |
| **Node.prototype.nodeName** | 노드의 이름을 문자열로 반환한다.<br> • 요소 노드: 대문자 문자열로 태그 이름 (`"UL"`, `"LI"` 등)을 반환<br> • 텍스트 노드: 문자열 `"#text"`를 반환<br> • 문서 노드: 문자열 `"#document"`를 반환                                                                                                                     |

## 39.5 요소 노드의 텍스트 조작

### `nodeValue`

`Node.prototype.nodeValue` 프로퍼티 setter와 getter 모두 존재하는 접근자 프로퍼티다. `nodeValue` 프로퍼티는 참조와 할당 모두 가능하다.

참조하면 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트다. 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 `nodeValue` 프로퍼티를 참조하면 `null`을 반환한다.

텍스트를 변경하려면,

1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드를 탐색한다. 텍스트 노드는 요소 노드의 자식 노드이므로 `firstChild` 프로퍼티를 사용하여 탐색한다.
2. 탐색한 텍스트 노드의 `nodeValue` 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.

### `textContent`

`Node.prototype.textContent` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.

참조하면 HTML 마크업을 제외한 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환한다.

`nodeValue`는 텍스트 노드의 `nodeValue` 프로퍼티를 참조할 때만 텍스트를 반환한다. 때문에 코드가 좀 더 복잡하다.

요소 노드의 `textContent` 프로퍼티에 문자열을 할당하면 요소 노드의 **모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가**된다. 이떄 문자열에 HTML 마크업이 포함되어 있어도 텍스트로 취급한다. 파싱이 이루어지지 않는다.

`innerText` 프로퍼티도 있다. 사용하지 않는 것이 좋은데, 그 이유는

1. CSS에 순종적이다. 예를 들어 `visibility: hidden;`으로 지정된 요소 노드의 텍스트를 반환하지 않는다.
2. CSS를 고려하므로 `textContent`보다 느리다.

## 39.6 DOM 조작

DOM 조작은 새로운 노드를 생성해 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.

이는 리플로우와 리페인트를 발생시켜 성능에 영향을 준다.

### `innerHTML`

`Element.prototype.innerHTML` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경한다. 요소 노드의 `innerHTML` 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내 포함된 모든 HTML 마크업을 문자열로 반환한다.

`textContent`와 다르게 HTML 마크업을 포함한 문자열을 그대로 반환한다.

문자열을 할당하게 되면 요소 노드의 **모든 자식 노드가 제거되고 할당한 문자열에 포함된 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영**된다.

⚠️이때 사용자로부터 입력받은 데이터를 그대로 `innerHTML` 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격(XSS: Cross-site Scripting Attacks)에 취약하므로 위험하다.

HTML5는 `innerHTML` 프로퍼티로 삽입된 `script` 요소 내의 자바스크립트 코드를 실행하지 않는다. 하지만 `script` 요소 없이도 XSS가 가능하므로 주의할 필요가 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 에러 이벤트를 강제로 발생시켜서 자바스크립트 코드가 실행되도록 한다.
    document.getElementById(
      "foo"
    ).innerHTML = `<img src="x" onerror="alert(document.cookie)">`;
  </script>
</html>
```

> HTML 새니티제이션(HTML sanitization): XSS 공격을 막기 위해 잠재적 위험을 제거하는 기능을 말한다. 직접 구현할 수 있지만 DOMPurify 라이브러리를 사용하는 것을 권장한다.
>
> ```js
> DOMPurify.sanitize(`<img src="x" onerror="alert(document.cookie)">`); // <img src="x">
> ```
>
> 2014년 2월부터 제공하는 기능으로 어느 정도 안정성이 보장된 새니티제이션 라이브러리다.

아래는 HTML 새니티제이션 코드 예제다.

```js
function sanitizeHTML(html) {
  const parser = new DOMParser();
  const doc = parser.parseFromString(html, "text/html");

  const allowedTags = ["div", "span", "img", "a", "p"]; // 명시된 태그만 허용
  const allowedAttributes = {
    img: ["src", "alt"],
    a: ["href", "title"],
    div: [],
    span: [],
    p: [],
  }; // 명시된 속성만 허용

  function sanitizeNode(node) {
    if (node.nodeType === Node.ELEMENT_NODE) {
      if (!allowedTags.includes(node.tagName.toLowerCase())) {
        node.remove();
        return;
      }

      // 속성 검사
      [...node.attributes].forEach((attr) => {
        const allowed = allowedAttributes[node.tagName.toLowerCase()] || [];
        if (!allowed.includes(attr.name.toLowerCase())) {
          node.removeAttribute(attr.name);
        }

        // src나 href에 javascript: 같은게 들어가는지 체크
        if (
          ["src", "href"].includes(attr.name.toLowerCase()) &&
          attr.value.trim().toLowerCase().startsWith("javascript:")
        ) {
          node.removeAttribute(attr.name);
        }
      });
    }

    // 자식도 반복
    node.childNodes.forEach((child) => sanitizeNode(child));
  }

  doc.body.childNodes.forEach((child) => sanitizeNode(child));

  return doc.body.innerHTML;
}
```

> HTML 태그를 사용자 입력으로 받는 경우는 댓글 작성, 게시글 편집 같은 텍스트 기능에 `<b>`나 `<i>` 같은 꾸밈 태그를 허용할 때가 있다.

다른 단점은 HTML 마크업 문자열을 할당할 때 모든 자식 노드를 제거하고 다시 처음부터 새롭게 자식 노드를 생성한다는 것이다.

```js
$fruits.innerHTML += '<li class="banana">Banana</li>';
```

이 코드는 HTML 마크업을 기존의 자식 노드에 추가하지 않는다. 모두 제거하고 다시 생성한다.

또, 새로운 요소를 삽입할 때 삽입될 위치를 지정해줄 수 없다는 단점도 있다.

**💡`innerHTML` 프로퍼티는 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋다.**

### `insertAdjacentHTML` 메서드

`Element.prototype.insertAdjacentHTML(position, DOMString)` 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다. 때문에 `innerHTML` 프로퍼티보다 효율적이고 빠르다.

`position`에는 `beforebegin`, `afterbegin`, `beforeend`, `afterend` 4가지를 전달할 수 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <!-- beforebegin -->
    <div id="foo">
      <!-- afterbegin -->
      text
      <!-- beforeend -->
    </div>
    <!-- afterend -->
  </body>
  <script>
    const $foo = document.getElementById("foo");

    $foo.insertAdjacentHTML("beforebegin", "<p>beforebegin</p>");
    $foo.insertAdjacentHTML("afterbegin", "<p>afterbegin</p>");
    $foo.insertAdjacentHTML("beforeend", "<p>beforeend</p>");
    $foo.insertAdjacentHTML("afterend", "<p>afterend</p>");
  </script>
</html>
```

### 노드 생성과 추가

DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.

- 요소 노드 생성

`Document.prototype.createElement(tagName)` 메서드는 요소 노드를 생성해 반환한다. `tagName`에는 태그 이름을 나타내는 문자열을 인수로 전달한다.

생성만 할 뿐, 별도의 처리를 통해 DOM에 추가해주는 작업이 필요하다.

생성 직후에는 아무런 자식 노드가 없어 `childNodes`에 텍스트 노드도 없다.

- 텍스트 노드 생성

`Document.prototype.createTextNode(text)` 메서드는 텍스트 노드를 생성해 반환한다. `text`에는 텍스트 노드 값으로 사용할 문자열을 인수로 전달한다.

생성만 할 뿐, 별도의 처리를 통해 요소 노드에 추가해주는 작업이 필요하다.

- 텍스트 노드를 요소 노드의 자식 노드로 추가

`Node.prototype.appendChild(childNode)` 메서드는 `childNode`에 인수로 전달한 노드를 `appendChild` 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.

요소 노드에 자식 노드가 하나도 없는 경우, `textContent` 프로퍼티를 사용하는 편이 더욱 간편하다.

```js
// 텍스트 노드를 생성하여 요소 노드의 자식 노드로 추가
$li.appendChild(document.createTextNode("Banana"));

// $li 요소 노드에 자식 노드가 하나도 없는 위 코드와 동일하게 동작한다.
$li.textContent = "Banana";
```

- 요소 노드를 DOM에 추가

`Node.prototype.appendChild` 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 추가하고 싶은 요소 노드의 마지막 자식 요소로 추가한다.

이렇게 새로 생성한 노드가 DOM에 추가되고, 리플로우와 리페인트가 발생한다.

### 복수의 노드 생성과 추가

기존 DOM에 요소 노드를 반복하여 추가하는 것은 비효율적이다.

컨테이너 요소를 미리 생성한 다음, DOM에 추가해야 할 3개의 요소 노드를 요소에 자식 노드로 추가하고, 컨테이너 요소를 자식으로 추가하면 DOM은 한 번만 변경된다.

컨테이너 요소는 기존의 방식보다는 효율적이지만, 불필요한 컨테이너 요소가 DOM에 추가되는 부작용이 있다.

이러한 문제는 `DocumentFragment` 노드를 통해 해결할 수 있다. `DocumentFragment` 노드는 문서, 요소, 속성, 텍스트 노드와 같은 노드 객체의 일종으로, 부모 노드가 없어서 기존 DOM과는 별도로 존재한다는 특징이 있다. 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용한다.

`Document.prototype.createDocumentFragment` 메서드는 비어 있는 `DocumentFragment` 노드를 생성하여 반환한다.

```js
const $fruits = document.getElementById("fruits");

// DocumentFragment 노드 생성
const $fragment = document.createDocumentFragment();

["Apple", "Banana", "Orange"].forEach((text) => {
  // 1. 요소 노드 생성
  const $li = document.createElement("li");

  // 2. 텍스트 노드 생성
  const textNode = document.createTextNode(text);

  // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
  $li.appendChild(textNode);

  // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
  $fragment.appendChild($li);
});

// 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
$fruits.appendChild($fragment);
```

### 노드 삽입

- 마지막 노드로 추가

`Node.prototype.appendChild` 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다. 언제나 마지막 자식 노드로 추가한다.

- 지정한 위치에 노드 삽입

`Node.prototype.insertBefore(newNode, childNode)` 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.

```js
const $fruits = document.getElementById("fruits");

// 요소 노드 생성
const $li = document.createElement("li");

// 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
$li.appendChild(document.createTextNode("Orange"));

// $li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
$fruits.insertBefore($li, $fruits.lastElementChild);
// Apple - Orange - Banana
```

`childNode`는 반드시 메서드를 호출한 노드의 자식 노드여야 하며, 그렇지 않을 경우 `DOMException` 에러가 발생한다.

`childNode`가 `null`이면 `appendChild` 메서드처럼 동작한다.

### 노드 이동

DOM에 이미 존재하는 노드를 `appendChild` 또는 `insertBefore` 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. 이런 식으로 노드를 이동 시킬 수 있다.

```js
const $fruits = document.getElementById("fruits");

// 이미 존재하는 요소 노드를 취득
const [$apple, $banana] = $fruits.children;

// 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
$fruits.appendChild($apple); // Banana - Orange - Apple

// 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
$fruits.insertBefore($banana, $fruits.lastElementChild);
// Orange - Banana - Apple
```

### 노드 복사

`Node.prototype.cloneNode([deep: true | false])` 메서드는 노드의 사본을 생성해 반환한다. `deep`에 `true`를 인수로 전달하면 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성, `false`를 전달하거나 생략하면 노드를 얕은 복사하여 노드 자신만의 사본을 생성한다. 얕은 복사로 생성된 요소 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.

```js
const $fruits = document.getElementById("fruits");
const $apple = $fruits.firstElementChild;

// $apple 요소를 얕은 복사하여 사본을 생성. 텍스트 노드가 없는 사본이 생성된다.
const $shallowClone = $apple.cloneNode();
// 사본 요소 노드에 텍스트 추가
$shallowClone.textContent = "Banana";
// 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
$fruits.appendChild($shallowClone);

// #fruits 요소를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
const $deepClone = $fruits.cloneNode(true);
// 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
$fruits.appendChild($deepClone);
```

### 노드 교체

`Node.prototype.replaceChild(newChild, oldChild)` 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다. `oldChild` 노드를 제거하고 `newChild` 노드로 교체한다.

### 노드 삭제

`Node.prototype.removeChild(child)` 메서드는 `child`에 인수로 전달한 노드를 DOM에서 삭제한다.

## 39.7 어트리뷰트(속성)

### 속성 노드와 `attributes` 프로퍼티

HTML 요소는 여러 개의 속성을 가질 수 있다. HTML 속성은 HTML 시작 태그에 `속성 이름="속성 값"` 형식으로 정의한다.

글로벌 속성(`id`, `class`, `style`, `title`, `lang`, `tabindex`, `draggable`, `hidden` 등)과 이벤트 핸들러 속성은 모든 HTML 요소가 가질 수 있지만, 특정 HTML 요소에만 한정으로 사용 가능한 속성도 있다. 예를 들어, `type`, `value`, `checked` 태그는 `input` 태그에만 사용할 수 있다.

HTML 문서 파싱 시 HTML 속성은 속성 노드로 변환되어 요소 노드와 연결된다. 속성 당 하나의 속성 노드가 생성된다.

이떄 모든 속성 노드의 참조는 유사 배열 객체이자 이터러블인 `NamedNodeMap` 객체에 담겨서 요소 노드의 `attributes` 프로퍼티에 저장된다.

요소 노드의 모든 속성 노드는 요소 노드의 `Element.prototype.attributes` 프로퍼티로 취득할 수 있다.
getter만 존재하는 읽기 전용 접근자 프로퍼티다.

```html
<!DOCTYPE html>
<html>
  <body>
    <input id="user" type="text" value="ungmo2" />
    <script>
      // 요소 노드의 attribute 프로퍼티는 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.
      const { attributes } = document.getElementById("user");
      console.log(attributes);
      // NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

      // 어트리뷰트 값 취득
      console.log(attributes.id.value); // user
      console.log(attributes.type.value); // text
      console.log(attributes.value.value); // ungmo2
    </script>
  </body>
</html>
```

### HTML 속성 조작

`Element.prototype.getAttribute/setAttribue` 메서드를 사용하면 `attributes` 프로퍼티를 통하지 않고 요소 노드에서 메서드를 통해 직접 속성 값을 얻거나 변경할 수 있다.

`Element.prototype.hasAttribute(attributeName)` 메서드로 속성의 유무를 확인한다.

`Element.prototype.removeAttribute(attributeName)` 메서드로 특정 속성을 삭제한다.

### HTML 속성 vs. DOM 프로퍼티

요소 노드 객체에는 HTML 속성에 대응하는 프로퍼티(DOM 프로퍼티)가 존재한다. DOM 프로퍼티는 HTML 속성 값을 초기값으로 가지고 있다. getter와 setter 모두 존재하는 접근자 프로퍼티다.

요소 노드의 초기 상태는 속성 노드가, 최신 상태는 DOM 프로퍼티가 관리한다.

- 속성 노드

요소 노드의 초기 상태를 관리한다. 사용자의 입력에 의해 상태가 변경되어도 변하지 않고 초기 상태를 유지한다.

`getAttribute` 메서드는 언제나 초기 상태를 가져온다.

`setAttribute` 메서드는 초기 상태 값을 변경하기 위해 사용한다.

- DOM 프로퍼티

사용자가 입력한 최신 상태를 관리한다. 단, 모든 DOM 프로퍼티가 사용자의 입력에 의해 변경된 최신 상태를 관리하는 것은 아니다.

예를 들어, `checkbox` 요소의 사용자 입력에 의한 상태 변화는 `checked` 프로퍼티가 관리한다. 하지만 `id` 속성에 대응하는 `id` 프로퍼티는 사용자의 입력과 아무런 관계가 없다. `id` 속성이 변하면 `id` 프로퍼티 역시 변한다. 그 반대도 마찬가지다.

"사용자 입력에 의한 상태 변화와 관계있는" DOM 프로퍼티만 최신 상태 값을 관리한다. 그 외에는 속성과 DOM 프로퍼티는 항상 동일한 값으로 연동한다.

- HTML 속성과 DOM 프로퍼티의 대응 관계

대부분은 서로 1:1 대응하지만 언제나 그런 것은 아니며, 속성 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아니다.

1. `id` 어트리뷰트와 `id` 프로퍼티는 1:1 대응하며, 동일한 값으로 연동한다.
2. `input` 요소의 `value` 어트리뷰트는 `value` 프로퍼티와 1:1 대응한다. 하지만 `value` 어트리뷰트는 초기 상태를, `value` 프로퍼티는 최신 상태를 갖는다.
3. `class` 어트리뷰트는 `className`, `classList` 프로퍼티와 대응한다.
4. `for` 어트리뷰트는 `htmlFor` 프로퍼티와 1:1 대응한다.
5. `td` 요소의 `colspan` 어트리뷰트는 대응하는 프로퍼티가 존재하지 않는다.
6. `textContent` 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
7. 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따른다(`maxlength` → `maxLength`).

- DOM 프로퍼티 값의 타입

`getAttribute` 메서드로 취득한 속성 값은 언제나 문자열이다. 하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다. 예를 들어, `checkbox` 요소의 `checked` 속성 값은 문자열이지만 `chekced` 프로퍼티 값을 불리언 타입이다.

### `data` 속성과 `dataset` 프로퍼티

`data` 속성과 `dataset` 프로퍼티는 HTML 요소에 정의한 사용자 정의 속성과 자바스크립트 간에 데이터를 교환하는데 사용된다. `data` 속성은 `data-user-id`와 같이 `data-` 접두사 다음 임의의 이름을 붙여 사용한다.

`data` 속성 값은 `HTMLElement.dataset` 프로퍼티로 취득할 수 있다. `dataset` 프로퍼티는 HTML 요소의 모든 `data` 속성의 정보를 제공하는 `DOMStringMap` 객체를 반환한다. `DOMStringMap` 객체는 `data` 속성의 `data-` 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있다. 이 프로퍼티로 `data` 속성 값을 취득하거나 변경할 수 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul class="users">
      <li id="1" data-user-id="7621">Lee</li>
      <li id="2" data-user-id="9524">Kim</li>
    </ul>
    <script>
      const users = [...document.querySelector(".users").children];

      // user-id가 '7621'인 요소 노드를 취득한다.
      const user = users.find((user) => user.dataset.userId === "7621");

      // user-id가 '7621'인 요소 노드에 새로운 data 어트리뷰트를 추가한다.
      user.dataset.role = "admin";
      console.log(user.dataset);
      /*
    DOMStringMap {userId: "7621", role: "admin"}
    -> <li id="1" data-user-id="7621" data-role="admin">Lee</li>
    */
    </script>
  </body>
</html>
```

`dataset` 프로퍼티에 값을 할당하면 HTML 요소에 `data` 속성이 추가된다. 이때 케밥 케이스로 자동 변경되어 추가된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul class="users">
      <li id="1" data-user-id="7621">Lee</li>
      <li id="2" data-user-id="9524">Kim</li>
    </ul>
    <script>
      const users = [...document.querySelector(".users").children];

      // user-id가 '7621'인 요소 노드를 취득한다.
      const user = users.find((user) => user.dataset.userId === "7621");

      // user-id가 '7621'인 요소 노드에 새로운 data 어트리뷰트를 추가한다.
      user.dataset.role = "admin";
      console.log(user.dataset);
      /*
    DOMStringMap {userId: "7621", role: "admin"}
    -> <li id="1" data-user-id="7621" data-role="admin">Lee</li>
    */
    </script>
  </body>
</html>
```

## 39.8 스타일

### 인라인 스타일 조작

`HTMLElement.prototype.style` 프로퍼티는 setter와 getter를 모두 갖는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가, 변경한다.

`style` 프로퍼티를 참조하면 `CSSStyleDeclaration` 타입의 객체를 반환한다. `CSSStyleDeclaration` 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 갖는다. 이 프로퍼티에 값을 할당하면 해당 CSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.

CSS 프로퍼티는 케밥 케이스를 따르지만 `CSSStyleDeclaration` 객체의 프로퍼티는 카멜 케이스를 따르기에 다음과 같이 사용할 수 있다.

```js
$div.style.backgroundColor = "yellow";
$div.style["background-color"] = "yellow";
```

단위 지정이 필요한 값(`width` 등)은 반드시 단위를 지정해야 한다. 그렇지 않으면 해당 CSS 프로퍼티는 적용되지 않는다.

### 클래스 조작

`.`으로 시작하는 클래스 선택자를 사용해 CSS `class`를 미리 정의하고 HTML 요소의 `class` 속성 값을 변경하여 HTML 요소의 스타일을 변경할 수 있다.

`class` 속성에 대응하는 DOM 프로퍼티는 `className`과 `classList`다. (`class`가 JS의 예약어이므로)

- `className`

`Element.prototype.className` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 HTML 요소의 `class` 속성 값을 취득하거나 변경한다.

요소 노드의 `className` 프로퍼티를 참조하면 `class` 속성 값을 문자열로 반환하고, 요소 노드의 `className` 프로퍼티에 문자열을 할당하면 `class` 속성 값을 할당한 문자열로 변환한다.

- `classList`

`Element.prototype.classList` 프로퍼티는 `class` 속성의 정보를 담은 `DOMTokenList` 객체를 반환한다.

`DOMTokenList` 객체는 `class` 속성의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블이다. 유용한 메서드들을 제공한다.

대부분 문자열을 인수로 받는다.

1. `add(...className)`: 속성 값 추가
2. `remove(...className)`: 일치하는 속성 값 삭제
3. `item(index)`: 인덱스에 해당하는 클래스 반환
4. `contains(className)`: 클래스 포함 여부 확인
5. `replace(oldClassName, newClassName)`: 첫 번째 인수를 두 번째 인수로 변경, 두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있고, `true`일 시 `class` 속성에 클래스 강제 추가, `false`일 시 `class` 속성에서 강제 삭제
6. `toggle(className[. force])`: 클래스가 존재하면 제거, 없으면 추가
7. `forEach`, `entries`, `keys`, `values`, `supports` 등...

### 요소에 적용되어 있는 CSS 스타일 참조

`style` 프로퍼티는 인라인 스타일만 반환한다. 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 `style` 프로퍼티로 참조할 수 없다. HTML 요소에 적용된 모든 CSS 스타일을 참조해야 할 경우 `getComputedStyle` 메서드를 사용한다.

`window.getComputedStyle(element[, pseudo])` 메서드는 첫 번째 인수로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 `CSSStyleDeclaration` 객체에 담아 반환한다. 평가된 스타일이란, 스타일을 적용할 수 있는 모든 수단이 평가되어 최종적으로 적용된 스타일을 말한다.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        color: red;
      }
      .box {
        width: 100px;
        height: 50px;
        background-color: cornsilk;
        border: 1px solid black;
      }
    </style>
  </head>
  <body>
    <div class="box">Box</div>
    <script>
      const $box = document.querySelector(".box");

      // .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
      const computedStyle = window.getComputedStyle($box);
      console.log(computedStyle); // CSSStyleDeclaration

      // 임베딩 스타일
      console.log(computedStyle.width); // 100px
      console.log(computedStyle.height); // 50px
      console.log(computedStyle.backgroundColor); // rgb(255, 248, 220)
      console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

      // 상속 스타일(body -> .box)
      console.log(computedStyle.color); // rgb(255, 0, 0)

      // 기본 스타일
      console.log(computedStyle.display); // block
    </script>
  </body>
</html>
```

두 번째 인수로 `:after`, `:before` 같은 의사 요소를 지정하는 문자열을 전달할 수 있다. 일반 요소의 경우 두 번째 인수는 생략한다.

## 39.9 DOM 표준

HTML과 DOM 표준은 W3C과 WHATWG(Web Hypertext Application Technology Working Group)이라는 두 단체가 공통된 표준을 만들어 왔다.

그런데 최근 서로 다른 결과물을 내놓기 시작하면서 2018년 4월부터 구글, 애플, MS, 모질라로 구성된 WHATWG이 단일 표준을 내놓기로 두 단체가 합의했다.

DOM은 현재 4개의 레벨(버전)이 있다.
