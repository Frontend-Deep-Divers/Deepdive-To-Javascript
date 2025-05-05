# 48장 모듈

## 48.1 모듈의 일반적 의미

모듈(module)은 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다. 모듈이 성립하려면 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 한다.

자신의 파일 스코프의 자산(변수, 함수, 객체 등)은 기본적으로 비공개 상태로 캡슐화된다. 모듈은 애플리케이션과 분리되어 존재해야 한다.

공개가 필요한 자산에 한하여 명시적으로 선택적 공개한다. 이를 export라 한다.

공개된 모듈의 자산은 다른 모듈이 재사용할 수 있으며, 모듈 사용자는 일부 또는 전체의 자산을 자신의 스코프 내로 불러들여 재사용한다. 이를 import라 한다.

모듈화로 코드의 단위를 명확히 분리하여 애플리케이션을 구성할 수 있고, 재사용성이 좋아서 개발 효율성과 유지보수성을 높일 수 있다.

## 48.2 자바스크립트와 모듈

자바스크립트는 파일 스코프와 import, export를 지원하지 않았었다.

클라이언트 사이드 자바스크립트는 `script` 태그를 사용하여 외부 자바스크립트 파일을 로드할 수 있지만 독립적인 파일 스코프를 갖지 않는다. 이는 전역 변수의 충돌 같은 문제를 발생시킨다.

모듈 시스템을 위해 CommonJS와 AMD(Asynchronous Module Definition)이 생겨났다. 자바스크립트 모듈 시스템은 크게 두 진영으로 나뉘게 되었다.

Node.js는 모듈 시스템의 사실상 표준인 CommonJS를 채택했고 독자적인 진화를 거쳐, 현재는 CommonJS 사양과 100% 동일하지 않지만 CommonJS를 따르는 모듈 시스템을 지원한다.

## 48.3 ES6 모듈(ESM)

ES6에서 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다.

`script` 태그에 `type="module"` 속성을 추가하면 로드된 자바스크립트 파일이 모듈로서 동작한다. 자바스크립트 파일 확장자를 `.mjs`로 하면 ESM임을 명시해줄 수 있다.

```html
<script type="module" src="app.mjs"></script>
```

기본적으로 strict mode가 적용된다.

### 모듈 스코프

독자적인 모듈 스코프를 갖는다.

모듈 내에서 `var` 키워드로 선언한 변수는 전역 변수가 아니며 `window` 객체의 프로퍼티도 아니다.

### `export` 키워드

`export` 키워드는 선언문 앞에 사용한다. 변수, 함수, 클래스 등 모든 식별자를 export할 수 있다.

```js
// lib.mjs
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}

// 클래스의 공개
export class Person {
  constructor(name) {
    this.name = name;
  }
}
```

하나의 객체로 묶어 export할 수 있다.

```js
// lib.mjs
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

### `import` 키워드

다른 모듈이 export한 식별자 이름으로 import하며 ESM의 경우 파일 확장자를 생략할 수 없다.

```js
// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import한다.
// ESM의 경우 파일 확장자를 생략할 수 없다.
import { pi, square, Person } from "./lib.mjs";

console.log(pi); // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person("Lee")); // Person { name: 'Lee' }
```

의존성은 `script` 태그로 로드하지 않아도 된다.

하나의 이름으로 한 번에 `import`할 수 있다. 이때 `as` 뒤에 지정한 이름의 객체에 프로퍼티로 할당된다.

```js
// app.mjs
// lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import한다.
import * as lib from "./lib.mjs";

console.log(lib.pi); // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person("Lee")); // Person { name: 'Lee' }
```

이름을 변경해서 import할 수 있다.

```js
// app.mjs
// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import한다.
import { pi as PI, square as sq, Person as P } from "./lib.mjs";

console.log(PI); // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P("Kim")); // Person { name: 'Kim' }
```

모듈에서 하나의 값만 export한다면 `default` 키워드를 사용한다. 이 경우 `var`, `let`, `const` 키워드는 사용할 수 없다.

```js
// lib.mjs
export default (x) => x * x;
```

`default` 모듈은 `{}` 없이 임의의 이름으로 import한다.

```js
// app.mjs
import square from "./lib.mjs";

console.log(square(3)); // 9
```
