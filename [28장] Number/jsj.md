# 28장 Number

## 28.1 Number 생성자 함수

- new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있으며, 생성자 함수에 인수를 전달하지 않는 경우 `[[NumberData]]` 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
- 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한다. 인수를 숫자로 변환할 수 없다면 NaN을 할당한다.
- new 연산자를 사용하지 않고 호출하면 Number인스턴스가 아닌 숫자를 반환 (타입 변환)

## 28.2 Number 프로퍼티

- Number.EPSILON
    - 아주아주 작은 숫자를 나타낸다. (부동 소수점 산술 연산의 기준)
    - 두 수를 뺀 절댓값이 얘보다 작으면 같은 수로 인정한다.
- Number.MAX_VALUE
    - 자바 스크립트에서 표현할 수 있는 가장 큰 양수값
    - 이보다 큰 숫자는 Infinity다.
- Number.MIN_VALUE
    - 자바 스크립트에서 표현할 수 있는 가장 작은 양수값
    - 이보다 작은 숫자는 0이다.

## 28.3 Number 메서드

- Number.isFinite
    - 인수로 전달된 숫자값이 정상적인 유한수 (Infinity, -Infinity)인지 검사
- Number.isInteger
    - 인수로 전달된 숫자값이 정수인지 검사
- Number.isNaN
    - 인수로 전달된 숫자값이 NaN인지 검사
- Number.isSafeInteger
    - 인수로 전달된 숫자값이 안전한 정수인지 검사
- Number.prototype.toExponential
    - 숫자를 지수 표기법으로 변환하여 문자열로 반환
- Number.prototype.toFixed
    - 숫자를 반올림하여 문자열로 반환
- Number.prototype.toString
    - 숫자를 문자열로 변환하여 반환