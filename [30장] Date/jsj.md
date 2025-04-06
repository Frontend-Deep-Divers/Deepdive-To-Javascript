# 30장 Date

## 30.1 Date 생성자 함수

- Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가짐
- 이 값은 1970년 1월 1일을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.
- Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가진다.
- 현재 날짜, 시간이 아닌 다른 날짜와 시간을 다루고 싶다면 명시적으로 인수에 전달해줘야한다.
- new Date()
    - Date생성자 함수를 인수없이 new 연산자와 함께 호출하여 현재 날짜와 시간을 가진 Date 객체를 반환
- new Date(milliseconds)
    - 1970년 1월 1일을 기준으로 인수로 전달된 밀리초 만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환
- new Date(dateString)
    - 지정된 날짜와 시간을 나타내는 Date 객체를 반환
- new Date(year, month[, day, hour, minute, second, millisecond])
    - 연, 월, 일, 시, 분, 초, 밀리초 를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
    - 이때 연, 월은 반드시 지정해야한다.

## 30.2 Date 메서드

- Date.now
    - 1970년 1월 1일을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환
- Date.parse
    - 인수로 전달된 지정 시간(`new Date(dateString)`)의 인수까지의 밀리초를 숫자로 변환
- 이외에도 Date 객체의 연도, 월, 시간, 요일등을 반환하거나 설정할 수 있는 메서드들이 있다..

> ❓dateString으로 해석될 수 있는 형식은 어떤것이 있을까?
공식적으로 Date.parse()가 지원하는 형식은 `YYYY-MM-DDTHH:mm:ss.sssZ`  하나뿐이다. 이는 ECMAScript 에서 보장한 유일한 파싱 형식이다.
그 외에는 브라우저마다 다르게 해석될 수 있다.
> 

## 30.3 Date를 활용한 시계 예제

```jsx
(function printNow(){
	const today = new Date();
	
	const dayNums = [
		'(일요일)',
		'(월요일)',
		'(화요일)',
		'(수요일)',
		'(목요일)',
		'(금요일)',
		'(토요일)'
];

const day = dayNames[today.getDay()]; //해당 요일(0~6)을 나타내는 정수 반환

const year = today.getFullYear(); //Date객체의 연도
const month = today.getMonth()+1; //Date객체의 월. 0부터 1월임
const date = today.getDate();     //Date객체의 일
let hour = today.getHours();      //Date객체의 시간 (0~23)
let minute = today.getMinutes();  //Date객체의 분
let second = today.getSeconds();  //Date객체의 시간
const ampm = hour >= 12 ? 'PM' : 'AM';
hour = hour || 12 //hour가 0이면 12로 재할당 	
//10 아래인 분, 초 앞에 0을 붙여서 2자리로 만듬
minute = minute < 10? '0'+minute : minute; 
second = second < 10? '0'+second : second;	

const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;
console.log(now);

setTimeout(printNow, 1000); //1초마다 printNow 함수 재귀호출
}());
```