---
layout: post
title: "이것이 코딩테스트다 - [Implementation] 상하좌우 by JavaScript"
date: 2021-04-06 12:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 상하좌우

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 문제에서 주어진 대로 구현하는 구현 유형의 문제입니다.
- 'L' 이면 왼쪽으로, 'R' 이면 오른쪽으로, 'U' 면 위로, 'D' 면 아래로 이동하면 됩니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5
// R R R U D D`.split("\n");

const isPossibleRoute = (n, x, y) => 0 < x && x < n && 0 < y && y < n;

const controller = (x, y, command) => {
	switch (command) {
		case "U":
			return [x - 1, y];
		case "D":
			return [x + 1, y];
		case "L":
			return [x, y - 1];
		case "R":
			return [x, y + 1];
	}
};

const solution = (input) => {
	const n = parseInt(input[0]);
	const commands = input[1].split(" ");
	let posX = 1;
	let posY = 1;

	for (let i = 0; i < commands.length; i++) {
		const [nextPosX, nextPosY] = controller(posX, posY, commands[i]);

		if (!isPossibleRoute(n, nextPosX, nextPosY)) continue;
		[posX, posY] = [nextPosX, nextPosY];
		console.log(posX, posY);
	}

	return posX + " " + posY;
};

console.log(solution(input));
```

---

## 다른 방식의 문제 풀이 step 1

- 저자님의 풀이를 보고 조금 더 효율적이게 만들 수 있을 것 같아서 참고해서 작성해봤습니다.
- controller 함수 내부만 조금 바뀌었습니다.
- 아무래도 switch 문 보다는 배열의 index 접근이 더 빠를 것 같습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5
// R R R U D D`.split("\n");

const isPossibleRoute = (n, x, y) => 0 < x && x < n && 0 < y && y < n;

const controller = (x, y, command) => {
	const keys = {
		U: 0,
		D: 1,
		L: 2,
		R: 3,
	};

	const nx = [-1, 1, 0, 0];
	const ny = [0, 0, -1, 1];

	return [x + nx[keys[command]], y + ny[keys[command]]];
};

const solution = (input) => {
	const n = parseInt(input[0]);
	const commands = input[1].split(" ");
	let posX = 1;
	let posY = 1;

	for (let i = 0; i < commands.length; i++) {
		const [nextPosX, nextPosY] = controller(posX, posY, commands[i]);

		if (!isPossibleRoute(n, nextPosX, nextPosY)) continue;
		[posX, posY] = [nextPosX, nextPosY];
	}

	return posX + " " + posY;
};

console.log(solution(input));
```
