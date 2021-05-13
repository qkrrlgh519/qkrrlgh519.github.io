---
layout: post
title: "BOJ[2290] - LCD Test by JavaScript"
date: 2021-05-13 16:30:00 +0900
categories: BOJ(Implementation)
---

# LCD Test

## 문제

- [백준 2290번 - LCD Test](https://www.acmicpc.net/problem/2290)

## 언어

- JavaScript

## 순서도

1. 주어진 n 을 한 글자씩 출력 양식으로 변환

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- 가장 고민되는 부분은 각 숫자를 출력 형식의 모양으로 변환하는 방법입니다.
- 각 숫자를 그려보면 어떤 성질을 찾을 수 있습니다.
- 그 성질에 대해서는 아래에서 알아보겠습니다.

## 문제 풀이 step 2

- 8 에 대해서 먼저 알아보겠습니다. (8 이 모든 경우를 볼 수 있기 때문입니다.)
- s 가 1 인 경우,
  ```jsx
  [
  	[" ", "-", " "], // 위쪽
  	["|", " ", "|"],
  	[" ", "-", " "], // 중간
  	["|", " ", "|"],
  	[" ", "-", " "], // 아래쪽
  ];
  ```
- s 가 2 인 경우,
  ```jsx
  [
  	[" ", "-", "-", " "], // 위쪽
  	["|", " ", " ", "|"],
  	["|", " ", " ", "|"],
  	[" ", "-", "-", " "], // 중간
  	["|", " ", " ", "|"],
  	["|", " ", " ", "|"],
  	[" ", "-", "-", " "], // 아래쪽
  ];
  ```
- 각 경우마다 위와 같은 형태의 배열을 만들 수 있습니다.
- 행의 경우는 총 세 가지가 있습니다.
  - 위쪽 : `(0, 1), (0, 2)`
  - 중간 : `(3, 1), (3, 2)`
  - 아래쪽 : `(6, 1), (6, 2)`
- 배열의 가로는 s + 2 인데, 이 때 행의 경우는 좌측 끝과 우측 끝은 무조건 공백(" ") 입니다.
- 그래서 행은 딱 s 크기만큼만 차지합니다.
- 그리고
- 열의 경우는 총 네 경우가 있습니다.
  - 왼쪽 위 : `(1, 0), (2, 0)`
  - 오른쪽 위 : `(1, 3), (2, 3)`
  - 왼쪽 아래 : `(4, 0), (5, 0)`
  - 오른쪽 아래 : `(4, 3), (5, 3)`
- 배열의 세로는 2 x s + 3 인데, 이 때 열의 경우는 행이 그려지는 위쪽, 중간, 아래쪽은 무조건 공백(" ") 입니다.
- 그리고 열은 2 x s 크기만큼만 차지합니다.

## 문제 풍이 step 3

- 위에서 알아봤듯이 숫자를 만들기 위해서 총 7 개의 경우를 고려해서 만들어야 합니다.
  - 행 : 위쪽, 중간 아래쪽
  - 열 : 왼쪽 위, 오른쪽 위, 왼쪽 아래, 오른쪽 아래
- s 가 2 인 경우, 0 을 예시로 든다면
  ```jsx
  [
  	[" ", "-", "-", " "],
  	["|", " ", " ", "|"],
  	["|", " ", " ", "|"],
  	[" ", " ", " ", " "],
  	["|", " ", " ", "|"],
  	["|", " ", " ", "|"],
  	[" ", "-", "-", " "],
  ];
  ```
- 0 을 만들기 위해서는
  - 행은 위쪽, 아래쪽
  - 열은 왼쪽 위, 오른쪽 위, 왼쪽 아래, 오른쪽 아래 를 그려야 합니다.

## 문제 풀이 step 4

- 이렇게 알아본 7 개의 성질에 매칭되는 함수를 각각 하나씩 만들어줍니다.
- 7 개의 성질에 매칭되는 함수를 이용해서 0 ~ 9 까지 각 숫자별로 출력 형식으로 변환해주는 함수를 만들어줍니다.
- 그리고 주어진 n 에 대해서 한 글자씩 출력 형식의 배열로 만들어주고, 만든 출력 형식의 배열을 이어붙여서 문자열로 만들어서 출력해주면 정답이 됩니다.
- 자세한 설명은 주석에 작성하겠습니다.

## 후기

- 소스 코드를 줄이기 위해서 꼭 다시 풀어볼 문제
- 이번 문제 역시 펜으로 그려보면서 감을 잡았습니다.
- 다만 최대한 소스 코드의 중복을 줄이려 했으나, 많이 줄이지 못해서 다음에 더 줄여봤으면 좋겠습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const ROW_STR = "-";
const COL_STR = "|";

// 총 7 개의 경우를 정의
// 행: 위쪽
const rowTop = (s, row, col, baseArr) => {
	for (let i = 1; i < row - 1; i++) baseArr[0][i] = ROW_STR;
};
// 행: 중간
const rowCenter = (s, row, col, baseArr) => {
	for (let i = 1; i < row - 1; i++) baseArr[(col - 1) / 2][i] = ROW_STR;
};
// 행: 아래쪽
const rowBot = (s, row, col, baseArr) => {
	for (let i = 1; i < row - 1; i++) baseArr[col - 1][i] = ROW_STR;
};

// 열: 왼쪽 위
const colTopLeft = (s, row, col, baseArr) => {
	for (let i = 1; i <= s; i++) baseArr[i][0] = COL_STR;
};
// 열: 오른쪽 위
const colTopRight = (s, row, col, baseArr) => {
	for (let i = 1; i <= s; i++) baseArr[i][row - 1] = COL_STR;
};
// 열: 왼쪽 아래
const colBotLeft = (s, row, col, baseArr) => {
	for (let i = 1; i <= s; i++) baseArr[i + s + 1][0] = COL_STR;
};
// 열: 오른쪽 아래
const colBotRight = (s, row, col, baseArr) => {
	for (let i = 1; i <= s; i++) baseArr[i + s + 1][row - 1] = COL_STR;
};

// 위에서 정의한 7 개의 경우 함수를 이용해서 각 함수를 생성
const makeZero = (s, row, col, baseArr) => {
	// 0 은
	// 행은 위쪽과 아래쪽
	// 열은 왼쪽 위, 오른쪽 위, 왼쪽 아래, 오른쪽 아래
	rowTop(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopLeft(s, row, col, baseArr);
	colTopRight(s, row, col, baseArr);
	colBotLeft(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeOne = (s, row, col, baseArr) => {
	// 1 은
	// 행은 없음
	// 열은 오른쪽 위, 오른쪽 아래
	colTopRight(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeTwo = (s, row, col, baseArr) => {
	// 2 는
	// 행은 위쪽, 중간, 아래쪽
	// 열은 오른쪽 위, 왼쪽 아래
	rowTop(s, row, col, baseArr);
	rowCenter(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopRight(s, row, col, baseArr);
	colBotLeft(s, row, col, baseArr);

	return baseArr;
};

const makeThree = (s, row, col, baseArr) => {
	rowTop(s, row, col, baseArr);
	rowCenter(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopRight(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeFour = (s, row, col, baseArr) => {
	rowCenter(s, row, col, baseArr);

	colTopLeft(s, row, col, baseArr);
	colTopRight(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeFive = (s, row, col, baseArr) => {
	rowTop(s, row, col, baseArr);
	rowCenter(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopLeft(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeSix = (s, row, col, baseArr) => {
	rowTop(s, row, col, baseArr);
	rowCenter(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopLeft(s, row, col, baseArr);
	colBotLeft(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeSeven = (s, row, col, baseArr) => {
	rowTop(s, row, col, baseArr);

	colTopRight(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeEight = (s, row, col, baseArr) => {
	rowTop(s, row, col, baseArr);
	rowCenter(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopLeft(s, row, col, baseArr);
	colTopRight(s, row, col, baseArr);
	colBotLeft(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const makeNine = (s, row, col, baseArr) => {
	rowTop(s, row, col, baseArr);
	rowCenter(s, row, col, baseArr);
	rowBot(s, row, col, baseArr);

	colTopLeft(s, row, col, baseArr);
	colTopRight(s, row, col, baseArr);
	colBotRight(s, row, col, baseArr);

	return baseArr;
};

const solution = (input) => {
	let [s, n] = input[0].split(" ");
	s = Number(s);
	const [row, col] = [s + 2, 2 * s + 3];

	// 각 함수를 배열의 원소로 넣어줌
	const maker = [
		makeZero,
		makeOne,
		makeTwo,
		makeThree,
		makeFour,
		makeFive,
		makeSix,
		makeSeven,
		makeEight,
		makeNine,
	];

	const res = Array.from({length: col}, () => []);
	for (let i = 0; i < n.length; i++) {
		const num = Number(n[i]);

		// 주어진 s 를 이용해서 공백(" ") 으로 채워져 있는 base 배열을 생성
		const baseArr = Array.from({length: col}, () => Array(row).fill(" "));

		// 숫자를 출력 형식의 배열로 변환
		const makedNum = maker[num](s, row, col, baseArr);

		// 만든 배열을 합쳐줌
		for (let j = 0; j < col; j++) {
			res[j] = res[j].concat([...makedNum[j], " "]);
		}
	}

	// 문자열 형태로 변환해서 출력
	return res.map((v) => v.join("")).join("\n");
};

console.log(solution(input));
```
