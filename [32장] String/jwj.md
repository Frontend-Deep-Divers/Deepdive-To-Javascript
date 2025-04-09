# 32장 String

표준 빌트인 객체 `String`에 대해서 다룬다.

## 32.1 `String` 생성자 함수

표준 빌트인 객체 `String`은 생성자 함수 객체다. 따라서 `new` 연산자와 함께 호출하여 `String` 인스턴스를 생성할 수 있다.

인수를 전달하지 않고 `new` 연산자와 함께 호출하면 `[[StringData]]` 내부 슬롯에 빈 문자열을 할당한 `String` 래퍼 객체를 생성한다.

```js
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}
```

ES5에서는 `[[StringData]]`를 `[[PrimitiveValue]]`라 불렀다.

문자열 인수를 전달하면 내부 슬롯에 해당 문자열을 할당한 `String` 래퍼 객체를 생성한다.

`String` 래퍼 객체는 배열과 마찬가지로 유사 배열 객체면서 이터러블이다. 따라서 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.

단, 문자열은 원시 값이므로 변경할 수 없다. 이때 에러도 발생하지 않는다.

```js
str = "Jeon";
str[0] = "S";
conslelog(str); // Jeon
```

인수로 문자열이 아닌 값을 전달하면 문자열로 강제 변환한다.

`new` 연산자를 사용하지 않고 호출하면 `String` 인스턴스가 아닌 원시 값 문자열을 반환한다.

## 32.2 `length 프로퍼티`

해당 문자열의 문자 개수를 반환한다.

```js
`Jeon`.length; // 4
```

## 32.3 `String` 메서드

배열과 다르게 `String` 객체는 원본 `String` 래퍼 객체를 직접 변경하는 메서드가 없다. 언제나 새로운 문자열을 반환한다. 문자열은 불변한 원시 값이기 때문에 `String` 래퍼 객체도 읽기 전용 객체로 제공된다.

```js
const strObj = new String("Jeon");

console.log(Object.getOwnPropertyDescriptors(strObj));
/* String 래퍼 객체는 읽기 전용 객체다. 즉, writable 프로퍼티 어트리뷰트 값이 false다.
{
  '0': { value: 'L', writable: false, enumerable: true, configurable: false },
  '1': { value: 'e', writable: false, enumerable: true, configurable: false },
  '2': { value: 'e', writable: false, enumerable: true, configurable: false },
  length: { value: 3, writable: false, enumerable: false, configurable: false }
}
*/
```

만약 `String` 래퍼 객체가 읽기 전용 객체가 아니라면 변경된 `String` 래퍼 객체를 문자열로 되돌릴 때 문자열이 변경된다.

### `String.prototype.indexOf`

문자열에서 인수로 전달받은 문자열이 나오는 첫 번째 인덱스를 반환한다. 없으면 -1을 반환한다.

2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

### `String.prototype.search`

문자열에서 인수로 전달받은 정규 표현식과 매칭되는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다. 없으면 -1을 반환한다.

### `String.prototype.includes`

문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 불리언 값을 반환한다.

### `String.prototype.startsWith`

문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 불리언 값을 반환한다.

2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

### `String.prototype.endsWith`

문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 불리언 값을 반환한다.

2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

### `String.prototype.charAt`

문자열에서 인수로 전달받은 인덱스의 문자를 반환한다. 문자열의 인덱스를 벗어나면 빈 문자열을 반환한다.

### `String.prototype.substring`

문자열에서 첫 번째 인수부터 두 번째 인수 바로 직전까지의 문자를 반환한다.

- 첫 번째 인수 > 두 번째 인수의 경우 서로 교환된다.
- 인수 < 0 또는 `NaN`인 경우 0으로 취급된다.
- 인수 > 문자열 길이(`str.length`)인 경우 인수는 문자열의 길이(`str.length`)로 취급된다.

### `String.prototype.slice`

`substring` 메서드와 동일하게 동작한다. 단, 음수인 인수를 전달할 수 있다. 움수인 인수는 문자열 맨 뒤부터 시작한다.

### `String.prototype.toUpperCase`

문자열을 모두 대문자로 변경한 문자열을 반환한다.

### `String.prototype.toLowerCase`

문자열을 모두 소문자로 변경한 문자열을 반환한다.

### `String.prototype.trim`

대상 문자열 앞뒤 공백 문자를 제거한 문자열을 반환한다.

### `String.prototype.repeat`

대상 문자열을 인수 만큼 반복해 연결한 새로운 문자열을 반환한다. 기본값은 0이며 음수이면 `RangeError`를 발생시킨다.

### `String.prototype.replace`

첫 번째 인수로 전달받은 문자열 또는 정규 표현식을 검색해 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환한다.

특수한 교체 패턴을 사용할 수 있다. 자세한 내용은 MDN의 설명 참고.

```js
const str = "Hello world";

// 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
str.replace("world", "<strong>$&</strong>");
```

두 번째 인수로 치환 함수를 전달할 수 있다.

```js
// 카멜 케이스를 스네이크 케이스로 변환하는 함수
function camelToSnake(camelCase) {
  // /.[A-Z]/g는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
  // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  return camelCase.replace(/.[A-Z]/g, (match) => {
    console.log(match); // 'oW'
    return match[0] + "_" + match[1].toLowerCase();
  });
}

const camelCase = "helloWorld";
camelToSnake(camelCase); // -> 'hello_world'

// 스네이크 케이스를 카멜 케이스로 변환하는 함수
function snakeToCamel(snakeCase) {
  // /_[a-z]/g는 _와 소문자로 이루어진 문자열에 매치한다.
  // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  return snakeCase.replace(/_[a-z]/g, (match) => {
    console.log(match); // '_w'
    return match[1].toUpperCase();
  });
}

const snakeCase = "hello_world";
snakeToCamel(snakeCase); // -> 'helloWorld'
```

### `String.prototype.split`

첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.

빈 문자열을 인수로 전달하면 각 문자를 모두 분리한 배열을 반환한다.

인수를 생략하면 문자열 전체를 단일 요소로 하는 배열을 반환한다.

두 번째 인수로 배열의 길이를 지정할 수 있다.

💡`reverse`, `join` 메서드를 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

```js
function reverseString(str) {
  return str.split("").reverse().join("");
}
```
