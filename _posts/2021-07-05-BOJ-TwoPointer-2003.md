---
layout: post
title: "BOJ[2003] - 수들의 합 2 by JavaScript"
date: 2021-07-05 12:30:00 +0900
categories: BOJ(TwoPointer)
---

# 수들의 합 2

## 문제

- [백준 2003번 - 수들의 합 2](https://www.acmicpc.net/problem/2003)

## 언어

- JavaScript

## 순서도

1. start 와 end 두 포인터 선언
2. start 부터 end 까지의 합이 m 보다 작다면, end 를 1 증가시키기
3. start 부터 end 까지의 합이 m 보다 크다면, start 를 1 증가시키기
4. start 부터 end 까지의 합이 m 과 같은지 비교
5. 2 ~ 4 번 과정을 반복하며 구간합이 m 이 되는 모든 경우의 수의 개수 출력

## 문제 풀이 step 1

- 두 포인터 형식의 문제로 두 개의 포인터를 선언하고 조작하며 문제에서 원하는 결과를 출력하는 문제입니다.
- 본 문제는 **N 개의 수열**을 두 개의 포인터로 구간합을 구해서 그 구간합이 **M 과 같은 경우의 수**의 개수를 출력하는 문제입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 간단하게 두 포인터 알고리즘이 무엇인지 알아볼 수 있는 문제입니다.

## 소스 코드 1

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const nums = input[1].split(" ").map(Number);

	let start = 0;
	let end = 0;
	let sum = 0;
	let cnt = 0;
	// end 가 n 을 넘어가지 않을 때까지 반복
	while (end <= n) {
		// 구간합이 m 보다 크다면, start 포인터를 1 증가
		if (sum >= m) sum -= nums[start++];
		// 구간합이 m 보다 작다면, start 포인터를 1 증가
		else if (sum < m) sum += nums[end++];

		// 구간합이 m 과 같다면, 경우의 수의 개수 1 증가
		if (sum === m) cnt += 1;
	}

	// 경우의 수의 개수 출력
	return cnt;
};

console.log(solution(input));
```
