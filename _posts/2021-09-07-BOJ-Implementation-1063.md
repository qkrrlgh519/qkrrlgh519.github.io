---
layout: post
title: "BOJ[1063] - 킹 by JavaScript"
date: 2021-09-07 12:00:00 +0900
categories: BOJ(Implementation)
---

# 킹

## 문제

- [백준 1063번 - 킹](https://www.acmicpc.net/problem/1063)

## 언어

- JavaScript

## 순서도

1. 킹 옮기기
2. 만약 옮긴 킹의 위치가 체스판을 넘어간다면 옮기지 않기
3. 만약 옮긴 킹의 위치에 돌이 있다면, 돌을 킹과 같은 방향으로 한 칸 옮기기
4. 만약 옮긴 돌의 위치가 체스판을 넘어간다면 옮기지 않기
5. n 번 만큼 위의 과정 반복하기

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 8 X 8 크기의 체스판에 킹과 돌이 하나 있습니다.
- 킹과 돌의 현재 위치가 주어지는 데, 알파벳 하나와 숫자 하나로 이루어져 있습니다.
  - 알파벳은 열을 의미하고, 숫자는 행을 의미합니다.
  - 열은 가장 왼쪽 열이 A 이고, 가장 오른쪽 열이 H 까지이고, 행은 가장 아래가 1 이고, 가장 위가 8 입니다.
- 킹은 상, 하, 좌, 우, 우상, 좌상, 우하, 좌하 이렇게 8 가지 방향으로 움직일 수 잇습니다.
- 그리고 킹이 이동할 때, 이동한 위치에 돌이 있다면, 킹이 이동한 방향과 동일한 방향으로 한 칸 이동시킵니다.
- 만약 킹이나 돌을 움직였을 때, 체스판 밖으로 나간다면, 그 경우는 건너 뛰고 다음 이동을 합니다.
- 입력으로 킹이 어떻게 움직여야 하는지 주어질 때, 킹과 돌의 최종 위치를 구하는 문제입니다.

## 문제 풀이 step 2

- 위에서 설명한 체스판 위에서의 킹과 돌의 이동 규칙에 따라서 로직을 작성하면 됩니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 문제의 말을 이해 못해서, 조금 시행 착오를 겪었습니다. 킹과 돌을 동일한 위치로 이동시키는 줄 알았는데, 킹과 돌의 동선이 겹칠 경우 돌을 밀어내는 것이었습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 알파벳으로 주어지는 열을 숫자로 바꾸는 함수
const changeToNum = (str) => {
	const charCode = str.charCodeAt(0);

	if ("A".charCodeAt(0) <= charCode && charCode <= "H".charCodeAt(0))
		return charCode - "A".charCodeAt(0);
	return Number(str) - 1;
};

// 숫자로 바꾼 열을 다시 알파벳으로 바꾸는 함수
const changeToAlphabet = (num) => {
	return String.fromCharCode(num + "A".charCodeAt(0));
};

// 체스 판을 넘어가는 지 검사하는 함수
const isPossibleRoute = (y, x) => 0 <= y && y < 8 && 0 <= x && x < 8;

// 총 8 가지 방향으로 이동시키는 함수
const move = (pos, direction) => {
	let [y, x] = pos;

	switch (direction) {
		case "R":
			[y, x] = [y + 1, x];
			break;
		case "L":
			[y, x] = [y - 1, x];
			break;
		case "B":
			[y, x] = [y, x - 1];
			break;
		case "T":
			[y, x] = [y, x + 1];
			break;
		case "RT":
			[y, x] = [y + 1, x + 1];
			break;
		case "LT":
			[y, x] = [y - 1, x + 1];
			break;
		case "RB":
			[y, x] = [y + 1, x - 1];
			break;
		case "LB":
			[y, x] = [y - 1, x - 1];
			break;
	}

	return [y, x];
};

const moveKing = (king, stone, direction) => {
	// 킹을 이동 시키기
	let [ky, kx] = move(king, direction);
	let [sy, sx] = stone;

	// 킹이 체스판을 넘어간다면, 이동시키지 않기
	if (!isPossibleRoute(ky, kx)) return;

	// 킹이 이동한 위치에 돌이 있지 않다면, 킹의 위치 확정하고 return
	if (kx !== sx || ky !== sy) {
		king[0] = ky;
		king[1] = kx;
		return;
	}

	// 킹이 이동한 위치에 돌이 있다면, 킹과 동일한 방향으로 돌 이동시키기
	[sy, sx] = move(stone, direction);

	// 돌이 체스판을 넘어간다면, 이동시키지 않기
	if (!isPossibleRoute(sy, sx)) return;

	// 돌이 체스판을 넘어가지 않는다면, 킹과 돌의 위치 확정시키기
	king[0] = ky;
	king[1] = kx;
	stone[0] = sy;
	stone[1] = sx;
};

const solution = (input) => {
	let [king, stone, n] = input[0].split(" ");

	king = king.split("").map(changeToNum);
	stone = stone.split("").map(changeToNum);

	// n 번 만큼 킹 이동시키기
	for (let i = 1; i < 1 + Number(n); i++) {
		const direction = input[i];

		moveKing(king, stone, direction);
	}

	// 다시 알파벳 형태로 변환 후 출력
	return `${changeToAlphabet(king[0]) + (king[1] + 1)}\n${
		changeToAlphabet(stone[0]) + (stone[1] + 1)
	}`;
};

console.log(solution(input));
```
