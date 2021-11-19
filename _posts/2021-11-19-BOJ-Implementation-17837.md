---
layout: post
title: "BOJ[17837] - 새로운 게임 2 by JavaScript"
date: 2021-11-19 12:00:00 +0900
categories: BOJ(Implementation)
---

# 새로운 게임 2

## 문제

- [백준 17837번 - 새로운 게임 2](https://www.acmicpc.net/problem/17837)

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

- 새로운 게임은 N x N 인 체스판에서 진행되고, 사용하는 말의 개수는 K 개 입니다.
- 말은 원판모양이고, 하나의 말 위에 다른 말을 올릴 수 있습니다.
- 체스판의 각 칸은 흰색, 빨간색, 파란색 중 하나로 색칠되어 있습니다.
- 게임은 체스판 위에 말 K 개를 놓고 시작합니다.
  - 말은 1 번부터 K 번까지 번호가 매겨져 있고, 이동 방향도 미리 정해져 있습니다.
  - 이동 뱡향은 위, 아래, 왼쪽, 오른쪽 4 가지 중 하나입니다.
- 턴 한 번은 1 번 말부터 K 번 말까지 순서대로 이동시키는 것입니다.
- 한 말이 이동할 때 위에 올려져 있는 말도 함께 이동합니다.
- 말의 이동 방향에 있는 칸에 따라서 말의 이동이 다르며 아래와 같습니다.
  - 턴이 진행되던 중에 말이 4 개 이상 쌓이는 순간 게임이 종료됩니다.
  - A 번 말이 이동하려는 칸이
    - 흰색인 경우에는 그 칸으로 이동합니다. 이동하려는 칸에 말이 이미 있는 경우에는 가자 위에 A 번 말을 올려 놓습니다.
      - A 번 말의 위에 다른 말이 있는 경우에는 A 번 말과 위에 있는 모든 말이 이동합니다.
      - 예를 들어, A, B, C 로 쌓여있고, 이동하려는 칸에 D, E 가 있는 경우에는 A 번 말이 이동한 후에는 D, E, A, B, C 가 됩니다.
    - 빨간색인 경우에는 이동한 후에 A 번 말과 그 위에 있는 모든 말의 쌓여있는 순서를 반대로 바꿉니다.
      - A, B, C 가 이동하고, 이동하려는 칸에 말이 없는 경우에는 C, B, A 가 됩니다.
      - A, D, F, G 가 이동하고, 이동하려는 칸에 말이 E, C, B 로 있는 경우에는 E, C, B, G, F, D, A 가 됩니다.
    - 파란색인 경우에는 A 번 말의 이동 방향을 반대로 하고 한 칸 이동합니다. 방향을 반대로 바 꾼 후에 이동하려는 칸이 파란색인 경우에는 이동하지 않고 가만히 있습니다.
    - 체스 판을 벗어나는 경우에는 파란색과 같은 경우입니다.

## 문제 풀이 step 2

- 본 문제는 [백준 17780번 - 새로운 게임 풀이](<https://qkrrlgh519.github.io/boj(implementation)/2021/10/29/BOJ-Implementation-17780.html>)와 거의 동일한 풀 수 있습니다.
  - 단, 이번 문제는 말을 움직일 때, 맨 밑에 있지 않아도 움직일 수 있다는 점이 다릅니다.
  - 이에 따라, 말을 움직일 때 움직이려는 말과 그 위에 있는 말들을 별도로 분리하는 로직이 추가됩니다.
- 우선, 주어진 정보를 이용해서 2 개의 배열을 생성합니다.
  - 첫 번째 배열은 각 말의 정보를 담는 1 차원 배열로, 각 말의 번호마다 [행, 열, 방향] 정보를 담습니다.
  - 두 번째 배열은 각 좌표마다 말들이 어떤 순서로 쌓여있는지 담는 2 차원 배열로, 각 말의 번호가 순서대로 쌓여있습니다.
- 이 두 개의 배열을 이용해서 게임의 로직을 진행합니다.
  - 첫 번째 배열을 순회하며, 각 말을 순서대로 이동시킵니다.
  - 각 말의 좌표와 방향 정보를 이용해서 이동하려는 칸을 구하고, 그 칸의 색에 맞게 로직을 분기합니다.
    - 우선, 이동하려는 말과 그 위의 말을 별도로 분리합니다.
    - 이동하려는 칸이 흰색이면, 별도로 분리한 말들을 그냥 이동시키면 됩니다.
    - 이동하려는 칸이 빨간색이면, 별도로 분리한 말들의 순서를 뒤집고 이동시키면 됩니다.
    - 이동하려는 칸이 파란색이면, 이동하려는 말의 방향을 바꾸고 별도로 분리한 말들을 반대 방향으로 한 칸 이동시킵니다.
      - 이 때, 반대 방향으로 이동하려는 칸이 흰 색이면, 별도로 분리한 말들을 그냥 이동시키면 됩니다.
      - 빨간색이면, 별도로 분리한 말들의 순서를 뒤집고 이동시키면 됩니다.
      - 파란색이거나 벽이면, 별도로 분리한 말들을 원래 위치에 원상복귀시킵니다.
- 게임의 로직을 진행해가며 말이 4 개 이상 쌓이는지 검사하고, 4 개 이상 쌓였으면 턴 수를 출력하면 정답입니다.
- 반대로, 턴이 1000 번을 넘어가거나 게임이 종료되지 않으면 -1 을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 이 문제를 풀기 전에 이미 비슷한 문제를 풀었었기에 조금 간단하게 풀 수 있었던 것 같습니다.
- 중요한 것은 말을 원 상태로 되돌리는 경우를 제외한 모든 경우는 말을 움직이게 되므로, 그 때만 말이 4 개 이상 쌓이는지 검사하면 됩니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 총 4 가지 방향
const dir = [
	[0, 1],
	[0, -1],
	[-1, 0],
	[1, 0],
];

// 체스판을 벗어나는지 검사하는 함수
const isPossibleRoute = (n, x, y) => 0 <= x && x < n && 0 <= y && y < n;

// 이동하려는 말과 그 위에 있는 말들을 별도로 분리하는 함수
const getPiecesWillMove = (posBoard, x, y, i) => {
	const tempPieces = [];

	while (posBoard[x][y].length) {
		const num = posBoard[x][y].pop();
		tempPieces.push(num);

		if (num === i) break;
	}
	return tempPieces;
};

// 말을 움직이는 함수
const movePieces = (pieces, posBoard, tempPieces, nx, ny) => {
	for (let i = tempPieces.length - 1; i >= 0; i--) {
		const num = tempPieces[i];
		[pieces[num][0], pieces[num][1]] = [nx, ny];

		posBoard[nx][ny].push(num);
	}
};

// 말을 움직이며, 4 개 이상 쌓이는지 검사하는 함수
const movePiecesAndCheck = (n, k, board, pieces, posBoard) => {
	let turn = 1;

	while (turn <= 1000) {
		// 총 k 개의 말에 대해서 로직 진행
		for (let i = 0; i < k; i++) {
			let [x, y, d] = pieces[i];

			// 움직이려는 말과 그 위에 있는 말들을 별도로 분리
			const tempPieces = getPiecesWillMove(posBoard, x, y, i);
			let [nx, ny] = [x + dir[d][0], y + dir[d][1]];

			if (isPossibleRoute(n, nx, ny) && board[nx][ny] === 0) {
				// 이동하려는 칸인 흰 칸인 경우
			} else if (isPossibleRoute(n, nx, ny) && board[nx][ny] === 1) {
				// 이동하려는 칸이 빨간 칸인 경우
				tempPieces.reverse();
			} else {
				// 이동하려는 칸이 벽이거나 파란 칸인 경우
				d % 2 === 0 ? (d += 1) : (d -= 1);
				pieces[i][2] = d;

				[nx, ny] = [x + dir[d][0], y + dir[d][1]];

				// 반대로 이동하려는 칸이 벽이거나 파란 칸인 경우
				if (!isPossibleRoute(n, nx, ny) || board[nx][ny] === 2) {
					// 원상 복귀
					movePieces(pieces, posBoard, tempPieces, x, y);
					continue;
				}

				// 반대로 이동하려는 칸이 빨간 칸인 경우
				if (board[nx][ny] === 1) tempPieces.reverse();
			}

			// 말 이동
			movePieces(pieces, posBoard, tempPieces, nx, ny);

			// 말이 4 개 이상 쌓였는지 검사
			if (posBoard[nx][ny].length >= 4) return turn;
		}

		// turn 갱신
		turn += 1;
	}

	return -1;
};

const solution = (input) => {
	const [n, k] = input[0].split(" ").map(Number);
	const board = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));
	const pieces = input.slice(n + 1).map((v) => v.split(" ").map(Number));

	const posBoard = Array.from(Array(n), () => Array.from(Array(n), () => []));
	for (let i = 0; i < k; i++) {
		const [x, y, d] = pieces[i];

		// 두 개의 배열에 말들의 정보 담기
		pieces[i] = [x, y, d].map((v) => v - 1);
		posBoard[x - 1][y - 1].push(i);
	}

	const result = movePiecesAndCheck(n, k, board, pieces, posBoard);
	return result;
};

console.log(solution(input));
```
