# 48.1 모듈의 일반적 의미

---

- 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다.
- 일반적으로 파일 단위로 분리한다.
- **❗ 🌟 모듈이 성립하기 위해서라면 파일 스코프를 가질 수 있어야 한다.**
- 각 모듈들의 자산은 기본적으로 비공개 상태이기 때문에 모든 자산은 캡슐화되어 다른 모듈에서 접근할 수 없다.
- **하지만 모듈이란 건 재사용해야 의미가 있지… → 따라서 모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능하고, 이를 export 라고 한다.**
- **모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들이는 걸 import 라고 한다.**
  ![image.png](attachment:b56f21ec-2729-4dcd-937b-b6ee67a44aa9:image.png)
- 모듈은 기능별로 분리되어 개별적인 파일로 작성되고, 명확히 단위를 분리하여 애플리케이션을 구성할 수 있어서 재사용성이 높아져 개발 효율성과 유지 보수성을 높일 수 있다.

# 48.2 자바스크립트와 모듈

---

- 하자가 많은 우리 JS…
- 원래는 import, export 도 지원 안했다…
- 자바스크립트는 파일마다 독립적인 파일 스코프를 갖지 않았다…
- 즉, 모든 자바스크립트 파일은 하나의 전역을 고유하였고, 문제가 많이 발생했다.
- **그 상황에서 제안된 것이 CommonJS 와 AMD 이다.**
- 이로써 자바스크립트의 모듈 시스템은 크게 CommonJS 와 AMD 진영으로 나뉘었고, 브라우저 환경에서 사용하기 위해 모듈 로더 라이브러리를 사용하게 되었다.
- Node.js 는 Common.js 를 체택하여 진화했다. 즉, 모듈 스코프를 가진다.

# 48.3 ES6 모듈 (ES Module)

---

- ES6 에서 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다.
- 사용 방법! script 태그에 type=”module” 을 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다.
- **브라우저와 Node.js**
  ## ✅ 브라우저 환경
  ```html
  html 복사편집
  <script type="module" src="main.js"></script>
  ```
  - 이 경우 JS 파일은 **모듈로 동작**합니다.
  - 특징:
    - `import`/`export` 사용 가능
    - **strict mode** 자동 적용
    - 모듈 간 **스코프가 독립적**
    - **지연 실행** (DOMContentLoaded 이전에 실행되지 않음)
    - **CORS 정책** 적용 (로컬에서 file://로 테스트하면 오류 가능)
  ***
  ## ✅ Node.js 환경
  Node.js는 기본적으로 CommonJS(CJS)를 사용합니다.
  하지만 **ECMAScript 모듈(ESM)**을 사용하려면:
  ### 📌 1. `package.json` 설정
  ```json
  json
  복사편집
  {
    "type": "module"
  }

  ```
  - 이 설정이 있으면 `.js` 파일은 **모듈로 인식**
  - `import/export` 구문 사용 가능
  - **`require()`는 사용 불가**
- 일반적인 자바스크립트 파일이 아닌 ESM 을 명확히 명시하기 위해 mjs 사용할 것을 권장한다.
  ![image.png](attachment:c5525342-b404-4bcd-8446-a9f8e3f0e2fe:image.png)

### 1️⃣ 모듈 스코프

- 독자적인 모듈 스코프를 가지게 된다.
- 따라서 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며, window 객체의 프로퍼티도 아니다.
  ![image.png](attachment:9e27c97e-d0da-4c1d-90f3-03f87e173d09:image.png)
- 당연히 외부 참조도 불가능하다.
  ![image.png](attachment:ffb88418-4c1b-48db-b524-bfe6adc12a00:image.png)

### 2️⃣ 3️⃣ export import

- 각각 모듈을 재사용하기 위해 내보내고 싶을 때와 사용하고 싶을 때 쓴다.
  - **export**
    ![image.png](attachment:82cfb159-106e-410c-9d56-bb630505d398:image.png)
  - **import**
    - 다른 모듈이 export 한 식별자 이름으로 import 해야 하며 ESM 의 경우 파일 확장자 생략 불가능.
    ![image.png](attachment:fc46ba8c-1b85-4177-8346-1b878f0c8a74:image.png)
    - app.js 는 진입점이므로 script 태그로 로드해야 하지만, lib.mjs 는 import 문에 의해 로드되는 의존성이므로 그렇게 로드하지 않아도 된다.
    - - as 별칭으로 한 번에 import 할 수 있다.
    ![image.png](attachment:eb47d909-3bd7-4dcb-b7db-5f6c6876fe84:image.png)
