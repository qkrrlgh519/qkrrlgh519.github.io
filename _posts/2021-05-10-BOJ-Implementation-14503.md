---
layout: post
title: "BOJ[14503] - 로봇 청소기 by JavaScript"
date: 2021-05-10 11:30:00 +0900
categories: BOJ(Implementation)
---

# 로봇 청소기

## 문제

- [백준 14503번 - 로봇 청소기](https://www.acmicpc.net/problem/14503)

## 언어

- JavaScript

## 순서도

- 이번 문제의 순서도는 문제에 나와있는 그 순서도와 똑같습니다.

1. 현재 위치 청소
2. 현재 위치에서 현재 방향 기준 왼쪽 방향으로 탐색 시작
   1. 왼쪽 방향에 아직 청소하지 않은 공간이 존재하면, 그 방향으로 회전 및 한 칸 전진 후 1번으로
   2. 왼쪽 방향에 청소 공간이 없다면, 회전 후 2번으로
   3. 네 방향 모두 청소 되어있거나 벽이면, 한 칸 후진
   4. 네 방향 모두 청소 되어있거나 벽이고, 뒤쪽 방향이 벽이면 작동 멈춤

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- 특히나 본 문제의 경우는 문제에서 주어진 순서대로 구현하면 오류없이 풀 수 있는 문제입니다.
- 설명은 소스코드에 주석으로 작성하겠습니다.

## 후기

- 구현 유형의 문제는 다루는 범위가 커서 디버깅하는 부분이 많이 어려운 것 같습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const isWall = (arr, x, y) => arr[x][y] === 1;

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	let [x, y, d] = input[1].split(" ").map(Number);
	const arr = input.slice(2).map((v) => v.split(" ").map(Number));

	// 청소를 했는지 안했는지 구별하기 위한 배열 생성
	const isClean = Array.from({length: n}, () => Array(m).fill(false));

	// 로봇의 움직임 정의
	// 0 : 북, 1 : 동, 2 : 남, 3 : 서
	const cx = [-1, 0, 1, 0];
	const cy = [0, 1, 0, -1];

	// 청소한 공간의 개수를 세는 변수
	let cnt = 0;

	while (true) {
		// 1) 현 위치 청소
		// 문제에서 이미 청소되어있는 칸을 또 청소하지 않는다고 했기 때문에, 조건을 달아줍니다.
		if (!isClean[x][y]) {
			isClean[x][y] = true;
			cnt += 1;
		}

		// 네 방향 모두 완성했는지 세는 변수
		let isFourSuccess = 0;

		// 왼쪽 방향에 청소 공간이 있는지 체크하는 변수
		let isExistInLeft = false;

		// 2) 현 위치 기준 왼쪽 방향부터 탐색
		for (let i = 0; i < 4; i++) {
			// 2.a, 2.b) 왼쪽 방향 회전
			d - 1 < 0 ? (d += 3) : (d -= 1);

			// 2.a) 왼쪽 방향 청소 공간 존재 -> 한 칸 전진
			const [nx, ny] = [x + cx[d], y + cy[d]];
			if (!isWall(arr, nx, ny) && !isClean[nx][ny]) {
				[x, y] = [nx, ny];
				isExistInLeft = true;
				break;
			}

			// 왼쪽 방향에 청소 공간이 존재하지 않으면 1 증가
			isFourSuccess += 1;
		}

		// 2.a) 1 번부터 진행
		if (isExistInLeft) continue;

		// 2.c, 2.d) 네 방향 모두 청소 되어 있거나 벽인 경우
		if (isFourSuccess === 4) {
			// 뒤쪽 방향
			const [nx, ny] = [x - cx[d], y - cy[d]];

			if (!isWall(arr, nx, ny)) {
				// 2.c. 뒤쪽 방향 벽이 아니면 -> 한 칸 후진
				[x, y] = [nx, ny];
				continue;
			} else {
				// 2.d. 뒤쪽 방향 벽이면 -> 작동 멈춤
				break;
			}
		}
	}

	// 청소한 공간의 개수 return
	return cnt;
};

console.log(solution(input));
```
