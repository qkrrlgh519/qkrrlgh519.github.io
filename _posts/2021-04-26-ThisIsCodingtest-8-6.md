---
layout: post
title: "이것이 코딩테스트다 - [DynamicProgramming] 개미 전사 by JavaScript"
date: 2021-04-26 12:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 개미 전사

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- **점화식**
  - dp[N] = N 개의 식량창고 중에서 뺏을 수 있는 식량의 최댓값
- **경우**
  1.  N 번째 식량창고를 터는 경우
  2.  N 번째 식량창고를 털지 않는 경우

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `4
// 1 3 1 5`.split("\n");

const solution = (input) => {
	const n = Number(input[0]);
	const arr = input[1].split(" ").map(Number);
	const dp = [];
	dp[0] = arr[0];
	dp[1] = Math.max(arr[0], arr[1]);

	for (let i = 2; i < n; i++) {
		dp[i] = Math.max(dp[i - 1], dp[i - 2] + arr[i]);
	}

	return dp[n - 1];
};

console.log(solution(input));
```
