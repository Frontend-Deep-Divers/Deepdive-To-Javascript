# 33장 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

- ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값
- 다른 값과 중복되지 않는 유일무이한 값이며 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용됨

## 33.2 심벌 값의 생성

- Symbol 함수
    - 심벌 값은 Symbol 함수를 호출하여 생성해야한다.
    - 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.
    - new 연산자 없이 호출하며 객체를 생성한다.
    - 선택적으로 문자열을 인수로 전달할 수 있는데 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용된다.
    - 심벌값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않지만 불리언 타입으로는 암묵적으로 타입 변환된다.
- Symbol.for 메서드
    - 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
    - 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다
    - 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.
    - Symbol 함수는 호출될 때마다 유일무이한 심벌 값을 생성하는데 이 경우 자바 스크립트 엔진이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없어 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
    - Symbol.for 메서드를 통해 애플리케이션 전역에서 중복되는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.
- Symbol.keyFor
    - 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

## 33.3 심벌과 상수

- 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

## 33.4 심벌과 프로퍼티 키

- **심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**

## 33.5 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for…in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없다.
- 이를 통해 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

## 33.6 심벌과 표준 빌트틴 객체 확장

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 추가하여 확장하는 것은 권장되지 않는다.
(미래에 표준으로 추가될 메서드와 사용자가 직접 추가한 메서드의 이름이 중복될 수 있음)
- 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 객체의 기존 프로퍼티 키와 충돌하지 않으며 앞으로 추가될 프로퍼티 키와도 충돌할 우려가 없다.

## 33.7 Well-known Symbol

- 자바 스크립트가 기본 제공하는 빌트인 심벌값