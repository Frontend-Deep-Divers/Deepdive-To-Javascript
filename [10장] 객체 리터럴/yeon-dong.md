# 10장 객체 리터럴

## 10.1 객체란?

- 자바스크립트에서는 원시 값을 제외한 나머지 값(함수, 배열, 정규표현식 등)은 모두 객체이다.
- 원시 값은 변경 불가능하지만 객체 타입의 값은 변경 가능하다.
```jsx
var person = {
    name: 'Lee',
    age: 20
};
```
- 위와 같이 객체는 프로퍼티 키 : 프로퍼티 값으로 구성되며 여기에 함수가 추가 되면 메서드라고 부른다.


## 10.2 객체 리터럴에 의한 객체 생성

- 자바스크립트는 프로토타입 기반 객체지향 언어로서 아래와 같은 다양한 객체 생성 방법을 지원함.
    - 객체 리터럴
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스(ES6)
- 리터럴은 사람이 이해할 수 있는 문자나 약속된 기호를 사용하여 값을 생성하는 것을 의미.
- 객체 리터럴은 중괄호({...})내에 0개 이상의 프로퍼티를 정의함. -> js엔진은 변수 할당되는 시점에 객체 리터럴을 해석해 객체 생성
- new 연산자와 함께 생성자를 호출할 필요 없고 그냥 객체를 만들 수 있다.

## 10.3 프로퍼티

- **객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.**
- firstName과 같은 식별자 네이밍 규칙을 준수하는 프로퍼티 키를 사용할 것을 권장.
- first-name과 같이 쓰려면 'first-name'과 같이 따옴표로 써야함. -> 이렇게 하지 않으면 - 연산자가 있는 것으로 파악됨.
- 빈 문자열을 프로퍼티 키로도 쓸 수 있고(권장 x) 동적으로 obj[key] = "world";와 같이 생성도 가능.
- 예약어도 권장하지 않고, 같은 이름의 프로퍼티를 중복 선언 하면 제일 뒤가 덮어 쓰게 됨.

## 10.4 메서드

- 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 **메서드**라고 부름.

## 10.5 프로퍼티 접근

- 프로퍼티에 접근하는 방법은 2가지
    - 마침표 프로퍼티 접근 연산자 (`.`)
    - 대괄호 프로퍼티 접근 연산자 (`[ … ]`)
- 대괄호 프로퍼티 접근 연산자 내부에 사용하는 프로퍼티 키는 반드시 **따옴표**로 감싼 문자열이여야 한다.
    - 안하면 ReferenceError 발생, 없는 식별자이기 때문에
- 객체에 존재하지 않는 프로퍼티 접근 시 undefined를 반환.
- 네이밍 준수하지 않는 first-name 과 같은 경우 무조건 대괄호 프로퍼티 접근 연산자를 이용해야 함.

## 10.6 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가된다.

## 10.8 프로퍼티 삭제

- `delete` 연산자는 객체의 프로퍼티를 삭제한다. 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다.
- 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 **무시**된다.

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

- 원래 ES5에서는 x: x, y: y 이런 식으로 선언했어야 했지만, ES6부터는 축약 표현 가능
```jsx
let x = 1, y = 2;

const obj = {x, y}; //프로퍼티 축약 표현

console.log(obj); // {x: 1, y: 2}
```
    
- ES5에서는 계산된 프로퍼티 이름을 쓰고 싶으면 이미 만들어진 객체에 대괄호 표기법으로 동적으로 만들었어야 함.
- 하지만 ES6부터는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 동적 생성 가능!

```jsx
// ES5
obj[prefix + "-" + ++i] = i;

// ES6
const obj = {
    [`${prefix}-${++i}`]: i
};
```

- 메서드 축약 표현도 아래와 같이 가능하다
```jsx
//ES5
var obj = {
    print: function() {
        console.log("hi");
    }
}

//ES6
const obj = {
    print() {
        console.log("hi");
    }
}
```