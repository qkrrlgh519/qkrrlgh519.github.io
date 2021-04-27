---
layout: post
title: "이것이 코딩테스트다 - [DynamicProgramming] 1로 만들기 by JavaScript"
date: 2021-04-26 11:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 1로 만들기

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 다이나믹 프로그래밍

- 다이나믹 프로그래밍을 사용할 수 있는 문제의 조건
  1.  큰 문제를 작은 문제로 나눌 수 있다.
  2.  작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일하다.
- 다이나믹 프로그래밍 유형의 문제를 풀면 풀수록 익숙해지는 조건들인것 같습니다.

## 문제 풀이 step 1

- **점화식**
  - dp[N] = 정수 N 을 1 로 만들 수 있는 연산의 최소 횟수
- **경우**
  1.  1 을 빼는 경우
  2.  2 로 나누어떨어지면, 2 로 나누는 경우
  3.  3 으로 나누어떨어지면, 3 으로 나누는 경우
  4.  5 로 나누어떨어지면, 5 로 나누는 경우

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `26`.split("\n");

const solution = (input) => {
	const n = parseInt(input[0]);
	const dp = [];
	dp[1] = 0;

	for (let i = 2; i <= n; i++) {
		dp[i] = dp[i - 1] + 1;
		if (i % 5 === 0) dp[i] = Math.min(dp[i], dp[i / 5] + 1);
		if (i % 3 === 0) dp[i] = Math.min(dp[i], dp[i / 3] + 1);
		if (i % 2 === 0) dp[i] = Math.min(dp[i], dp[i / 2] + 1);
	}

	return dp[n];
};

console.log(solution(input));
```
