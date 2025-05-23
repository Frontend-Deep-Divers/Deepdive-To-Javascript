- 모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정되는데, 이를 스코프라고 함
    - 스코프: 식별자가 유효한 범위
- 자바스크립트 엔진은 어떤 변수를 참조해야 할 것인지를 결정해야 하는데 이를 식별자 결정이라고 함 → 식별자 결정을 할 때 사용하는 규칙을 스코프라고 할 수 있음
- 서로 다른 스코프에선 같은 이름의 변수를 사용할 수 있음 → 스코프는 네임스페이스라고 할 수 있음

# 스코프의 종류

## 전역 스코프

전역이란 코드의 가장 바깥 영역을 말함

전역 변수는 어디서든 참조할 수 있음

## 지역 스코프

지역이란 함수 몸체 내부를 말함

지역 변수는 자신이 선언된 지역과 하위 지역에서만 참조할 수 있음

# 스코프 체인

스코프는 함수의 중첩에 의해 계층적 구조를 가짐 → 최상위 스코프 = 전역 스코프

스코프가 계층적으로 연결된 구조를 스코프 체인이라고 함

자바스크립트 엔진은 변수를 참조할 때 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작해 상위 스코프 방향으로 이동하며 식별자 결정을 함

자바스크립트 엔진은 코드를 실행하기 앞서 먼저 렉시컬 환경을 생성하는데, 모든 변수 식별자가 렉시컬 환경에 저장되고 변수 검색도 렉시컬 환경에서 이뤄진다

# 함수 레벨 스코프

`var` 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정함 → 함수 레벨 스코프

# 렉시컬 스코프

함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정됨 → 함수의 상위 스코프는 언제나 자신이 정의된 스코프

함수 객체는 상위 스코프를 기억하고 있음 → 클로저가 발생하는 원인!
