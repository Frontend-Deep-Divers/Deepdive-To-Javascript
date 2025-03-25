# 21.1 자바스크립트 객체의 분류

---

- 자바스크립트 객체 3가지
  1. **표준 빌트인 객체:** ECMAScript 사양에 정의된 객체를 말하며,애플리케이션 전역의 공통 기능을 제공한다. 자바스크립트 실행 환경과 관계없이 언제나 사용할 수 있다. 따로 별도의 선언 없이 언제나 참조할 수 있다.
  2. **호스트 객체 :** ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체이다.
     - 브라우저 환경: DOM, BOM , Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API를 호스트 객체로 제공
     - Node.js 환경: Node.js 고유의 API 를 호스트 객체로 제공한다.
  3. **사용자 정의 객체:** 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

# 21.2 표준 빌트인 객체

---

- Object, String, Number, Bolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 갱의 표준 빌트인 객체를 제공한다.
- Math, Reflect, JSON 을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
- 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

![image.png](attachment:dbaaae2f-83f1-4612-a008-2b9a8192e91f:image.png)

- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

```jsx
const strObj = new String("Lee");
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

- 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공한다. 그리고 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공한다.

```jsx
const numObj = new Number(1.5);
console.log(numObj.toFixed()); // 2
console.log(Number.isInteger(1.5)); // false
```

# 21.3 원시값과 래퍼 객체

---

- 문자열, 숫자, 불리언 등의 원시값이 존재하는 데 Number,String,Boolean 등 표준 빌트인 생성자 함수는 왜 존재하는 걸까?

```jsx
const str = "hello";
console.log(str.length);
console.log(str.toUpperCase());
```

- 원시값인데 객체처럼 프로퍼티나 메서드를 동작시킬 수 있다?
- 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 . 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값과 연관된 객체로 바꿔주기 때문임. → 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.
- **🌟 이렇게 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라고 한다.**
- 예를 들어, 문자열에 대한 마침표 표기법으로 접근하면, 순간 String 생성자 함수의 인스턴스가 생성되고 문자열은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.

![image.png](attachment:a5f9ffdf-d9a7-4515-9280-260fadd625a9:image.png)

- 🌟 **그 후 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.**

```jsx
const str = "hello";
str.name = "Oh";
console.log(str.name); // undefined 이전과 다른 래퍼 객체를 가르킴.
```

- 당연히 숫자 값도 마찬가지.
- ES6 에서 도입된 심벌로 래퍼 객체를 생성한다. → 단 리터럴 표기법으로 생성할 수 없어서 다른 원시값과는 차이가 있다 → 이후 장에서 살펴보기
- 문자열, 숫자, 불리언, 심벌은 암묵적으로 생성되는 래퍼 객체에 의해 객체처럼 사용할 수 있어서 프로퍼티와 메서드를 참조할 수 있으니, String, Number, Boolean 생성자 함수를 new 와 함께 호출할 필요가 없으며, 권장하지 않는다.

# 21.4 전역 객체

---

- 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않는 최상위 객체이다.
- ❗브라우저 환경에서는 window, Node,js 환경에서는 global 이 전역 객체
- ⚠️ globalThis 라고 ES11 에서 통일한 식별자가 나왔다.
- ❗ 전역 객체는 표준 빌트인 객체와 호스트 객체, var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.
- ❗ 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않는 모든 빌트인 객체의 최상위 객체이다. 프로토타입 상속 관계에서 최상위라는 의미가 아니다.
- 전역 객체 특징

  - 개발자가 의도적으로 생성할 수 없다. 생성자 함수가 제공되지 않는다.
  - window(또는 global)을 생략하고 참조할 수 있다.
  - 전역 객체는 Object, String, Number, Boolean, Function, RegExp, Date , Math, Promise 와 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
  - 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다. (각 환경에 따른 호스트 객체)
  - var 키워드로 선언한 전역 변수와 선언하지 않은 전역 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.
  - let, const 키워드로 선언한 변수는 전역 객체의 프로퍼티가 아니다. let 이나 const 로 선언한 전역 변수는 보이지 않는 개념적인 블록 (🌟 **전역 렉시컬 환경의 선언적 환경 레코드** ) 내에 존재하게 된다.
  - 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체를 공유한다. 여러 개의 script 태그를 통해 자바스크립트 코드를 분리해도 하나의 전역 객체를 공유하는 것은 변함이 없다. 즉, 하나의 전역을 공유한다.

- 전역 객체가 가진 몇 가지 프로퍼티와 메서드 살펴보기.

1. **빌트인 전역 프로퍼티**

   - 전역 객체의 프로퍼티를 의미한다. 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

   1️⃣ **Infinity**

   - 무한대를 나타내는 숫자값.

   ```jsx
   console.log(3 / 0); // Infinity
   console.log(-3 / 0); // -Infinity
   console.log(typeof Infinity); // number
   ```

   **2️⃣ NaN**

   - NaN 프로퍼티는 숫자가 아님을 나타내는 숫자값임.
   - Number.NaN 과 같다.

   ```jsx
   console.log(Number("as")); // NaN
   console.log(1 * "String"); // NaN
   console.log(typeof NaN); // Number
   ```

   **3️⃣ undefined**

   - 정의하지 않음.

1. **빌트인 전역 함수**

   - 전역 객체의 메서드

   1️⃣ **eval**

   - 전달받은 문자열 코드가 표현식이면 런타임에 평가하여 값을 생성하고, 표현식이 아닌 문이라면 런타임에 실행한다.
   - 하지만 eval 함수는 보안에 매우 취약하며, 자바스크립트 엔진에 의해 최적화가 수행되지 않아, 처리속도가 느리다. → ❗사용을 금지!! 해야 한다.

   2️⃣ **isFinite**

   - 전달받은 인수가 정상적인 유한수라면 true, 무한수라면 false 를 반환한다.
   - 전달받은 인수가 숫자가 아닌 경우 타입 변환 후 검사 수행.
   - 만약 인수가 NaN 으로 평가되는 값이라면 false 반환

   ```jsx
   console.log(isFinite(0)); // true
   console.log(isFinite(2e64)); // true
   console.log(isFinite("10")); // true
   console.log(isFinite(null)); // true null -> 0
   console.log(isFinite(Infinity)); // false
   console.log(isFinite(-Infinity)); // false
   console.log(isFinite(NaN)); // false
   console.log(isFinite("Hello")); // false
   console.log(isFinite("2024/12/12")); // false
   ```

   **3️⃣ isNaN**

   - 전달받은 인수가 NaN 인지 검사하여 그 결과를 불리언 타입으로 반환한다. 전달받은 인수가 숫자가 아니라면 숫자로 변환 후 검사 수행.

   ![image.png](attachment:dee3ef04-8f8f-44b7-a52f-b328ed609518:image.png)

   ![image.png](attachment:b3d727ec-af6b-4b6d-8d4e-fe53dd3b1e14:image.png)

   **4️⃣ parseFloat**

   - 전달 받은 문자열 인수를 부동 소수점 숫자 실수로 해석해 반환한다.

   ![image.png](attachment:2540a1cb-c6e5-4fc4-b49e-2afe1771c58e:image.png)

   - ⚠️ 첫 문자열만 해석한다. 만약 숫자 중간에 문자열이 들어가면 숫자 까지만 변환한다.

   ```jsx
   console.log(parseFloat("3.14d1da")); // 3.14
   ```

   **5️⃣ parseInt**

   - 전달 받은 인수를 정수로 해석해 반환한다.

   ![image.png](attachment:0ad28520-9fdd-4960-827e-62793a39c30b:image.png)

   - 전달 받은 인수가 문자열이 아니면 문자열로 변환한 다음, 정수 해석해 반환한다.

   ![image.png](attachment:923e745b-5de7-4fc7-8a24-a6aaa8eccc1d:image.png)

   - 두 번째 인수로 진법을 나타내는 기수를 전달할 수 있다. 기수를 지정하면 첫 번째 문자열을 해당 기수의 숫자로 해석해서 반환한다. 기본은 10 진수이다.

   ![image.png](attachment:887b4fbb-4566-4d82-b7a3-32ffdd40724c:image.png)

   - 10 진수 숫자 → 문자열 : Number.prototype.toString() 메서드 사용
   - toString(2) 는 2진수로 변환하여 문자열로 반환.

   ![image.png](attachment:c4a3ba54-7e2b-4445-aa88-757df55a8a89:image.png)

   - 두 번째 인수를 진법을 나타내는 기수로 지정하지 않아도 0X 혹은 0x 면 16진수로 해석해 정수로 변환한다.

   ![image.png](attachment:10f7638e-66a8-4bab-a53f-d2cb082075ae:image.png)

   - 2 나 8 진수는 제대로 해석하지 못한다. → 반드시 지수를 지정해 줘야 한다.

   ![image.png](attachment:c6669931-ac8d-45a5-824c-2a6d2efc57e8:image.png)

   ![image.png](attachment:a2d13c14-7508-47fe-85bf-18a28a7c7f5b:image.png)

   - 첫 번째 인수로 전달한 문자열의 처 번째 문자가 지수로 변환될 수 없다면 NaN 반환

   ![image.png](attachment:2cb9e852-8c4a-4cc7-a203-f482bd66bfbd:image.png)

   - 이후 문자 무시, 중간에 공백 있으면 처음만, 첫 문자 앞 뒤 공백은 무시 paresFloat 와 동일

**6️⃣ encodeURI / decodeURI**

- **`완전한 URI` 를 문자열로 전달받아, 이스케이프 처리를 위해 인코딩 한다.**
- URI 와 URL 그리고 이스케이프 처리와 인코딩
  ### 🔗 URI vs URL
  | 개념                                  | 설명                                           | 예시                                              |
  | ------------------------------------- | ---------------------------------------------- | ------------------------------------------------- |
  | **URI** (Uniform Resource Identifier) | 리소스를 식별하는 모든 문자열 (포괄적 개념)    | `mailto:hello@example.com`, `https://example.com` |
  | **URL** (Uniform Resource Locator)    | 리소스의 위치(주소)를 나타내는 URI의 하위 개념 | `https://example.com/index.html`                  |
  > ✅ URL ⊂ URI
  >
  > 모든 URL은 URI지만, 모든 URI가 URL은 아님.
  ***
  ### 🔐 이스케이프 처리 vs 인코딩
  | 개념                | 설명                                                 | 예시                                                   |
  | ------------------- | ---------------------------------------------------- | ------------------------------------------------------ |
  | **이스케이프 처리** | 특수 문자를 문자 그대로 쓰기 위해 `\` 등으로 구분    | `"He said, \"Hi\""` → 큰따옴표 출력                    |
  | **인코딩**          | 문자를 다른 형식(코드)으로 변환하여 전송 가능하게 함 | 공백 → `%20`, 한글 → `%ED%95%9C%EA%B8%80` (URL 인코딩) |
  > ✅ 이스케이프 처리는 프로그래밍 코드 내 특수문자용,
  >
  > ✅ **인코딩**은 **전송·저장 등을 위한 변환** (특히 URL/네트워크에서 자주 사용)
- ❗네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.
- ❗URI 문법 형식 표준 RFC3986에 따르면 URL 은 아스키 문자 셋으로만 구성되어야 하며 한글을 포함한 대부분의 외국어나 아스키 문자 셋에 정의되지 않은 특수 문자의 경우 URL 에 포함될 수 없다. 이러한 것들을 시스템에 의해 해석될 수 있는 문자로 이스케이프 처리하여 문제를 예방하기 위해 처리가 필요한 것이다.
- decodeURI 함수는 이스케이프 처리 이전으로 디코딩 한다.

```jsx
const URI = "http://example.com?name=오정환&job=programmer";
const enc = encodeURI(URI);
console.log(enc); // http://example.com?name=%EC%98%A4%EC%A0%95%ED%99%98&job=programmer
const dec = decodeURI(enc);
console.log(dec); // http://example.com?name=오정환&job=programmer
```

7️⃣ encodeURIComponent / decodeURIComponent

- **`URI 구성 요소`를 인수로 전달 받아 인코딩한다.**
- encodeURIComponent 함수는 인수로 전달된 문자열을 URI 의 구성요소인 쿼리 스트링의 일부로 간주한다. 따라서 쿼리 스트링 문자열 구분가로 사용되는 = , ? , & 까지 인코딩한다.
- 반면, encodeURI 함수는 매개변수로 전달된 문자열을 완전한 URI 전체라고 간주하여 쿼리 스트림 구분자로 사용되는 =, ?, & 은 인코딩하지 않는다.

```jsx
const URICOMP = "name=오정환&job=programmer";
const enc = encodeURIComponent(URICOMP);
console.log(enc);
const dec = decodeURIComponent(enc);
console.log(dec);
const en = encodeURI(URICOMP);
console.log(en);
const de = encodeURI(en);
console.log(de);

// 결과
name%3D%EC%98%A4%EC%A0%95%ED%99%98%26job%3Dprogrammer
name=오정환&job=programmer
name=%EC%98%A4%EC%A0%95%ED%99%98&job=programmer
name=%25EC%2598%25A4%25EC%25A0%2595%25ED%2599%2598&job=programmer
```

- encodeURI 와 encodeURIComponent 의 결과가 다른 것을 확인할 수 있다.

1. **암묵적 전역**

   ```jsx
   var x = 10;

   function foo() {
     y = 20;
   }
   foo();
   // 선언하지 않은 식별자 y 를 전역에서 참조할 수 있다.
   console.log(x + y); // 30
   ```

   - foo 함수 내의 y 는 선언하지 않은 식별자 이지만 전역 변수처럼 동작한다.
   - 🌟 선언하지 않은 식별자는 전역 객체의 프로퍼티가 되기 때문! → 이를 암묵적 전역이라고 한다. ← 자바스크립트 엔진이 y = 20 을 window.y = 20 으로 해석해 전역 객체에 프로퍼티를 동적으로 생성하기 때문이다. → **암묵적 전역**
   - 🌟 y 는 변수 선언 없이 전역 객체의 프로퍼티로 추가된 것이기 때문에, 변수 호이스팅이 발생하지 않는다.

   ```jsx
   console.log(x); // undefined
   console.log(y); // 참조 에러 발생

   var x = 10;

   function foo() {
     y = 20;
   }
   foo();

   console.log(x + y);
   ```

   - ❗ 또한 변수가 아니라 단지 프로퍼티인 y 는 delete 연산자로 삭제할 수 있다. ❗ 전역 변수는 프로퍼티지만 delete 연산자로 삭제할 수 없다.

   ```jsx
   var x = 10;

   function foo() {
     y = 20;
   }
   foo();

   console.log(globalThis.x); // 10
   console.log(globalThis.y); // 20

   delete x;
   delete y;

   console.log(globalThis.x); // 10
   console.log(globalThis.y); // undefined
   ```

   - 브라우저 와 node.js
     > ✅ 브라우저에서는 var로 선언한 전역 변수는 window 객체의 프로퍼티로 등록됩니다.
     >
     > ❌ 하지만 **Node.js에서는 `var`로 선언해도 `global` 객체의 프로퍼티가 되지 않습니다.**
     ***
     ## ✅ 브라우저 환경
     ```html
     html 복사편집
     <script>
       var a = 10;
       console.log(window.a); // ✅ 10
     </script>
     ```
     - 브라우저에서는 전역 스코프에 선언된 `var` 변수는 `window` 객체의 속성이 됩니다.
     ***
     ## ❌ Node.js 환경
     ```
     js
     복사편집
     var a = 10;
     console.log(global.a); // ❌ undefined

     ```
     - Node.js에서는 전역 스코프가 **파일 단위 모듈 스코프**로 감싸져 있기 때문에
     - `var`, `let`, `const`로 선언한 변수는 `global` 객체에 붙지 않습니다.
     > 대신, Node.js에서 global에 등록하려면 명시적으로 해야 해요:
     ```
     js
     복사편집
     global.a = 10;
     console.log(global.a); // ✅ 10

     ```
     ***
     ## 💡 왜 다를까?
     - **브라우저**는 `<script>` 태그 내 코드가 전부 `window` 컨텍스트에서 실행되기 때문이고
     - **Node.js**는 각 파일을 **모듈로 감싸는 함수로 실행**하기 때문입니다.
     Node.js 내부적으로는 이렇게 감싸서 실행돼요:
     ```
     js
     복사편집
     (function(exports, require, module, __filename, __dirname) {
       var a = 10;
     })();

     ```
     그래서 `var`, `let`, `const`는 이 함수(모듈) 스코프에만 존재하게 되는 거죠.
     ***
     ## 🎯 핵심 요약
     | 구분                | `var a = 10` → 전역 객체 등록? | 비고                        |
     | ------------------- | ------------------------------ | --------------------------- |
     | 브라우저 (`window`) | ✅ `window.a === 10`           | 전역 스코프가 `window`      |
     | Node.js (`global`)  | ❌ `global.a === undefined`    | 파일이 모듈 스코프로 실행됨 |
