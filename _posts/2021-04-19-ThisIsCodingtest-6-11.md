---
layout: post
title: "이것이 코딩테스트다 - [Sort] 성적이 낮은 순서로 학생 출력하기 by JavaScript"
date: 2021-04-19 12:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 성적이 낮은 순서로 학생 출력하기

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 나동빈 저자님께서 파이썬의 기본 정렬 라이브러리를 이용해서 간결하게 풀어주셨습니다.
- JavaScript 에서도 Array 가 제공하는 sort 함수를 이용해서 간결하게 풀 수 있습니다.
- 이번에 키 포인트는 주어진 것은 이름과 점수지만, 정렬은 점수를 기준으로 하고, 출력은 이름으로 해야합니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `2
// 홍길동 95
// 이순신 77
// `.split("\n");

const solution = (input) => {
	const n = Number(input[0]);
	const arr = [];
	for (let i = 1; i < n + 1; i++) {
		const [name, point] = input[i].split(" ");
		arr[i - 1] = [name, Number(point)];
	}

	arr.sort((a, b) => a[1] - b[1]);

	let res = "";
	for (let i = 0; i < n; i++) {
		res += `${arr[i][0]} `;
	}

	return res;
};

console.log(solution(input));
```
