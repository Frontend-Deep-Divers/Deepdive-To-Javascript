# 49장 Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

대부분의 브라우저의 ES6 지원율은 약 98%인 반면 IE 11의 ES6 지원율은 약 11%다.

따라서 ES6+와 ES 제안 사양(ES.NEXT)의 최신 ES 사양으로 프로젝트를 진행하려면 구형 브라우저에서 문제 없이 동작하기 위해 개발 환경을 구축할 필요가 있다.

모듈 로더도 역시 필요하다.

1. IE를 포함한 구형 브라우저는 ESM을 지원하지 않는다.
2. ESM을 사용해도 트랜스파일링, 번들링이 필요하다.
3. ESM이 지원하지 않는 기능(bare import)이 있고, 몇가지 이슈가 존재한다.

- bare import: 경로 없이 패키지 이름만 사용하는 import(브라우저 환경에서의 이슈)

  최신 브라우저는 `<script type="importmap">`을 통해 bare import를 지원하기도 한다.

  ```html
  <script type="importmap">
    {
      "imports": {
        "lodash": "/node_modules/lodash-es/lodash.js"
      }
    }
  </script>
  <script type="module">
    import { debounce } from "lodash";
  </script>
  ```

## 49.1 Babel

트랜스파일러인 Babel을 설치해서 사용해보자.

### Babel 설치

```shell
# 프로젝트 폴더 생성
mkdir esnext-project && cd esnext-project

# package.json 생성
npm init -y

# babel-core, babel-cli 설치
npm install --save-dev @babel/core @babel/
```

### Babel 프리셋 설치와 babel.config.json 설정 파일 작성

Babel을 사용하려면 Babel 플러그인을 모아둔 프리셋(`@babel/preset-env`)을 설치해야 한다. 공식 프리셋은 4가지가 있다.

- `@babel/preset-env`: 최신 ES6+ 문법을 현재 환경(브라우저나 Node.js)에 맞게 변환
- `@babel/preset-flow`: Flow 타입 문법을 제거하여 자바스크립트로 변환
- `@babel/preset-react`: JSX 및 React 관련 최신 문법을 일반 자바스크립트로 변환
- `@babel/preset-typescript`: TypeScript 코드를 자바스크립트로 변환

필요 플러그인을 프로젝트 지원 환경에 맞춰 동적으로 결정해준다. 프로젝트 지원 환경은 Browserlist 형식으로 `.browserlistrc` 파일에 상세히 설정할 수 있다. 설정 작업을 생략하면 기본값으로 설정된다.

```shell
npm i --save-dev @babel/preset-env
```

설치 후 프로젝트 루트 폴더에 `babel.config.json` 설정 파일을 생성하고 `@babel/preset-env`을 사용하게끔 작성한다.

```json
{
  "presets": ["@babel/preset-env"]
}
```

### 트랜스파일링

ES6+/ES.NEXT 사양의 코드를 ES5 사양의 코드로 트랜스파일링할 수 있다.

`package.json`에 `scripts`를 추가한다.

```json
"scripts": {
    "build": "babel src/js -w -d dist/js",
  },
```

> -w: 타깃 폴더의 변경을 감지하여 자동으로 트랜스파일링(`--watch` 옵션) </br>
> -d: 트랜스파일링 결과물을 저장할 폴더를 지정, 디렉토리가 존재하지 않으면 생성(`--out-dir` 옵션)

아래 명령어로 트랜스파일링한다.

```shell
npm run build
```

### Babel 플러그인 설치

현재 제안 단계에 있는 사양을 트랜스파일링하려면 별도의 플러그인을 설치해야 한다.

### 브라우저에서 모듈 로딩 테스트

다음은 트랜스파일링된 코드 예제다.

```js
"use strict";

var _lib = require("./lib");
console.log(_lib.pi);
console.log((0, _lib.power)(_lib.pi, _lib.pi));
var f = new _lib.Foo();
console.log(f.foo());
console.log(f.bar());
```

브라우저는 `require` 함수를 지원하지 않으므로 브라우저에서 위 코드를 실행하면 에러가 발생한다.

브라우저의 ES6 모듈(ESM)을 사용하도록 Babel을 설정할 수도 있으나 ESM을 사용하는 것은 여러 문제가 발생할 수 있다. 이러한 문제를 해결하기 위해 Webpack을 사용할 수 있다.

## 49.2 Webpack

Webpack은 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스를 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러다.

의존 모듈이 하나의 파일로 번들링되어 별도의 모듈 로더가 필요 없다.

자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 `script` 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움이 없다.

Webpack에서 번들링 전에 Babel을 로드하여 트랜스파일링하도록 설정해줄 수 있다.

### Webpack, babel-loader 설치

```shell
npm i --save-dev webpack webpack-cli babel-loader
```

`package.json`의 `scripts`도 수정해야 한다.

```json
{
  "scripts": {
    "build": "webpack -w"
  }
}
```

### webpack.config.js 설정 파일 작성

프로젝트 루트 폴더에 `webpack.config.js` 파일을 생성하고 작성한다.

```js
const path = require("path");

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: ["@babel/polyfill", "./src/js/main.js"],
  // 번들링된 js 파일의 이름(filename)과 저장될 경로(path)를 지정
  // https://webpack.js.org/configuration/output/#outputpath
  // https://webpack.js.org/configuration/output/#outputfilename
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "js/bundle.js",
  },
  // https://webpack.js.org/configuration/module
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [path.resolve(__dirname, "src/js")],
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
            plugins: ["@babel/plugin-proposal-class-properties"],
          },
        },
      },
    ],
  },
  devtool: "source-map",
  // https://webpack.js.org/configuration/mode
  mode: "development",
};
```

### babel-polyfill 설치

Babel을 사용해서 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아 있을 수 있다. 예를 들어, `Promise`, `Object.assign`, `Array.from` 등은 ES5 사양에 대체할 기능이 없어 트랜스파일링되지 못하고 그대로 남는다.

따라서 구형 브라우저에서도 트랜스파일링으로 대체할 수 없는 객체나 메서드를 사용하려면 `@babel/polyfill`을 사용할 수 있다.

```shell
npm i @babel/polyfill
```

실제 운영 환경에서도 사용해야 하므로 개발용 의존성으로 지정하지 않는다.

ES6의 import를 사용하려면 엔트리포인트의 최상단에서 먼저 폴리필을 로드한다.

```js
import "@babel/polyfill";
// 나머지 코드들...
```

Webpack을 사용하면 설정 파일의 `entry` 배열에 폴리필을 추가한다.

```js
const path = require('path');

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: ['@babel/polyfill', './src/js/main.js'],
...
```
