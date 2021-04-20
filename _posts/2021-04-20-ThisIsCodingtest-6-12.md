---
layout: post
title: "이것이 코딩테스트다 - [Sort] 두 배열의 원소 교체 by JavaScript"
date: 2021-04-20 17:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 두 배열의 원소 교체

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 한 개의 배열을 오름차순 정렬하고, 다른 배열을 내림차순 정렬을 합니다.
- 그리고 0 번 index 에서 시작해서 K 번 만큼 오름 차순 배열에서의 작은 값들을 내림 차순 배열에서의 큰 값과 교환해주면 됩니다. (단, 작을 때만 교환해야 합니다.)

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5 3
// 1 2 5 4 3
// 5 5 6 6 5`.split("\n");

const solution = (input) => {
	const [n, k] = input[0].split(" ").map(Number);
	const aArr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);
	const bArr = input[2]
		.split(" ")
		.map(Number)
		.sort((a, b) => b - a);

	for (let i = 0; i < k; i++) {
		if (aArr[i] >= bArr[i]) continue;
		[aArr[i], bArr[i]] = [bArr[i], aArr[i]];
	}

	return aArr.reduce((ac, v) => ac + v);
};

console.log(solution(input));
```
