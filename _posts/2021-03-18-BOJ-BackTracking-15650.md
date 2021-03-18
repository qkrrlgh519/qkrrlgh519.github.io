---
layout: post
title: "BOJ[15650] - N과 M (2) by JavaScript"
date: 2021-03-18 21:10:00 +0900
categories: BOJ(BackTracking)
---

# N과 M (2)

## 문제

- [백준 15650번 - N과 M (2)](https://www.acmicpc.net/problem/15650)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 1 ~ N 까지의 자연수 중에서 중복 없이 M 개를 골라야 합니다. 단, 오름차순이어야 합니다.
- [백준 15649번 - N과 M (1) 풀이](<https://qkrrlgh519.github.io/boj(backtracking)/2021/03/18/BOJ-BackTracking-15649.html>)와 유사한데, 오름차순 조건이 생겼습니다.
- 조건은 M 개입니다. 즉, M 개를 고른 상황이 되면 탐색을 종료하면서 진행해나가면 됩니다.
- 중복 없이 조건이 있지만 오름차순 조건을 만족하는 방향으로 구현을 하면 중복 체크는 할 필요 없어집니다.
- 오름차순 조건을 만족하기 위해서 방금 고른 수를 `before` 변수에 기록하면서 탐색을 진행합니다. 다음 탐색 시에는 방금 고른 수의 다음 수부터 탐색하면 되겠습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

// 오름차순을 만들기 위해 before에 전 단계에 사용한 수를 기록
const rec = (n, m, res, step, before, depth) => {
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	// 전 단계 다음부터 탐색을 시작 (중복 체크는 할 필요 없음)
	for (let i = before + 1; i < n + 1; i++) {
		step.push(i);
		rec(n, m, res, step, i, depth + 1);
		step.pop();
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	const res = [];
	const step = [];
	rec(n, m, res, step, 0, 0);

	return res.join("\n");
};

console.log(solution(input));
```

---

## 다른 방식의 문제 풀이 step 1

- 위의 방식은 각 노드를 탐색하는 DFS와 유사한 방식의 풀이인 반면 이번에 다뤄볼 풀이는 접근 방식이 다릅니다.
- 이번에는 각 숫자를 선택하는 경우와 선택하지 않는 경우 이렇게 2 개의 경우로 나눠서 접근하는 방식입니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const rec = (n, m, res, step, index, depth) => {
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	// index를 늘려가며 진행해서 모든 수를 선택하지 않는 경우 n 을 넘어서는 경우가 있을 수 있다.
	if (index > n) return;

	// 현재의 수를 선택하는 경우
	step.push(index);
	rec(n, m, res, step, index + 1, depth + 1);

	// 현재의 수를 선택하지 않는 경우
	step.pop();
	rec(n, m, res, step, index + 1, depth);
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
