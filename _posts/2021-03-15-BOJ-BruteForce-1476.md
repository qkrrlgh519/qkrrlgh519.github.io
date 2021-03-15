---
layout: post
title: "BOJ[1476] - 날짜 계산 by JavaScript"
date: 2021-03-15 23:15:00 +0900
categories: BOJ(BruteForce)
---

# 날짜 계산

## 문제

- [백준 1476번 - 날짜 계산](https://www.acmicpc.net/problem/1476)

## 언어

- JavaScript

## 문제 풀이 step 1

- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀면 됩니다.
- 범위는 1 ~ 7980 으로 충분히 가능한 범위입니다.
- 1년씩 늘려가면서 주어진 E S M 에 도달할 때 년수가 정답입니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	const [E, S, M] = input[0].split(" ").map(Number);

	let e = 1;
	let s = 1;
	let m = 1;
	let year = 1;
	while (E !== e || S !== s || M !== m) {
		e++;
		s++;
		m++;
		year++;

		if (e === 16) e = 1;
		if (s === 29) s = 1;
		if (m === 20) m = 1;
	}

	return year;
};

console.log(solution(input));
```
