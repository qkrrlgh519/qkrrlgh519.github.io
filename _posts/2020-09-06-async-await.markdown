---
layout: post
title:  "JavaScript - async, await"
date:   2020-09-06 20:30:00 +0900
categories: JavaScript
---

# async, await

## 1. async, await란?

- **asynchronous** : 비동기, 동시에 일어나지 않는
- **await** : 기다리다
- async와 await는 promise를 조금 더 간결하고 간편하게 그리고 동기적으로 실행되는 것처럼 보이게 만들어준다.
- promise의 then을 이용해서 계속 체이닝하다보면 코드가 난잡해질 수 있는데 async와 await를 사용하면 더욱 간편해진다.
- async, await은 새로운 것이 추가된 것이 아니라 기존에 존재하는 promise 위에 간편한 api를 제공하는 것이다. (이런 것들을 syntactic sugar라고 한다.)

```jsx
function fetchUser() {
	// do network request in 10 secs...
	return 'kiho';
}

const user = fetchUser();
console.log(user);
```

- 네트워크와 같이 오래걸리는 작업을 비동기 처리하지 않으면 자바스크립트 엔진은 동기적으로 코드를 수행하기 때문에  네트워크를 이용해 사용자 정보를 받아오는데 10초 동안 사용자는 아무것도 받지 못하게 기다리게 된다.
- 네트워크와 같이 오래걸리는 작업은 비동기적으로 처리를 해줘야 한다.

## 2. Promise 복습

```jsx
const fetchUser = () => {
	return new Promise((resolve, return) => {
		// do network request in 10 secs...
		resolve('kiho');
	});
};

const user = fetchUser();
user.then(console.log);
```

## 3. async

- 위처럼 Promise를 이용하지 않고도 조금 더 간편하게 비동기를 작성할 수 있는 방법이 있다.
- 함수 앞에 "async"라는 키워드를 붙여주면 된다.
- "async" 키워드를 붙이면 Promise를 쓰지 않아도 자동적으로 함 수 안에 있는 코드 블럭들이 프로미스로 변환이 된다.

```jsx
const fetchUser = async () => {
	// do network request in 10 secs...
	return 'kiho';
};

const user = fetchUser();
user.then(console.log);
```

## 4. await

- "await'이라는 키워드는 "async"가 붙은 함수 안에서만 쓸 수 있다.
- 비동기 처리를 수행하는 코드 앞에 "await'를 붙여서 사용한다.

```jsx
const delay = (ms) => {
	return new Promise((resolve => setTimeout(resolve, ms));
}

const getApple = async () => {
	await delay(3000);
	return 'apple';
}

const getBanana = async () => {
	await delay(3000);
	return 'banana';
}

// getBanana() change to Promise
const getBanana = () => {
	return delay(3000)
		.then(() => 'banana');
};
```

- delay 함수는 promise를 리턴한다. 정해진 시간이 지나면 resolve를 호출한다.
- "await" 키워드를 사용하면 delay(3000)를 기다려준다. 그래서 3초가 있다가 사과를 리턴하는 Promise가 만들어진다.
- 체이닝을 하는 것보다 async, await를 이용해서 만드는 것이 더 가독성이 좋다.

```jsx
// 1. promise chaining 
const pickFruits = () => {
	return getApple().then(apple => {
		return getBanana().then(banana => `${apple} + ${banana}`);
	});		
}
prickFruits().then(console.log); 
```

```jsx
// 2. async, await
const pickFruits = async () => {
	const apple = await getApple();
	const banana = await getBanana();
	return `${apple} + ${banana}`; // 6초 후 'apple + banana'
}
pickFruits().then(console.log);
```

## 5. 유용한 Promise API

- **Promise.all()**
    - Promise.all에 Promise 배열을 전달하게 되면 모든 Promise들이 병렬적으로 다 받을 때까지 모아준다.

```jsx
const pickAllFruits = () => {
	return Promise.all([getApple(), getBanana()])
		.then(fruits => fruits.join(' + ')
	);
}
pickAllFruits().then(console.log);
```

- **Promise.race()**
    - Promise.race에 Promise 배열을 전달하게 되면 모든 Promise 중에서 가장 먼저 값을 리턴하는 promise만 전달된다.

```jsx
const pickOnlyOne = () => {
	return Promise.race([getApple(), getBanana()]);
}
pickOnlyOne().then(console.log);
```

## 6. Summary

- "async" 키워드를 붙이면 Promise를 쓰지 않아도 자동적으로 함 수 안에 있는 코드 블럭들이 프로미스로 변환이 된다.
- "await'는 "async" 키워드가 붙은 함수 안에서만 사용이 가능하며 비동기로 동작하는 소스 코드에 붙이면 된다.

## 출처

- "드림코딩 by 엘리"님 유튜브의 자바스크립트 13. 비동기의 꽃 JavaScript async 와 await 그리고 유용한 Promise APIs , 프론트엔드 개발자 입문편
- 링크 : [https://www.youtube.com/watch?v=aoQSOZfz3vQ](https://www.youtube.com/watch?v=aoQSOZfz3vQ)
