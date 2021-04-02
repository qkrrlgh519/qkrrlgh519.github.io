---
layout: post
title: "이것이 코딩테스트다 - [Greedy] 큰 수의 법칙 by JavaScript"
date: 2021-04-02 13:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 큰 수의 법칙

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 그리디 알고리즘의 방식으로 풀 수 있습니다.
- 가장 큰 수와 두 번째로 큰 수를 적절하게 더하는 방식으로 간단하게 풀 수 있습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5 8 3
// 2 4 5 4 6`.split("\n");

const solution = (input) => {
	let [n, m, k] = input[0].split(" ").map(Number);
	const arr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => b - a);
	const FIRST_BIG = arr[0];
	const SECOND_BIG = arr[1];

	let res = 0;
	let cnt = 0;
	while (m-- > 0) {
		if (cnt === 3) {
			cnt = 0;
			res += SECOND_BIG;
			continue;
		}

		res += FIRST_BIG;
		cnt++;
	}

	return res;
};
```

---

## 다른 방식의 문제 풀이 step 1

- 위의 방식은 한 번씩 횟수를 늘려가면서 더하는 과정을 수행하기 때문에 시간복잡도가 O(N)입니다.
- 더하는 과정 중간 중간에 중복이 존재하므로, 그런 중복을 없애는 방식을 통해서 시간복잡도를 향상시킬 수 있습니다.
- 나동빈 저자님의 풀이를 참고해서 풀었습니다.
  - 큰 수들과 작은 수가 결국은 수열 형태를 띄게 됩니다.
  - 그 수열의 형태를 파악하고, 나누기 연산을 통해서 반복되는 연산을 제거할 수 있습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5 8 3
// 2 4 5 4 6`.split("\n");

const solution = (input) => {
	let [n, m, k] = input[0].split(" ").map(Number);
	const arr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => b - a);
	const FIRST_BIG = arr[0];
	const SECOND_BIG = arr[1];
	const SEQUENCE = FIRST_BIG * k + SECOND_BIG;

	let res = 0;
	let cnt = parseInt(m / (k + 1));
	let mod = m % (k + 1);

	res += SEQUENCE * cnt;
	if (mod !== 0) res += mod * FIRST_BIG;

	return res;
};

console.log(solution(input));
```
