---
layout: post
title: "BOJ[2446] - 별 찍기 - 9 by JavaScript"
date: 2021-04-28 18:30:00 +0900
categories: BOJ(Implementation)
---

# 별 찍기 - 9

## 문제

- [백준 2446번 - 별찍기 - 9](https://www.acmicpc.net/problem/2446)

## 언어

- JavaScript

## 순서도

1. N 번째 줄까지 공백의 양을 1 씩 늘리고, 별의 양을 2 씩 줄여가며 출력
2. 2 x N - 1 번째 줄까지 공백의 양을 1씩 줄이고, 별의 양을 2씩 늘려가며 출력

## 문제 풀이 step 1

- 문제에서 주어진 출력의 내용을 통해서 규칙을 파악하고, 규칙에 맞게 별을 출력하면 되는 문제입니다.
- 규칙은 처음 2 x N - 1 개의 별로 시작해서 0 번째 줄부터 N 번째 줄까지는 2 씩 줄고, 그 이후로는 2 x N - 1 번째 줄까지 2 씩 늘어납니다.

## 후기

- 공백을 왼쪽에만 출력해주면 되는 문제입니다.
- 저는 왼쪽뿐만 아니라 오른쪽에도 공백을 출력해줘서 "출력 형식이 잘못되었습니다" 라는 오류를 계속 받았습니다.
  - "\*\*\*"
  - " \* "
  - "\*\*\*"
- 출력 부분을 마우스로 드래그해서 블럭 지정해보니 오른쪽 공백은 포함되어있지 않았습니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const getStars = (side, center) =>
	Array(side).fill(" ").concat(Array(center).fill("*")).join("") + "\n";

const solution = (input) => {
	const n = Number(input[0]) * 2 - 1;

	let res = "";

	let side = 0;
	let center = n;
	// 1. N 번째 줄까지 공백의 양을 1 씩 늘리고, 별의 양을 2 씩 줄여가며 출력
	for (let i = 0; i < parseInt(n / 2); i++) {
		res += getStars(side, center);
		side += 1;
		center -= 2;
	}

	// 2. 2 x N - 1 번째 줄까지 공백의 양을 1씩 줄이고, 별의 양을 2씩 늘려가며 출력
	for (let i = parseInt(n / 2); i < n; i++) {
		res += getStars(side, center);
		side -= 1;
		center += 2;
	}

	return res;
};

console.log(solution(input));
```
