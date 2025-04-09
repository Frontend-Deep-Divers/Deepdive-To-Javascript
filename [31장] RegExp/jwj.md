# 31장 RegExp

## 31.1 정규 표현식이란?

정규 표현식(regular expression)은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)다. 자바스크립트는 Perl의 정규 표현식 문법을 ES3부터 도입했다.

정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공한다. 특정 패턴과 일치하는 문자열을 검색, 추출 또는 치환할 수 있는 기능이다.

## 31.2 정규 표현식의 생성

자바스크립트에선 정규 표현식 객체를 리터럴 방식과 `RegExp` 생성자 함수를 사용하여 생성할 수 있다.

정규 표현식 리터럴은 패턴과 플래그로 구성된다.

```js
const target = "Is this all there is?";

// 패턴: is
// 플래그: i
const literalRegexp = /is/i;
const regexp = new RegExp(/is/i);

literalRegexp.test(target); // true
regexp.test(target); // true
```

동적으로 `RegExp` 객체를 생성할 수 있다.

```js
const count = (str, char) => (str.match(new RegExp(char, "gi")) ?? []).length;

count("Is this all there is", "is"); // 3
count("Is this all there is", "xx"); // 0
```

## 31.3 RegExp 메서드

### `RegExp.prototype.exec`

`exec` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해 매칭 결과를 배열로 반환한다. 매칭 결과가 없는 경우 `null`을 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

⚠️`g` 플래그를 지정해도 첫 번째 매칭 결과가 반환한다.

### `RegExp.prototype.test`

`test` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해 매칭 결과를 불리언 값으로 반환한다.

### `String.prototype.match`

`match` 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;

target.match(regExp); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

`g` 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

## 31.4 플래그

정규 표현식의 검색 방식을 설정하는 데 사용한다. 총 6개가 있지만 중요한 3개만 살펴보자.

| 플래그 | 의미        | 설명                                                            |
| ------ | ----------- | --------------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                  |

플래그는 옵션이며 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다. 플래그가 없으면 대소문자를 구별한다. 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```js
const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
target.match(/is/i);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);
// -> ["is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi);
// -> ["Is", "is", "is"]
```

## 31.5 패턴

패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.

따옴표를 포함하면 패턴에 포함하여 검색된다.

패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다.

### 문자열 검색

플래그를 생략하면 대소문자를 구별하여 매칭되는 첫 번째 결과만 반환한다.

`i` 플래그로 대소문자 구별없이 첫 번째 결과만 반환한다.

`g` 플래그로 매칭되는 모든 문자열을 전역 검색할 수 있다.

### 임의의 문자열 검색

`.`은 임의의 문자 한 개를 의미한다.

```js
const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 반복 검색

`{m,n}`은 앞선 패턴이 최소 `m`번, 최대 `n`번 반복되는 문자열을 의미한다.

⚠️콤마 뒤에 공백이 있으면 정상 동작하지 않는다.

```js
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
```

`{n}`은 앞선 패턴이 `n`번 반복되는 문자열을 의미한다.

`{n,}`은 앞선 패턴이 최소 `n`번 이상 반복되는 문자열을 의미한다.

```js
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // -> ["AA", "AAA"]
```

`+`는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. (== `{1,}`)

`?`는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. (== `{0,1}`)

```js
const target = "color colour";

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // -> ["color", "colour"]
```

### OR 검색

`|`는 or의 의미를 갖는다.

```js
const regExp = /A|B/g;
```

분해되지 않은 단어 레벨로 검색하려면 `+`를 함께 사용할 수 있다.

`[]`내의 문자는 or로 동작한다.

```js
const regExp1 = /A+|B+/g;

const regExp2 = /[AB]+/g;
```

범위를 지정하려면 `[]`내에 `-`를 사용한다.

대소문자를 구분하지 않고 알파벳을 검색하고 싶으면 아래와 같이 작성하면 된다.

```js
const target = "AA BB Aa Bb 12";

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Za-z]+/g;

target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]
```

숫자를 검색하고 싶으면 아래와 같이 작성한다. `\d`는 숫자를 의미한다. `\D`는 숫자가 아닌 문자를 의미한다.

```js
const regExp1 = /[0-9]+/g;

const regExp2 = /[\d,]+/g;
```

`\w`는 알파벳, 숫자, 언더스코어를 의미한다. (== `[A-Za-z0-9_]`) `\W`는 이와 반대로 동작한다.

```js
const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", " ", "$%&"]
```

### NOT 검색

`[]` 내의 `^`은 not의 의미를 갖는다.

### 시작 위치로 검색

`[]` 밖의 `^`는 문자열의 시작을 의미한다.

```js
const target = "https://poiemaweb.com";

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // -> true
```

### 마지막 위치로 검색

`$`는 문자열의 마지막을 의미한다.

```js
const target = "https://poiemaweb.com";

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;

regExp.test(target); // -> true
```

## 31.6 자주 사용하는 정규표현식

### 특정 단어로 시작하는지 검사

```js
const url = "https://example.com";

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true
```

### 특정 단어로 끝나는지 검사

```js
const fileName = "index.html";

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

### 숫자로만 이루어진 문자열인지 검사

```js
const target = "12345";

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

### 하나 이상의 공백으로 시작하는지 검사

`\s`는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. (= `[\t\r\n\v\f]`)

```js
const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

### 아이디로 사용 가능한지 검사

```js
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

### 메일 주소 형식에 맞는지 검사

`*`는 앞선 패턴이 0번 이상 반복될 수 있음을 의미한다.

`()`로 패턴을 묶을 수 있다.

```js
const email = "ungmo2@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  email
); // -> true
```

### 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

### 특수 문자 포함 여부 검사

```js
const target = "abc#123";

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
/[^A-Za-z0-9]/gi.test(target); // -> true

/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi.test(target); // -> true
```
