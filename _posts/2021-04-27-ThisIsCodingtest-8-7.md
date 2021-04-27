---
layout: post
title: "이것이 코딩테스트다 - [DynamicProgramming] 바닥 공사 by JavaScript"
date: 2021-04-27 12:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 바닥 공사

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- **점화식**
  - dp[N] = 가로의 길이가 N, 세로의 길이가 2 인 직사각형을 3 종류의 덮개로 채울 수 있는 방법의 수
- **경우**
  1.  N - 1 까지 채운 직사각형에 **1 개의 2 x 1 의 덮개**를 추가하는 경우
  2.  N - 2 까지 채운 직사각형에 **2 개의 1 x 2 의 덮개**를 추가하는 경우와 **1 개의 2 x 2 의 덮개**를 추가하는 경우

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `3`.split("\n");

const solution = (input) => {
	const n = Number(input[0]);

	const dp = [];

	// base
	dp[0] = 1;
	dp[1] = 1;

	for (let i = 2; i <= n; i++) {
		dp[i] = dp[i - 1] + dp[i - 2] * 2;
	}

	return dp[n];
};

console.log(solution(input));
```
