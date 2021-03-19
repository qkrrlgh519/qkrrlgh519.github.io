---
layout: post
title: "BOJ[15652] - N과 M (4) by JavaScript"
date: 2021-03-19 16:30:00 +0900
categories: BOJ(BackTracking)
---

# N과 M (4)

## 문제

- [백준 15652번 - N과 M (4)](https://www.acmicpc.net/problem/15652)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 1 ~ N 까지의 자연수 중에서 중복 가능하게 M 개를 골라야 합니다.
- [백준 15649번 - N과 M (2) 풀이](<https://qkrrlgh519.github.io/boj(backtracking)/2021/03/18/BOJ-BackTracking-15650.html>)와 유사한데, 이번에는 비내림차순입니다.
- 위의 풀이에서는 방금 고른 수를 `before` 라는 변수에 담고, 다음 탐색시 `before` 보다 1 큰 수부터 탐색을 시작했습니다.
- 이번에도 방금 고른 수를 `before`라는 변수에 담지만 다음 탐색시 `before` 에서 바로 탐색을 시작하면 됩니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const rec = (n, m, res, step, before, depth) => {
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	// 전 단계에서 고른 수부터 탐색을 시작
	for (let i = before; i <= n; i++) {
		step.push(i);
		rec(n, m, res, step, i, depth + 1);
		step.pop();
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	const res = [];
	const step = [];
	rec(n, m, res, step, 1, 0);

	return res.join("\n");
};

console.log(solution(input));
```
