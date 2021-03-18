---
layout: post
title: "BOJ[15651] - N과 M (3) by JavaScript"
date: 2021-03-19 22:30:00 +0900
categories: BOJ(BackTracking)
---

# N과 M (3)

## 문제

- [백준 15651번 - N과 M (3)](https://www.acmicpc.net/problem/15651)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 1 ~ N 까지의 자연수 중에서 중복 가능하게 M 개를 골라야 합니다.
- [백준 15649번 - N과 M (1) 풀이](<https://qkrrlgh519.github.io/boj(backtracking)/2021/03/18/BOJ-BackTracking-15649.html>)와 유사한데, 중복 조건이 없어졌습니다.
- 위의 풀이에 중복 체크하는 부분을 제거해주면 됩니다. 즉, 방문 처리를 하지 않고, 방문 횟수만 만족하면 탐색을 종료하는 방식으로 DFS가 진행됩니다.
- 이렇게 말하고 보니, DFS 라고 말하기가 애매해집니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const rec = (n, m, res, step, depth) => {
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	for (let i = 1; i <= n; i++) {
		// 중복 체크하는 부분이 없다.
		step.push(i);
		rec(n, m, res, step, depth + 1);
		step.pop();
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	const res = [];
	const step = [];
	rec(n, m, res, step, 0);

	return res.join("\n");
};

console.log(solution(input));
```
