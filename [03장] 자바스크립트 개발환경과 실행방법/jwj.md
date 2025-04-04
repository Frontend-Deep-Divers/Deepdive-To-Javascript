# 3장 자바스크립트 개발 환경과 실행 방법

## 3.1 자바스크립트 실행 환경

**모든 브라우저**와 **Node.js**는 자바스크립트 엔진을 내장하고 있다. 따라서 자바스크립트를 실행하기 위해서는 이 두가지가 필요하다.

하지만 Node.js는 용도가 달라 자바스크립트 코어인 ECMAScript를 제외한 기능은 호환되지 않는다.

반대로 Node.js에서 제공되는 파일을 생성하고 수정할 수 있는 파일 시스템이 브라우저 환경의 자바스크립트에서는 보안 상의 이유로 제공하지 않는다.

&nbsp;

## 3.2 웹 브라우저

크롬 브라우저의 개발자 도구는 웹 개발에 유용한 다양한 기능을 제공한다.

콘솔(REPL)의 `Shift`를 사용해 자바스크립트 코드를 여러 줄 실행할 수 있다.

&nbsp;

## 3.3 Node.js

웹 어플리케이션의 프로젝트 규모가 커져 프레임워크나 라이브러리를 이용해야 할 땐 Node.js와 npm을 사용해야 할 때가 있다.

Node.js 설치 후 cmd에 `node` 명령어로 자바스크립트 코드를 실행할 수 있다.

명령어 뒤에 `.js` 파일명을 입력하면 자바스크립트 파일을 실행할 수 있다.

&nbsp;

## 3.4 비주얼 스튜디오 코드

VSCode의 내장 터미널에서 `node` 명령어를 실행하거나 `Code Runner` 익스텐션으로 단축키를 통해 자바스크립트 파일을 실행할 수 있다.

`Live Server` 익스텐션은 소스코드의 수정 사항이 있으면 브라우저에 자동으로 반영해준다.
