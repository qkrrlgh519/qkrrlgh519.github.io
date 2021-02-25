---
layout: post
title: "BOJ[10773] - 제로 by JavaScript"
date: 2021-02-25 21:00:00 +0900
categories: BOJ(DataStructure)
---

# 제로 (스택)

## 문제

- [백준 10773번 - 제로](https://www.acmicpc.net/problem/10773)

## 언어

- JavaScript

## 문제 풀이 step 1

- 스택을 이용해서 풀 수 있습니다.
- '0'이 들어올 경우 pop 하고, 그 외의 경우에는 push 합니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	const K = parseInt(input[0]);
	const stack = [];

	for (let i = 1; i < K + 1; i++) {
		if (input[i] === "0") stack.pop();
		else stack.push(parseInt(input[i]));
	}

	let sum = 0;
	for (let i = 0; i < stack.length; i++) {
		sum += stack[i];
	}

	return sum;
};

console.log(solution(input));
```
