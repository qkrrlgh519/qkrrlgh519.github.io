---
layout: post
title: "BOJ[6603] - 로또 by JavaScript"
date: 2021-03-25 12:30:00 +0900
categories: BOJ(BruteForce)
---

# 로또

## 문제

- [백준 6603번 - 로또](https://www.acmicpc.net/problem/6603)

## 언어

- JavaScript

## 문제 풀이 step 1

- [백준 15650번 - N과 M (2) 풀이](<https://qkrrlgh519.github.io/boj(backtracking)/2021/03/18/BOJ-BackTracking-15650.html>) 에서 알아 본 알고리즘을 이용하면 쉽게 풀 수 있습니다.
- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀 수 있습니다.
- k 개의 숫자를 중복없이 6개 골라야 합니다. 단, 오름차순으로 골라야 합니다.
- DFS와 유사한 방식의 탐색을 하는 방법으로도 풀 수 있고, 각 숫자를 선택할 지, 선택하지 않을 지 이렇게 2 개의 경루로 나눠서 접근하는 방법으로도 풀 수 있습니다.
- 저는 후자의 방법으로 풀어봤습니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

let res = "";

const rec = (n, arr, choices, index, depth) => {
	if (depth === 6) {
		res += choices.join(" ") + "\n";
		return;
	}

	if (index >= n) return;

	choices.push(arr[index]);
	rec(n, arr, choices, index + 1, depth + 1);

	choices.pop();
	rec(n, arr, choices, index + 1, depth);
};

const solution = (input) => {
	let index = 0;

	while (true) {
		const arr = input[index++].split(" ").map(Number);
		if (arr[0] === 0) break;

		const [k, s] = [arr[0], arr.slice(1)];

		const choices = [];
		rec(k, s, choices, 0, 0);

		res += "\n";
	}

	return res;
};

console.log(solution(input));
```
