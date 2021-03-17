---
layout: post
title: "BOJ[6064] - 카잉 달력 by JavaScript"
date: 2021-03-17 16:15:00 +0900
categories: BOJ(BruteForce)
---

# 카잉 달력

## 문제

- [백준 6064번 - 카잉 달력](https://www.acmicpc.net/problem/6064)

## 언어

- JavaScript

## 문제 풀이 step 1

- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀면 됩니다.
- 범위는 40000 \* 40000 은 1600000000 으로 이대로는 할 수 없습니다.
- 하지만 중간 중간 제외헤도 되는 경우가 존재하므로, 그 부분을 제외한 경우만 보면 됩니다.
  - 핵심 변수는 **year** 와 **stepY** 입니다.
  - x 값은 고정시키고, stepY 값만 늘려가면서 주어진 y 값과 같은 경우가 생기는지 비교합니다.
  - 우선 year 와 stepY 를 주어진 x 값으로 초기화합니다.
  - 그리고 x 값을 고정시키며 stepY를 늘리기위해서는 year 와 stepY 를 m 씩 증가시켜야 합니다.
  - 왜냐하면 x + m 은 `x + m > m` 이니까 다시 `x + m - m` 을 해서 x 가 되기 때문입니다.
  - 증가시키면서 문제에서 주어진 경우가 있는지 없는지 비교합니다.
  - 반복은 stepY 가 한 바퀴를 돌아 다시 x 가 되기 전까지 진행합니다.

## 시행착오 (삽질)

- solution 함수 안에 m 과 n 의 크기를 비교하고, 더 작은 것이 앞으로 오게 하는 로직이 있습니다.
- 제가 생각한 대로면 크기를 비교하고, 작은 것이 앞으로 오게하는 로직이 없어도 정상적으로 돌아가야 했는데, 이 로직을 빼면 시간초과가 발생했습니다.
- 게속해서 고민한 결과 반례를 찾을 수 있었습니다.
- 만약 `[m, n, x, y] = [13, 8, 12, 7]` 일 경우 stepY 를 x 값인 12 로 초기화하고 다시 x 값이 될 때까지 반복을 할 텐데,
- 제가 구현한 로직대로면 매 반복마다 stepY 를 n 보다 작게 만들어서, 다시 x 값이 되는 경우가 발생하지 않아서 무한 반복에 빠지게 됩니다.
- 그래서 대소 비교를 하는 로직을 추가해야 했습니다.
- 대소 비교 대신 아예 year 값만 이용하는 방법도 또 하나의 해결책이 될 수 있을 것 같습니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const check = (m, n, x, y) => {
	// x는 고정
	let year = x;
	let stepY = year;

	do {
		// 주어진 y와 같은 경우
		if (stepY === y) break;

		year += m;
		stepY += m;
		while (stepY > n) stepY -= n;
	} while (stepY !== x);

	if (stepY === y) return year;
	else return -1;
};

const solution = (input) => {
	let T = parseInt(input[0]);

	let res = "";
	let index = 1;
	while (T-- > 0) {
		const [m, n, x, y] = input[index++].split(" ").map(Number);

		// 대소 비교 로직
		if (m < n) res += check(m, n, x, y) + "\n";
		else res += check(n, m, y, x) + "\n";
	}

	return res;
};

console.log(solution(input));
```
