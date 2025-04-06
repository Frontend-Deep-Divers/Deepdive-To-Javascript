현재 날짜와 시간은 자바스크립트가 실행된 시스템의 시계에 의해 결정됨

# 생성자 함수

Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가짐

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타냄

### new Date()

현재 날짜와 시간을 가지는 Date 객체 반환

new 연산자 없이 호출하면 문자열을 반환

### new Date(milliseconds)

인수로 전달된 밀리초만큼 경과된 날짜와 시간을 나타내는 Date 객체 반환

### new Date(dateString)

Date.parse 메서드로 해석할 수 있는 형식의 문자열을 인수로 전달하면 해당하는 Date 객체 반환

### new Date(year, month [, day, hour, minute, second, milliseconds])

각 정보에 해당하는 숫자를 인수로 전달하면 Date 객체 반환 → year, month 는 필수

# 메서드

### Date.now

현재 시간까지 경과한 밀리초를 숫자로 반환

### Date.parse

일정한 포맷으로 전달된 인수에 해당하는 날짜까지의 밀리초를 숫자로 반환

### Date.UTC(year, month[, day, …])

전달된 시간까지의 밀리초를 숫자로 반환

로컬 타임이 아닌 UTC로 인식

### Date.prototype.getFullYear/setFullYear

### Date.prototype.getMonth/setMonth

### Date.prototype.getDate/setDate

### Date.prototype.getDay/setDay

요일을 나타내는 정수 반환

### Date.prototype.getHours/setHours

### Date.prototype.getMinutes/setMinutes

### Date.prototype.getSeconds/setSeconds/getMilliseconds/setMilliseconds

### Date.prototype.getTime/setTime

경과된 밀리초를 반환

### Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로케일 시간과의 차이를 분 단위로 반환

### Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 날짜를 반환

### Date.prototype.toTimeString

시간을 반환

### Date.prototype.toISOString

ISO 8601 형식으로 날짜와 시간을 표현한 문자열을 반환

### Date.prototype.toLocaleString/toLocaleTimeString

인수로 로케일을 전달하면, 해당 로케일 기준으로 날짜와 시간을 문자열로 반환
