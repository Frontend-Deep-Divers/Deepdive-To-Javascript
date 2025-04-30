# 41.1 호출 스케줄링

---

- 함수를 명시적으로 호출하면 즉시 실행.
- 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용한다. → 이를 호출 스케줄링이라 한다.
- 브라우저와 Node.js 환경 모두에서 적역 객체의 메서드로서 타이머 함수를 제공한다. 즉 호스트 객체
- setTimeout, setInterval 함수 → 타이머 마다 콜백 함수 호출
- clearTimeout, clearInterval 함수

# 41.2 타이머 함수

---

**1️⃣ setTimeout, clearTimeout**

- 인수로 전달받은 시간을 단 한 번 동작하는 타이머를 생성한다.
- 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출됨.
- 단 한 번 실행되도록 호출 스케줄링된다.
- 3번째 인수로 콜백 함수의 인수를 전달할 수 있다.
  ![image.png](attachment:f1c44998-b445-4398-bd52-0e4642df7cae:image.png)
- 두 번째 인수를 생략하면, 기본값은 0 이다.
- **setTimeout 함수는 타이머를 식별할 수 있는 고유한 타이머 id 를 반환하는데,** ⚠️**브라우저 환경인 경우 숫자이며, Node.js 환경인 경우 객체이다.
  →** 🌟 **타입 스크립트 이야기**
- clearTimeout 함수의 인수로 id 를 전달하면 타이머를 취소할 수 있다. 즉, clearTimeout 함수는 호출 스케줄링을 취소한다.
  ![image.png](attachment:ab455ebe-dc02-4419-9340-3faebebacd85:image.png)

**2️⃣ setInterval / clearInterval**

- 전달받은 시간으로 반복 동작하는 타이머 생성.
- 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다.
- 취소될 때까지 호출된다.
- 인수는 동일하다.
- 역시 고유한 타이머 id 를 반환하고, 브라우저에서는 숫자, Node.js 에서는 객체이다.
- clearTimeInterval 에 인수로 넘겨 취소 가능하다.
  ![image.png](attachment:e23f0b02-7079-43bf-8785-458d74b66429:image.png)

# 41.3 디바운스와 스로틀

---

- scroll, resize, input, mosemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생하는데, 이 때 이벤트에 바인딩한 핸들러가 과도하게 호출되어 성능에 문제를 일으킬 수 있다.
- 이걸 보완해주는 두 가지 기법이다. 내부 구현은 처음 본다…
  ![image.png](attachment:6c1ebecd-51d7-4c6b-8faa-8c86b6acafab:image.png)

**1️⃣ 디바운스**

- 연속으로 발생하면 계속 호출하지 않다가 일정 시간 경과한 이후 한 번 만 호출하는 기법.
- 여러 번 중 마지막
  ![image.png](attachment:63527c29-1af1-4dd8-800b-ac44abe57bfa:image.png)
- 디바운스는 resize 이벤트 처리나 input 요소 등 입력필드 자동완성 UI, 버튼 중복 클릭 방지 처리에 유용하게 사용된다.
- 예제는 완전하지 않기에 Underscores 나 Lodash 의 debounce 를 사용하는 것을 권장한다.

**2️⃣ 스로틀**

- 짧은 간격으로 계속 이벤트가 발생하더라도 일정 시간 간격으로 이벤트 호출하는 기법.
  ![image.png](attachment:72a0bc7f-1d60-4f79-b860-0b5b9c3b1257:image.png)
- 무한 스크롤이나 scroll 이벤트 처리에 유용하게 사용된다.
- 예제는 완전하지 않기에 Underscores 나 Lodash 의 throttle을 사용하는 것을 권장한다.
