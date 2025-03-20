# 일급 객체

다음과 같은 조건을 만족하는 객체를 일급 객체라 함

1. 무명의 리터럴로 생성할 수 있음 → 런타임에 생성이 가능
2. 변수나 자료구조에 저장할 수 있음
3. 함수의 매개변수에 전달할 수 있음
4. 함수의 반환값으로 사용할 수 있음

자바스크립트의 함수는 일급 객체

# 함수 객체의 프로퍼티

```jsx
function square(num) {
  return num * num;
}

console.log(Object.getOwnPropertyDescriptors(square));

```

```jsx
{
  length: { value: 1, writable: false, enumerable: false, configurable: true },
  name: {
    value: 'square',
    writable: false,
    enumerable: false,
    configurable: true
  },
  arguments: {
    value: null,
    writable: false,
    enumerable: false,
    configurable: false
  },
  caller: {
    value: null,
    writable: false,
    enumerable: false,
    configurable: false
  },
  prototype: { value: {}, writable: true, enumerable: false, configurable: false }
}
```

### `arguments`

함수 호출 시 전달된 인수들의 정보를 담고 있는 Iterable 유사 배열 객체 → 배열 메서드를 사용할 수 없음 → ES6부터 Rest 파라미터의 도입으로 해결

함수 내부에서만 참조 가능

초과된 인수는 버려지지 않고 암묵적으로 `arguments` 객체의 프로퍼티로 보관됨

`arguments.callee` : `arguments` 객체를 생성한 함수, 즉 함수 자신

### `caller`

비표준 프로퍼티

함수 자신을 호출한 함수

### `length`

함수를 정의할 때 선언한 매개변수의 개수 ≠ `arguments.length` ( 인자의 개수 )

### `name`

함수의 이름 ( ≠ 함수 객체를 가리키는 식별자 )

ES6 이후 표준

- ES5: 익명 함수 표현식의 경우 빈 문자열
- ES6: 익명 함수 표현식의 경우 함수 객체를 가리키는 식별자

### `__proto__` 접근자 프로퍼티

`Object.prototype` 으로부터 상속받는 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지는데, 상속을 구현하는 프로토타입 객체를 가리킴

`__proto__` 접근자 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

### `prototype`

함수가 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴

constructor만이 소유함
