# 47.1 에러 처리의 필요성

---

- 발생한 에러에 대처하지 않고 방치하면 프로그램은 강제 종료된다.
- try-catch 문을 사용해 적절하게 대응해 종료되지 않고 실행할 수 있도록 가능하다.
  ![image.png](attachment:5e98920a-5031-4a2a-8670-291c30e3cada:image.png)
- 직접적으로 에러를 발생시키지는 않는 예외적인 상황이 발생할 수도 있다.
  ![image.png](attachment:36d54e6c-e111-4c5a-89a9-f0b06b839734:image.png)
- 그렇기에 언제나 예외적인 상황이 발생할 수 있다는 것을 전제하고 이에 대응하는 코드를 작성하는 것이 중요하다.

# 47.2 try … catch … finally 문

---

- 기본적으로는 null 또는 -1 을 단축 평가나 옵셔널 체이닝으로 처리한다.
- try … catch … finally 문 은 두 번째 방법이다. finally 생략 가능.
  ![image.png](attachment:fa8f074a-ed18-4ebf-88cf-a18bf6789861:image.png)
  ![image.png](attachment:6ad9343b-2110-40bb-887d-f030152d18ba:image.png)

# 47.3 Error 객체

---

- Error 생성자 함수는 에러 객체를 생성한다.
- 생성자 함수에는 에러를 상세히 설명하는 에러 메세지를 인수로 전달할 수 있다.
  ![image.png](attachment:5f934be7-eea4-439c-892e-7c8e26ec53ff:image.png)
- **Error 생성자 함수가 생성한 여러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.** ❗ **message 프로퍼티 값은 인수로 전달된 에러 메세지이고,** ❗**stack 프로퍼티의 값은 에러를 발생시킨 콜 스택의 호출 정보를 나타내는 문자열이며 디버깅 목적으로 사용한다.**
- 자바스크립트는 ERROR 생성자 함수를 포함해 7가지의 에러 객체를 생성할 있는 생성자 함수를 제공한다.
- 모든 에러는 Error.prototype 을 상속받는다.
  ![image.png](attachment:ee2b9959-2961-459e-ae52-3122fa803285:image.png)

# 47.4 throw 문

---

- Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니고 **try 문 안에서 throw 를 해서 던져야 한다.**
  ![image.png](attachment:35cf2dea-4dc1-4b7f-9245-3ef0054d33bd:image.png)
- 아래와 같은 코드도 만들 수 있다.
  ![image.png](attachment:f95a268a-d03f-4b86-9876-4baf4701673d:image.png)

# 47.5 에러의 전파

---

- 에러는 호출자 방향으로 전파한다.
  ![image.png](attachment:d5685012-0596-4d95-ad6a-f15c8d84caa2:image.png)
- 그림
  ![image.png](attachment:6a69af50-a549-4418-bbae-921e7b08b208:image.png)
- 만약 throw 된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다.
- ❗**주의할 점은 비동기 함수인 setTimeout 이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 점이다.**
- 그럼 어떻게 해요.. ㅠㅠ
  ### ✅ `setTimeout`의 콜백 함수 내부에서 발생한 에러는 **전통적인 `try-catch`로는 외부에서 잡을 수 없고**,
  **콜백 내부에서 직접 `try-catch`로 감싸거나**,
  **전역 에러 핸들러를 이용해야 합니다.**
  ***
  ## ✅ 1. 콜백 내부에서 직접 처리
  ```
  js
  복사편집
  setTimeout(() => {
    try {
      throw new Error("setTimeout 내부 에러");
    } catch (e) {
      console.error("잡았다!", e.message);
    }
  }, 0);

  ```
  > 이건 정상적으로 에러를 잡습니다. 왜냐면 catch가 콜백 실행 시점에 같이 존재하니까요.
