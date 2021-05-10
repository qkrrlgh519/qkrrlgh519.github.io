---
layout: post
title: "BOJ[15662] - 톱니바퀴 (2) by JavaScript"
date: 2021-05-10 11:00:00 +0900
categories: BOJ(Implementation)
---

# 톱니바퀴 (2)

## 문제

- [백준 15662번 - 톱니바퀴 (2)](https://www.acmicpc.net/problem/15662)

## 언어

- JavaScript

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- [백준 14891번 - 톱니바퀴 풀이](<https://qkrrlgh519.github.io/boj(implementation)/2021/05/08/BOJ-Implementation-14891.html>) 와 같은 방식으로 풀 수 있습니다.
- 단, 다른 부분이 조금 있습니다.
  - 이번 문제에서는 톱니바퀴의 개수가 4 개가 아닌, T 개 입니다.
  - 그리고, 마지막에 점수를 계산하지 않고 톱니바퀴 중에서 12 시 방향이 S 극인 톱니바퀴의 개수를 출력하면 됩니다.
- 따라서 본 포스트에서는 달라지는 부분만 작성하겠습니다.

## 문제 풀이 step 2

- 톱니바퀴의 개수가 T 개이기 때문에, 톱니바퀴의 정보를 저장할 때 4 개가 아닌 T 개의 톱니바퀴 정보를 저장하면 되겠습니다.
- 그리고 회전하는 함수를 구현할 때, 회전 여부와 회전 방향 정보를 수집하는 과정에서 오른쪽으로 한 개씩 검사하는 과정의 범위가 `0 ~ 3` 에서 `0 ~ T - 1` 로 늘어납니다.
- 그리고 톱니바퀴를 다 회전한 이후에 각 톱니바퀴의 점수를 계산하지 않고, 12 시 방향이 S 극인 톱니바퀴의 개수만 세어주면 됩니다.
- 위 부분을 제외하고 달라지는 부분은 없습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const rotate = (n, gears, num, direction) => {
	const [left, right] = [6, 2];

	// 1 : 시계, -1 : 반시계, 0 : 무회전
	const directions = Array(n).fill(0);
	directions[num] = direction;

	let now = num;
	// 달라진 부분 1. 범위 증가
	while (now < n - 1) {
		const before = now;
		now = ++now;

		// 왼쪽 톱니바퀴가 회전하지 않으면, 오른쪽 톱니 바퀴 회전 X
		if (directions[before] === 0) continue;

		// 왼쪽 톱니바퀴와 오른쪽 톱니바퀴의 맞닿은 톱니가 같을 경우 회전 X
		if (gears[before][right] === gears[now][left]) continue;

		// 왼쪽 톱니바퀴가 회전하며, 맞닿은 톱니가 다른 경우 회전 O
		directions[now] = directions[before] * -1;
	}

	now = num;
	while (0 < now) {
		const before = now;
		now = --now;

		// 오른쪽 톱니바퀴가 회전하지 않으면, 왼쪽 톱니 바퀴 회전 X
		if (directions[before] === 0) continue;

		// 오른쪽 톱니바퀴와 왼쪽 톱니바퀴의 맞닿은 톱니가 같을 경우 회전 X
		if (gears[before][left] === gears[now][right]) continue;

		// 오른쪽 톱니바퀴가 회전하며, 맞닿은 톱니가 다른 경우 회전 O
		directions[now] = directions[before] * -1;
	}

	for (let i = 0; i < n; i++) {
		if (directions[i] === 0) continue;

		if (directions[i] === 1) gears[i].unshift(gears[i].pop());
		else gears[i].push(gears[i].shift());
	}
};

const solution = (input) => {
	const n = Number(input[0]);
	const gears = input.slice(1, n + 1).map((v) => v.split("").map(Number));
	const k = Number(input[n + 1]);
	const commands = input
		.slice(n + 2, n + 2 + k)
		.map((v) => v.split(" ").map(Number));

	for (let i = 0; i < k; i++) {
		let [num, direction] = commands[i];
		rotate(n, gears, num - 1, direction);
	}

	let res = 0;
	// 달라진 부분 2. 점수 계산이 아닌 개수 세기
	for (let i = 0; i < n; i++) {
		// 0 : N 극, 1 : S 극
		if (gears[i][0] === 0) continue;
		else res += 1;
	}

	return res;
};

console.log(solution(input));
```
