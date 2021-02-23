---
layout: post
title: "BOJ[2309] - 일곱 난쟁이 by JavaScript"
date: 2021-02-23 16:30:00 +0900
categories: BOJ(BruteForce)
---

# 일곱 난쟁이

## 문제

- [백준 2309번 - 일곱 난쟁이](https://www.acmicpc.net/problem/2309)

## 언어

- JavaScript

## 문제 풀이 step 1

- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀면 됩니다. (범위는 충분히 가능한 숫자입니다.)
- 난쟁이 9명 중 7명의 키의 총 합이 100입니다.
- 따라서 9명 중에서 2명을 빼는 모든 경우의 수를 고려해서 7명의 난쟁이의 키의 합이 100이 될 때가 정답입니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	const dwarf = input.map(Number);

	let sum = 0;
	for (let i = 0; i < 9; i++) {
		sum += dwarf[i];
	}

	for (let i = 0; i < 9; i++) {
		for (let j = 0; j < 9; j++) {
			if (i == j) continue;

			if (sum - (dwarf[i] + dwarf[j]) === 100) {
				dwarf[i] = 0;
				dwarf[j] = 0;
				i = 9;
				break;
			}
		}
	}

	return dwarf
		.filter((v) => v !== 0)
		.sort((a, b) => a - b)
		.join("\n");
};

console.log(solution(input));
```
