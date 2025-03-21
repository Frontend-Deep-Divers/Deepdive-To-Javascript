# 15.1 var 키워드로 선언한 변수의 문제점

---

- var 키워드는 일반적인 다른 언어들과는 다른 특징들이 존재한다.

1. **변수 중복 선언 허용**

   - 중복 선언이 되기 때문에 모르고 중복 선언하고 할당해버리면 예상치 못한 문제 발생할 수 있음.
   - 초기화 문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화 문이 없는 변수 선언문은 무시된다.

   ```jsx
   var x = 1;
   var y = 1;

   var x = 100;
   var y;

   console.log(x); // 100
   console.log(y); // 1
   ```

1. **함수 레벨 스코프**

   - var 키워드 변수는 오직 함수의 코드 블록 만을 지역 스코프로 인정한다.
   - 의도치 않게 중복 선언하는 경우 많을 것이다.

1. **변수 호이스팅**
   - var 키워드 변수는 변수 호이스팅에 의해 스코프 선두로 끌어올려 진 것처럼 동작한다. 할당 이전에 참조하면 undefined 반환
   - 변수 선언문 이전에 변수를 참조하는 것은 에러를 발생 시키지는 않지만, 프로그램 흐름상 맞지 않을 뿐더러 가독성을 떨어뜨리고 오류를 발생시킨다.

# 15.2 let 키워드

---

1. **변수 중복 선언 금지**

   - 중복 선언을 방지한다. 중복 선언 시 문법 에러 발생

1. **블록 레벨 스코프**

   - 일반적인 다른 프로그래밍 언어처럼 블록으로 스코프가 구분된다.

1. **변수 호이스팅**

   - let 으로 선언한 변수는 선언문 이전에 참조하면 참조 에러가 발생한다.
   - 🌟 var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 ‘선언 단계’와 ‘초기화 단계’가 한번에 진행된다.
   - 🌟**이에 반해 let 키워드는 선언 단계와 초기화 단계가 분리되어 진행된다.**
   - 초기화 단계 이전에 변수에 접근하려고 하면 참조 에러가 발생한다. 스코프의 시작 지점부터 초기화 단계(변수 선언문)까지 변수를 참조할 수 없다. 이 구간을 **일시적 사각지대** **( TDZ )** 라고 부른다. → 이래서 호이스팅이 발생하지 않은 것처럼 보인다. 하지만 그렇지 않다!

   ```jsx
   let var1 = 1;
   {
     console.log(var1); // referenceError
     let var1 = 2;
   }
   ```

   - 🌟🌟🌟만약 호이스팅이 발생하지 않았다면 참조 에러가 아니라 1이 나와야 한다. 하지만 호이스팅은 되므로 참조 에러가 발생하는 것!

1. **전역 객체와 let**
   - var 키워드로 선언한 전역 변수와 함수는 전역 객체 window 의 프로퍼티가 된다.
   - let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
   - ❗let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 에 존재하게 된다.

# 15.3 const 키워드

---

- const 키워드는 대게 상수를 선언하기 위해 사용한다. 하지만 상수만을 위해 사용하지 않는다~~

1. **선언과 초기화**

   - 선언과 동시에 반드시 초기화해야 한다.
   - 그렇지 않으면 문법 에러
   - let 과 비슷하게 호이스팅이 발생하지 않는 것처럼 동작한다. 물론 실제로 동작은 함.

1. **재할당 금지**

   - const 는 재할당이 불가능하다.

1. **상수**

   - const 에 원시 값을 할당하면 변경할 수 없기에 상수가 된다.
   - 상수의 이름은 대게 대문자 스네이크 케이스

1. **const 키워드와 객체**
   - const 에 원시 값이 아닌 객체를 할당하는 경우 변경이 가능하다.
   - 즉, const 가 불변은 아니라는 소리.

# 15.4 var vs let,const

---

- 아래와 같은 규칙을 사용하길 권장한다.
  - ES6를 사용한다면 var 키워드는 사용 X
  - 재할당이 필요한 경우에만 let 을 사용하고, 가급적이면 const 사용
  - 객체에는 const 사용, 읽기 전용 원시값도 const 사용
- 변수를 선언할 때 재할당이 필요할지 잘 모르겠는 경우가 있는데 이 때는 일단 const 키워드를 사용하자.
