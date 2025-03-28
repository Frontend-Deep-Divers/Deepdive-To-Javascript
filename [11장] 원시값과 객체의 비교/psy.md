# 원시 타입 vs 객체 타입

- 원시 타입
    - 변경 불가능한 값 (변수가 아닌 값을 변경할 수 없음 → 재할당은 가능)
    - 재할당 시 새 메모리 공간에 새로운 값을 저장함 → 불변성
    - 확보된 메모리 공간에 실제 값이 저장됨
    - 원시 값 변수를 다른 변수에 할당하면 원시 값이 복사되어 전달 pass by value
- 객체 타입
    - 변경 가능한 값
    - 확보된 메모리 공간에 참조 값이 저장됨
    - 다른 변수에 할당하면 참조 값이 복사되어 전달 pass by reference

# 문자열과 불변성

문자열도 원시 타입이기 때문에 변경이 불가능, 재할당 시 다른 메모리 공간에 새 값을 저장함

유사 배열 객체 + 이터러블

유사 배열 객체: 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체

래퍼 객체로 인해 객체처럼 동작

```jsx
let str = 'string';

str[0] = 'S' // 오류가 발생하진 않지만 변경도 되지 않음
str = 'new string' // 재할당은 가능
```

## 값에 의한 전달

```jsx
const score = 100;
const copy = score;
```

위 코드의 경우 두 가지 평가가 가능함

1. 새로운 100을 메모리 공간에 할당한 후 주소를 전달
2. score의 값 100의 메모리 주소를 그대로 전달

값에 의한 전달도 사실은 **메모리 주소를 전달**함, 하지만 100 자체가 변경 불가능하고, 재할당 시 새로운 메모리 공간이 할당되기 때문에 서로 간섭할 수 없음

# 객체

객체의 크기는 동적이기 때문에 메모리 공간의 크기를 사전에 정할 수 없음

대부분의 자바스크립트 엔진은 해시 테이블과 유사하지만 더 나은 방법으로 객체를 구현함

객채 생성 비용이 크기 때문에 객체는 변경 가능한 값으로 설계되어 있음 → 여러 식별자가 하나의 객체를 공유할 수 있음

## 참조에 의한 전달

객체를 가리키는 변수를 다른 변수에 할당하면 참조 값이 복사되어 전달됨 → 두 식별자가 하나의 객체를 공유

원시 타입도 메모리 공간에 저장된 원시 값을 전달할 뿐이고, 객체 타입도 메모리 공간에 저장된 참조 값을 전달할 뿐이므로 둘 다 값에 의한 전달이라고 볼 수 있음
