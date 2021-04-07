---
layout: post
title: "이것이 코딩테스트다 - [Implementation] 왕실의 나이트 by JavaScript"
date: 2021-04-07 16:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 왕실의 나이트

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 문제에서 주어진 대로 구현하는 구현 유형의 문제입니다.
- "8 x 8" 크기의 체스판에서 나이트의 위치가 주어졌을 때 나이트가 이동할 수 있는 다음 위치의 개수를 구해야 합니다.
- 우선 입력으로 주어지는 나이트의 위치를 보기 편한 숫자 형태로 바꿔 줍니다.
  - `a1` 은 `00` 으로
  - `c2` 는 `21` 로
- 문제에서 주어진 나이트의 이동 경우 2 가지를 이용해서 이동할 수 있는 경우 8 가지를 정의합니다.
  - `const cx = [2, 2, -2, -2, -1, 1, -1, 1];`
  - `const cy = [-1, 1, -1, 1, 2, 2, -2, -2];`
- 8 가지의 경우를 이용해서 나이트의 다음 위치가 체스판을 벗어나지 않는 경우만 세어줍니다.
- 위 과정을 통해 세어준 경우의 수가 정답이 됩니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `a1`.split("\n");

const isPossibleRoute = (x, y) => 0 <= x && x < 8 && 0 <= y && y < 8;

const solution = (input) => {
	let [x, y] = input[0].split("");

	// 주어지는 입력 숫자 형태로 바꾸기
	[x, y] = [x.charCodeAt(0) - "a".charCodeAt(0), y - 1];

	const cx = [2, 2, -2, -2, -1, 1, -1, 1];
	const cy = [-1, 1, -1, 1, 2, 2, -2, -2];

	let ans = 0;
	for (let i = 0; i < 8; i++) {
		const [nx, ny] = [x + cx[i], y + cy[i]];
		if (isPossibleRoute(nx, ny)) ans++;
	}

	return ans;
};
```
