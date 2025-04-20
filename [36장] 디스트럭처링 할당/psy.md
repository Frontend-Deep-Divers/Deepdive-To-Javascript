이터러블 또는 객체를 1개 이상의 변수에 개별적으로 할당하는 문법

## 배열 디스트럭처링 할당

```jsx
// 배열 디스트럭처링 할당
const [a, b, c] = [1, 2, 3];
const [a, , c] = [1, 2, 3]; // 1, 3
const [a, b, c = 10] = [1, 2]; // 1, 2, 10
const [a, b, c] = {}; // TypeError: 우변은 반드시 이터러블이어야 함

const [x, ...y] = [1, 2, 3] // 1, [2, 3]
```

## 객체 디스트럭처링 할당

```jsx
const user = { firstName: 'Park', lastName: 'SangYeon' };

const { firstName, lastName } = user;
const { firstName: fn, lastName: ln } = user;
const { firstName: fn = 'Kim', lastName: ln } = user; // 기본값

// 중첩 객체
const user = {
	name: 'Lee',
	address: {
	  city: 'Suwon'
	}
}

const { address: { city } } = user;

// Rest 프로퍼티
const {x, ...y} = {a:1, b:2, c:3};
// y = {b:2, c:3}
```
