# 41장 타이머

## 41.1 호출 스케줄링

함수를 일정 시간이 지난 이후에 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용한다. 이를 호출 스케줄링이라 한다.

자바스크립트는 타이머를 생성할 수 있는 타이머 함수 `setTimeout`과 `setInterval`, 타이머를 제거할 수 있는 타이머 함수 `clearTimeout`과 `clearInterval`을 제공한다.

타이머 함수는 ES 사양에 정의된 빌트인 함수가 아니다. 하지만 브라우저와 Node.js 환경에서 모두 전역 객체의 메서드로서 제공한다. 즉, 타이머 함수는 호스트 객체다.

`setTimeout`과 `setInterval` 모두 일정 시간 이후 콜백 함수를 호출하도록 타이머를 생성한다.

`setTimeout`은 타이머 종료 시 단 한 번만 함수를 호출하고, `setInerval`은 타이머에 설정한 시간마다 반복 동작한다.

자바스크립트 엔진은 싱글 스레드로 동작한다. 때문에 타이머 함수는 비동기 처리 방식으로 동작한다.

## 41.2 타이머 함수

### `setTimeout` / `clearTimeout`

`setTimeout` 함수는 두 번째 인수로 전달받은 시간(ms)으로 단 한 번 동작하는 타이머를 생성한다.

```js
const timeoutId = setTimeout(func|code[, delay, param1, param2, ...])
```

| 매개변수                | 설명                                                                                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `func`                  | 타이머가 만료된 후 호출될 콜백 함수<br>※ 함수 대신 코드 문자열을 전달할 수도 있으나, 이는 `eval`처럼 동작하므로 **권장되지 않음**                  |
| `delay`                 | 타이머 만료 시간 (밀리초 단위)<br>기본값은 `0`이며, 최소 지연 시간은 `4ms`로 지정됨<br>※ `delay`는 "실행 시점"이 아닌, "큐에 등록되는 지연 시간"임 |
| `param1`, `param2`, ... | 콜백 함수에 전달할 추가 인자들<br>※ 세 번째 인자부터 콜백 함수에 전달되며, IE9 이하에서는 인자를 전달할 수 없음                                    |

`setTimeout` 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 `id`를 반환한다. `id`는 브라우저의 경우 숫자로, Node.js 환경인 경우 객체로 반한한다.

`clearTimeout` 함수에 타이머 `id`를 인수로 전달하면 타이머를 취소할 수 있다.

### `setInterval` / `clearInterval`

`setInterval` 함수는 두 번째 인수로 전달받은 시간(ms)으로 반복 동작하는 타이머를 생성한다.

전달할 인수는 `setTimeout`과 동일하다.

`setInterval` 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 `id`를 반환한다. `id`는 브라우저의 경우 숫자로, Node.js 환경인 경우 객체로 반한한다.

`clearInterval` 함수에 타이머 `id`를 인수로 전달하면 타이머를 취소할 수 있다.

## 41.3 디바운스와 스로틀

`scroll`, `resize`, `input`, `mousemove` 같은 이벤트는 짧은 시간 간격으로 여러번 발생한다. 이러한 이벤트에 바인딩된 이벤트 핸들러가 과도하게 호출되면 성능에 문제를 일으킨다.

디바운스와 스로틀은 **짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러 호출을 방지하는 프로그래밍 기법**이다.

- 디바운스: 연속된 호출에 대해, 마지막 호출로부터 일정 시간 경과 후 핸들러 호출
- 스로틀: 연속된 호출에 대해, 일정 시간 간격 동안 발생한 이벤트 호출을 무시하고 최대 한 번만 핸들러 호출

디바운스와 스로틀 구현에는 타이머 함수가 사용된다.

### 디바운스

디바운스(debounce)는 **짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후 이벤트 핸들러가 한 번만 호출**되도록 한다.

즉, 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 한다.

`input` 이벤트가 짧은 시간 간격으로 연속해서 발생하는 경우를 살펴보자.

```html
<!DOCTYPE html>
<html>
  <body>
    <input type="text" />
    <div class="msg"></div>
    <script>
      const $input = document.querySelector("input");
      const $msg = document.querySelector(".msg");

      const debounce = (callback, delay) => {
        let timerId;
        // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
        return (...args) => {
          // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.
          // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
          if (timerId) clearTimeout(timerId);
          timerId = setTimeout(callback, delay, ...args);
        };
      };

      // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
      // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
      // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출된다.
      $input.oninput = debounce((e) => {
        $msg.textContent = e.target.value;
      }, 300);
    </script>
  </body>
</html>
```

사용자가 입력을 멈추면(일정 시간 텍스트 입력 필드에 값을 입력하지 않으면) 입력 완료로 간주하고 타이머의 딜레이만큼 시간이 지난 뒤 메시지의 `textContent`에 `value` 값을 할당한다.

디바운스는 `resize` 이벤트 처리나 `input` 요소에 입력된 값으로 `ajax` 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하다.

위 코드는 완벽하지 않은 구현이다. 실무에서는 Underscore나 Lodash의 `debounce` 함수를 사용하는 것을 권장한다. 해당 라이브러리들은 더 많은 기능을 제공한다.

### 스로틀

스로틀(throttle)은 **짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출**되도록 한다.

즉, 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

`scroll` 이벤트가 짧은 시간 간격으로 연속해서 발생하는 경우를 살펴보자.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .container {
        width: 300px;
        height: 300px;
        background-color: rebeccapurple;
        overflow: scroll;
      }

      .content {
        width: 300px;
        height: 1000vh;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="content"></div>
    </div>
    <div>
      일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
      <span class="normal-count">0</span>
    </div>
    <div>
      스로틀 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
      <span class="throttle-count">0</span>
    </div>

    <script>
      const $container = document.querySelector(".container");
      const $normalCount = document.querySelector(".normal-count");
      const $throttleCount = document.querySelector(".throttle-count");

      const throttle = (callback, delay) => {
        let timerId;
        // throttle 함수는 timerId를 기억하는 클로저를 반환한다.
        return (...args) => {
          // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
          // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정한다.
          // 따라서 delay 간격으로 callback이 호출된다.
          if (timerId) return;
          timerId = setTimeout(() => {
            callback(...args);
            timerId = null;
          }, delay);
        };
      };

      let normalCount = 0;
      $container.addEventListener("scroll", () => {
        $normalCount.textContent = ++normalCount;
      });

      let throttleCount = 0;
      // throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
      $container.addEventListener(
        "scroll",
        throttle(() => {
          $throttleCount.textContent = ++throttleCount;
        }, 100)
      );
    </script>
  </body>
</html>
```

설정한 시간이 경과하기 전에 이벤트가 발생하면 아무것도 하지 않다가 `delay` 시간이 경과했을 때 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머를 재설정한다.

스로틀은 `scroll` 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용된다.
