---
layout: post
title: "BOJ[1748] - 수 이어 쓰기 1 by JavaScript"
date: 2021-03-17 16:55:00 +0900
categories: BOJ(BruteForce)
---

# 수 이어 쓰기 1

## 문제

- [백준 1748번 - 수 이어 쓰기 1](https://www.acmicpc.net/problem/1748)

## 언어

- JavaScript

## 문제 풀이 step 1

- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀면 됩니다.
- 하지만 1 부터 100000000 까지 1씩 늘려가면서 자릿수를 세기에는 범위가 너무 큽니다.
- 다행히도, 중간 중간 건너뛸 수 있는 경우들이 있습니다.
  - 1 ~ 9 = 1 자리
  - 10 ~ 99 = 2 자리
  - 100 ~ 999 = 3 자리
  - ...
  - 10000000 ~ 99999999 = 8 자리
  - 100000000 = 9 자리
- 위의 경우들로 범위를 많이 줄일 수 있습니다.
- 처음에는 주어진 n 의 자릿수를 계산하고, 그 자릿수의 초기값 (10 ^ (n의 자릿수 - 1))을 빼줍니다.
- 그리고 이후는 위에 나온 각 자릿수의 경우대로 계산해주면 됩니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	let n = parseInt(input[0]);
	let len = input[0].length;
	let start = Math.pow(10, len - 1);
	let cnt = 0;

	while (len) {
		cnt += len * (n - start + 1);

		n = start - 1;
		len--;
		start /= 10;
	}

	return cnt;
};

console.log(solution(input));
```
