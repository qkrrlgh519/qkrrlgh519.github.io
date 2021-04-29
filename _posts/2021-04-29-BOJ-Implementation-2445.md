---
layout: post
title: "BOJ[2445] - 별 찍기 - 8 by JavaScript"
date: 2021-04-29 09:30:00 +0900
categories: BOJ(Implementation)
---

# 별 찍기 - 8

## 문제

- [백준 2445번 - 별찍기 - 8](https://www.acmicpc.net/problem/2445)

## 언어

- JavaScript

## 순서도

1. N 번째 줄까지 양 사이드쪽의 별의 개수를 1 씩 늘리고, 가운데 공백의 개수를 2 씩 줄입니다.
2. 2 x N - 1 번째 줄까지 양 사이드쪽의 별의 개수를 1 씩 줄이고, 가운데 공백의 개수를 2 씩 늘립니다.

## 문제 풀이 step 1

- 문제에서 주어진 출력의 내용을 통해서 규칙을 파악하고, 규칙에 맞게 별을 출력하면 되는 문제입니다.
- 규칙은 순서도에 작성한 순서입니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const getStars = (side, center) =>
	Array(side)
		.fill("*")
		.concat(Array(center).fill(" "))
		.concat(Array(side).fill("*"))
		.join("") + "\n";

const solution = (input) => {
	const n = Number(input[0]);

	let res = "";

	let side = 0;
	let center = n * 2;
	for (let i = 0; i < n; i++) {
		side += 1;
		center -= 2;
		res += getStars(side, center);
	}

	for (let i = n; i < 2 * n - 1; i++) {
		side -= 1;
		center += 2;
		res += getStars(side, center);
	}

	return res;
};

console.log(solution(input));
```
