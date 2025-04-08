## 생성자 함수

[[PrimitiveValue]], [[StringData]] 사용

String 래퍼 객체는 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 갖는 유사 배열 객체

문자열은 수정할 수 없지만 에러가 발생하지는 않음

## 메서드

String 래퍼 객체는 읽기 전용 객체 → 언제나 새로운 문자열을 생성하여 반환

### String.prototype.indexOf

문자열을 검색하여 첫 인덱스 반환

### String.prototype.search

정규 표현식과 매치하는 문자열을 검색하여 인덱스 반환

### String.prototype.includes

문자열을 검색하여 존재 여부를 불리언 값으로 반환

### String.prototype.startsWith/endsWith

인수로 전달받은 문자열로 시작하는지 확인하여 결과를 반환

### String.prototype.charAt

주어진 인덱스의 문자를 반환

### String.prototype.substring

첫 번째 인수 인덱스부터 두 번째 인수 인덱스의 이전 문자까지의 부분 문자열 반환

### String.prototype.slice

substring과 동일하지만 음수 인수를 전달할 수 있음

### String.prototype.toUpperCase/toLowerCase

### String.prototype.trim

### String.prototype.repeat

문자열을 인수로 전달한 정수만큼 반복하여 연결

### String.prototype.replace

전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환

검색된 문자열이 여럿 존재할 경우 첫 문자열만 치환하지만, 정규 표현식으로 여러 결과를 찾으면 한 번에 치환

특수한 교체 패턴을 사용할 수 있음

### String.prototype.split

두 번째 인수로 배열의 길이를 지정할 수 있음
