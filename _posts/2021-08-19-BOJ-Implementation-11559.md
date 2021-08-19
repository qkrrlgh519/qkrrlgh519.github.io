---
layout: post
title: "BOJ[11559] - Puyo Puyo by JavaScript"
date: 2021-08-19 13:00:00 +0900
categories: BOJ(Implementation)
---

# Puyo Puyo

## 문제

- [백준 11559번 - Puyo Puyo](https://www.acmicpc.net/problem/11559)

## 언어

- JavaScript

## 순서도

1. 같은 색 뿌요가 4개 이상 상하좌우로 연결되어 있는 경우 찾고, 연결된 같은 색 뿌요들을 삭제
2. 뿌요들이 없어지고 나서 위에 다른 뿌요들이 있다면, 아래로 내리기
3. 1, 2 번 과정을 반복하며 연쇄 횟수 세기
4. 더 이상 삭제할 뿌요가 없을 경우 반복을 종료하고 연쇄 횟수 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 뿌요 뿌요 게임은
  - 필드에 여러 가지 색깔의 뿌요가 있습니다.
  - 뿌요는 중력의 영향을 받아 아래에 바닥이나 다른 뿌요가 나올 때까지 아래로 떨어집니다.
  - 뿌요를 놓고 난 후, 같은 색 뿌요가 4개 이상 상하좌우로 연결되어 있으면 연결된 같은 색 뿌요들이 한꺼번에 없어집니다. (1 연쇄)
  - 뿌요들이 없어지고 나서 위에 다른 뿌요들이 있다면, 중력의 영향을 받아 차례대로 아래로 떨어집니다.
  - 아래로 떨어지고 나서 다시 같은 색의 뿌요들이 4개 이상 모이게 되면, 연결된 같은 색 뿌요들이 한꺼번에 없어집니다. (1 연쇄)

## 문제 풀이 step 2

- 위의 뿌요 뿌요 게임에 따른 설명대로, 뿌요 뿌요 게임을 진행하고, 연쇄의 횟수를 세는 문제입니다.
- 우선, 필드의 모든 칸에 대해서 BFS 탐색을 수행합니다.
  - 탐색을 수행하며, 연결된 같은 색 뿌요들을 찾습니다. 이 때, 뿌요들의 개수는 4개 이상이여야 합니다.
  - 뿌요들의 개수가 4개 이상인 그룹들을 모두 지웁니다.
- 그리고 뿌요들을 아래로 떨어뜨립니다.
  - BFS 탐색을 수행하며 연결된 같은 색 뿌요들이 지워졌다면, 위에 있는 뿌요들은 바닥이나 다른 뿌요를 만날 때까지 아래로 떨어져야 합니다.
- 이렇게 [BFS 탐색, 뿌요 아래로 이동] 의 두 과정을 반복해가며 연쇄의 횟수를 셉니다.
- 만약, BFS 탐색시 지울 뿌요 그룹이 없다면 반복을 종료하고, 연쇄의 횟수를 출력합니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

class Dot {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}
}

// 간단한 Queue 구현
class Queue {
	constructor() {
		this.bucket = [];
		this.front = -1;
		this.rear = -1;
	}

	enqueue(data) {
		this.bucket[++this.rear] = data;
	}

	dequeue() {
		return this.bucket[++this.front];
	}

	isEmpty() {
		return this.front === this.rear;
	}
}

// 다음에 탐색할 좌표가 필드 내의 좌표인지 검사하는 함수
const isPossibleRoute = (n, m, dot) =>
	0 <= dot.x && dot.x < n && 0 <= dot.y && dot.y < m;

// BFS 탐색 함수
// 같은 색 뿌요가 4개 이상 상하좌우로 연결된 그룹이 있는지 검사하는 함수
const BFS = (n, m, board, visited, i, j) => {
	const cx = [0, 0, 1, -1];
	const cy = [1, -1, 0, 0];

	const queue = new Queue();
	const eraseArr = [];

	queue.enqueue(new Dot(i, j));
	visited[i][j] = true;
	eraseArr.push(new Dot(i, j));

	while (!queue.isEmpty()) {
		const from = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			const to = new Dot(from.x + cx[i], from.y + cy[i]);

			if (!isPossibleRoute(n, m, to)) continue;
			if (visited[to.x][to.y]) continue;
			if (board[from.x][from.y] !== board[to.x][to.y]) continue;

			queue.enqueue(to);
			visited[to.x][to.y] = true;
			eraseArr.push(to);
		}
	}

	if (eraseArr.length >= 4) {
		// 같은 색 뿌요가 4개 이상 상하좌우로 연결되어 있다면,
		for (let dot of eraseArr) {
			board[dot.x][dot.y] = ".";
		}
		return true;
	} else {
		// 같은 색 부요가 4개 이상 상하좌우로 연결되어 있지 않다면,
		return false;
	}
};

// 뿌요를 아래로 내리는 함수
const downPuyo = (n, m, board) => {
	for (let j = 0; j < m; j++) {
		let diff = 0;

		for (let i = n - 1; i >= 0; i--) {
			if (board[i][j] === ".") diff += 1;
			else
				[board[i][j], board[i + diff][j]] = [board[i + diff][j], board[i][j]];
		}
	}
};

const solution = (input) => {
	const board = input.map((v) => v.split(""));
	const [n, m] = [board.length, board[0].length];

	let chain = 0;
	while (true) {
		let cnt = 0;
		const visited = Array.from({length: n}, () => Array(m).fill(false));

		// 지우는 과정
		for (let i = 0; i < n; i++) {
			for (let j = 0; j < m; j++) {
				if (board[i][j] === ".") continue;
				if (visited[i][j]) continue;

				if (BFS(n, m, board, visited, i, j) === true) cnt += 1;
			}
		}

		// 지운 뿌요 그룹이 하나도 없다면, 반복 종료
		if (cnt === 0) break;
		// 지운 뿌요 그룹이 하나라도 있다면, 연쇄 횟수 증가
		chain += 1;

		// 내려오는 과정
		downPuyo(n, m, board);
	}

	return chain;
};

console.log(solution(input));
```
