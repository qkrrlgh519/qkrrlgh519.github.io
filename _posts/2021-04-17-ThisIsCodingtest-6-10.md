---
layout: post
title: "이것이 코딩테스트다 - [Sort] 위에서 아래로 by JavaScript"
date: 2021-04-17 15:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 위에서 아래로

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 나동빈 저자님께서 파이썬의 기본 정렬 라이브러리를 이용해서 간결하게 풀어주셨습니다.
- JavaScript 에서도 Array 가 제공하는 sort 함수를 이용해서 간결하게 풀 수 있습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `3
// 15
// 27
// 12`.split("\n");

const solution = (input) => {
	const n = Number(input[0]);
	const arr = input.slice(1).map(Number);

	return arr.sort((a, b) => b - a);
};

console.log(solution(input));
```
