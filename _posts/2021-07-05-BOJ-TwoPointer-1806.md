---
layout: post
title: "BOJ[1806] - 부분합 by JavaScript"
date: 2021-07-05 13:00:00 +0900
categories: BOJ(TwoPointer)
---

# 부분합

## 문제

- [백준 1806번 - 부분합](https://www.acmicpc.net/problem/1806)

## 언어

- JavaScript

## 순서도

1. start 와 end 두 포인터 선언
2. start 부터 end 까지의 합이 s 보다 작다면, end 를 1 증가시키기
3. start 부터 end 까지의 합이 s 보다 크다면, start 를 1 증가시키기
4. start 부터 end 까지의 합이 s 이상인지 검사하기
5. 2 ~ 4 번 과정을 반복하며 구간합이 s 이상이 되는 모든 경우 중에서 구간합의 길이가 가장 짧은 것의 길이 출력

## 문제 풀이 step 1

- 두 포인터 형식의 문제로 두 개의 포인터를 선언하고 조작하며 문제에서 원하는 결과를 출력하는 문제입니다.
- 본 문제는 **N 개의 수열**을 두 개의 포인터로 연속된 수들의 부분합을 구해서 그 부분합이 **S 이상이 되는 경우** 중에서 부분합의 길이가 가장 짧은 것의 길이를 구하는 문제입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드 1

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	// N 의 최대 길이가 100,000 이라서 임의로 200,000 으로 정했습니다.
	const MAX = 200000;
	const [n, s] = input[0].split(" ").map(Number);
	const nums = input[1].split(" ").map(Number);

	let start = 0;
	let end = 0;
	let sum = 0;
	let length = MAX;
	while (end <= n) {
		// sum 이 s 보다 같거나 크다면 start 포인터 1 증가
		if (sum >= s) sum -= nums[start++];
		// sum 이 s 보다 작다면 end 포인터 1 증가
		else if (sum < s) sum += nums[end++];

		// sum 이 s 이상이라면 길이 갱신
		if (sum >= s) length = Math.min(length, end - start);
	}

	// 부분합이 s 가 되는 경우가 없다면 0 return
	if (length === MAX) return 0;
	else return length;
};

console.log(solution(input));
```
