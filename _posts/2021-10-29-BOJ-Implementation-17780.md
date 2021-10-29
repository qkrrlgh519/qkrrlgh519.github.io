---
layout: post
title: "BOJ[17780] - 새로운 게임 by JavaScript"
date: 2021-10-29 12:00:00 +0900
categories: BOJ(Implementation)
---

# 새로운 게임

## 문제

- [백준 17780번 - 새로운 게임](https://www.acmicpc.net/problem/17780)

## 언어

- JavaScript

## 순서도

1. 각 말의 정보를 담는 1 차원 배열 생성
2. 각 좌표마다 말들이 어떤 순서로 쌓여있는지 정보를 담는 2 차원 배열 생성
3. 위에 선언한 두 개의 배열을 이용해서 말들을 이동시키고, 말이 4 개 이상 쌓이는지 검사
4. 4 개 이상 쌓였으면, 게임을 진행한 횟수를 반환
5. 4 개 이상 쌓이지 않았으면, 계임을 계속 진행하다가 진행한 횟수가 1000 번을 넘어가면 -1 을 반환

## 문제 풀이 step 1

- 새로운 게임은 크기가 N x N 인 체스판에서 진행되고, 사용하는 말의 개수는 K 개 입니다.
- 말은 원판모양이고, 하나의 말 위에 다른 말을 올릴 수 있습니다.
- 체스판의 각 칸은 흰색, 빨간색, 파란색 중 하나로 색칠되어 있습니다.
- 게임은 체스판 위에 말을 K 개를 놓고 시작합니다. 말은 1 번부터 K 번까지 번호가 매겨져 있고, 이동 방향도 미리 정해져 있습니다.
- 이동 방향은 위, 아래, 왼쪽, 오른쪽 4 가지 중 하나입니다.
- 턴 한 번은 1 번 말부터 K 번 말까지 순서대로 이동시키는 것입니다.
- 한 말이 이동할 때 위에 올려져 있는 말도 함께 이동하며, 가장 아래에 있는 말만 이동할 수 있습니다.
- 말의 이동 방향에 있는 칸에 따라서 말의 이동이 다르며 아래와 같습니다.
- 턴이 진행되던 중에 말이 4 개 이상 쌓이는 순간 게임이 종료됩니다.

## 문제 풀이 step 2

- 위의 정보를 기반해서 새로운 게임을 진행하다가 게임이 정상적으로 종료되면 게임의 턴 수를 출력하고, 턴이 1000 을 넘어가거나 종료되지 않는 경우 -1 을 출력하는 문제입니다.
- 우선, 주어진 정보를 이용해서 2 개의 배열을 생성합니다.
  - 첫 번째 배열은 각 말의 정보를 담는 1 차원 배열로, 각 말의 번호마다 [행, 열, 방향] 정보를 담습니다.
  - 두 번째 배열은 각 좌표마다 말들이 어떤 순서로 쌓여있는지 담는 2 차원 배열로, 각 말의 번호가 순서대로 쌓여있습니다.
- 이 두 개의 배열을 이용해서 게임의 로직을 진행합니다.
  - 첫 번째 배열을 순회하며, 각 말을 순서대로 이동시킵니다.
  - 각 말의 좌표와 방향 정보를 이용해서 이동하려는 칸을 구하고, 그 칸의 색에 맞게 로직을 분기합니다.
    - 이동하려는 칸이 흰색이면, 그냥 이동하면 됩니다.
    - 이동하려는 칸이 빨간색이면, 두 번째 배열의 순서를 뒤집은 다음에 이동하면 됩니다.
    - 이동하려는 칸이 파란색이면, 말의 방향을 바꾸고 반대 방향으로 한 칸 이동합니다.
      - 이 때, 반대 방향으로 이동한 칸이 흰색이면 그냥 이동하면 됩니다.
      - 빨간색이면 두 번째 배열의 순서를 뒤집은 다음에 이동하면 됩니다.
      - 파란색이거나 벽이면 방향만 바꾸고 이동하지 않습니다.
- 게임의 로직을 진행해가며 말이 4 개 이상 쌓이는지 검사하고, 4 개 이상 쌓였으면 턴 수를 출력하면 정답입니다.
- 반대로, 턴이 1000 번을 넘어가거나 게임이 종료되지 않으면 -1 을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 크게 어려운 문제는 아니었는데, 삽질을 상당히 많이 했습니다.
- 이동하려는 칸이 파란색일 때, 이동하려는 반대 칸도 마찬가지로 흰색, 빨간색, 파란색, 벽 검사를 진행해야하는데 파란색과 벽 검사만 진행했습니다.
- 문제에 나온 예시를 꼼꼼하게 확인하고 따라해보는 것이 중요한 것 같습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 4 가지 방향 정의
const dir = [null, [0, 1], [0, -1], [-1, 0], [1, 0]];

// 벽을 넘어가는 지 검사하는 함수
const isPossibleRoute = (n, x, y) => 0 <= x && x < n && 0 <= y && y < n;

// 이동하려는 칸이 확실하게 정해졌을 때, 말을 이동시키는 함수
const move = (pieces, board, x, y, nx, ny) => {
	while (board[x][y].length) {
		const pieceNum = board[x][y].shift();

		pieces[pieceNum][0] = nx;
		pieces[pieceNum][1] = ny;

		board[nx][ny].push(pieceNum);
	}
};

// 말을 이동시키고, 말이 4 개 이상 쌓였는지 검사하는 함수
const movePiecesAndCheck = (n, k, boardData, pieces, board) => {
	// k 개의 말을 순회하며,
	for (let i = 0; i < k; i++) {
		let [x, y, d] = pieces[i];

		// 바닥에 있는 말이 아니라면 제외
		if (board[x][y][0] !== i) continue;

		// 이동하려는 칸
		let [nx, ny] = [x + dir[d][0], y + dir[d][1]];

		if (isPossibleRoute(n, nx, ny) && boardData[nx][ny] === 0) {
			// 흰색
		} else if (isPossibleRoute(n, nx, ny) && boardData[nx][ny] === 1) {
			// 빨간색
			board[x][y].reverse();
		} else {
			// 파란색 또는 벽
			d % 2 === 1 ? (d += 1) : (d -= 1); // 방향 전환
			pieces[i][2] = d;

			// 반대 칸
			[nx, ny] = [x + dir[d][0], y + dir[d][1]];

			// 반대 칸도 마찬가지로 파란색인지, 벽인지 검사
			if (!isPossibleRoute(n, nx, ny) || boardData[nx][ny] === 2) continue;

			// 반대 칸도 마찬가지로 빨간색인지 검사
			if (boardData[nx][ny] === 1) board[x][y].reverse();
		}

		// 이동하려는 칸이 정해졌으니, 말 이동
		move(pieces, board, x, y, nx, ny);

		// 말이 4 개 이상 쌓였는지 검사
		if (board[nx][ny].length >= 4) return true;
	}

	return false;
};

const solution = (input) => {
	const [n, k] = input[0].split(" ").map(Number);
	const boardData = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	const pieces = [];
	const board = Array.from({length: n}, () =>
		Array.from({length: n}, () => [])
	);
	for (let i = n + 1, pieceNum = 0; i < n + k + 1; i++, pieceNum++) {
		const [x, y, d] = input[i].split(" ").map(Number);

		pieces[pieceNum] = [x - 1, y - 1, d];
		board[x - 1][y - 1].push(pieceNum);
	}

	let turn = 0;
	while (true) {
		// 턴 갱신
		turn += 1;

		// 말 이동 및 검사
		if (movePiecesAndCheck(n, k, boardData, pieces, board) === true) break;

		// 턴이 1000 을 넘어가면 -1 반환
		if (turn > 1000) return -1;
	}

	// 게임이 정상 종료시 턴 수 반환
	return turn;
};

console.log(solution(input));
```
