---
layout: post
title: "이것이 코딩테스트다 - [Greedy] 숫자 카드 게임 by JavaScript"
date: 2021-04-05 12:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 숫자 카드 게임

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 그리디 알고리즘의 방식으로 풀 수 있습니다.
- 입력을 2 차원 배열 형태로 바꿔주고, 각 행의 최솟값들 중에서 최댓값을 찾으면 정답이 됩니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `2 4
// 7 3 1 8
// 3 3 3 4`.split("\n");

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	let max = 0;
	const arr = [];
	for (let i = 1; i < n + 1; i++) {
		arr[i - 1] = input[i].split(" ").map(Number);

		let min = Math.min(...arr[i - 1]);
		if (min > max) max = min;
	}

	return max;
};

console.log(solution(input));
```
