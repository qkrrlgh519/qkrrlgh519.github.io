---
layout: post
title: "BOJ[16234] - 인구 이동 by JavaScript"
date: 2021-07-01 14:00:00 +0900
categories: BOJ(Implementation)
---

# 인구 이동

## 문제

- [백준 16234번 - 인구 이동](https://www.acmicpc.net/problem/16234)

## 언어

- JavaScript

## 순서도

1. BFS 를 이용해서 연합인 곳들 다 구하기
2. 연합인 곳들 모두 인구수 변환 해주기
3. 연합이 안 생길 때까지 1 번과 2 번 과정을 반복하기
4. 인구수를 변환한 횟수 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 로직은 순서도의 내용처럼
  - BFS 탐색을 이용해 땅 내에서 연합을 구하고, 각 연합은 인구 이동을 통해 인구수를 바꿉니다.
  - 더 이상 인구 이동을 할 수 없을 때까지 반복하고, 최종으로 인구 이동을 한 횟수를 출력합니다.
- 인구 이동에 관한 정보는 다음과 같습니다.
  - N x N 크기의 땅이 있고, 각각의 땅에는 나라가 하나씩 존재하며, r 행 c 열에 있는 나라에는 A[r][c] 명이 살고 있습니다.
  - 인접한 나라 사이에는 국경선이 존재합니다.
  - 국경선을 공유하는 두 나라의 인구 차이가 L 명 이상, R 명 이하라면, 두 나라는 공유하는 국경선을 하루 동안열고 연합이 됩니다.
  - 연합이란 국경선이 열려있어 인접한 칸만을 이용해 이동할 수 있으면, 그러한 나라들을 연합이라고 합니다.
  - N x N 크기의 땅에서 위의 조건에 따라 열어야 하는 모든 국경선을 열었다면, 인구 이동을 시작합니다.
  - 연합을 이루고 있는 각 칸의 인구수는 (연합의 인구수) / (연합을 이루고 있는 칸의 개수) 가 됩니다. 편의상 소수점은 버립니다.
  - 인구 이동이 끝나면, 연합을 해체하고, 모든 국경선을 닫습니다.
- 위의 인구 이동의 성질과 BFS 탐색을 이용해 푸는 문제입니다.

## 문제 풀이 step 2

- N x N 땅에서 방문하지 않은 각 나라마다 BFS 탐색을 수행하며 연합을 구합니다.
  - 연합이 될 수 있는 나라들은 서로의 인구수 차이가 L 명 이상 R 명 이하여야 합니다.
  - 이는 [백준 2667번 - 단지 번호 붙이기 풀이](<https://qkrrlgh519.github.io/boj(bfs)/2021/03/31/BOJ-BFS-2667.html>) 와 유사합니다.
  - 간단히 말하면, 연합이 될 수 있는 조건을 가지고 연결 요소의 개수를 구하는 것입니다.
- 각 연합끼리 인구 이동의 특징에 따라서 인구 이동을 합니다.
  - **(연합의 인구수) / (연합을 이루고 있는 칸의 개수)** 연산의 결과로 연합 내의 각 나라의 인구수를 변환해줍니다.
- 인구 이동을 한 번 할때마다 그 횟수를 세어줍니다.
- 더 이상 인구 이동을 하지 않을 때까지 위의 과정 (BFS 탐색, 인구 이동) 을 반복하며, 횟수를 세어줍니다.
- 그 횟수를 출력하면 정답이 됩니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 간단하지만, 꼭 다시 풀어볼 문제입니다.

## 소스 코드

```jsx
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

// BFS 탐색을 위해 Queue 구현, 배열을 써도 무방합니다.
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

// 다음에 이동할 좌표가 땅을 벗어나지 않는지 검사하는 함수
const isPossibleRoute = (n, to) =>
	0 <= to.x && to.x < n && 0 <= to.y && to.y < n;

// BFS 탐색
const BFS = (n, l, r, land, visited, start) => {
	const cx = [0, 0, 1, -1];
	const cy = [1, -1, 0, 0];

	const queue = new Queue();
	queue.enqueue(start);
	// 시작점을 연합에 추가
	const union = [start];
	// 연합의 인구수 총합
	let unionPopulation = land[start.x][start.y];
	// 방문 처리
	visited[start.x][start.y] = true;

	while (!queue.isEmpty()) {
		const from = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			const to = new Dot(from.x + cx[i], from.y + cy[i]);

			if (!isPossibleRoute(n, to)) continue;
			if (visited[to.x][to.y]) continue;
			// 인구수 차이가 L 명 이상 R 명 이하여야만 연합에 추가할 수 있습니다.
			const populationDiff = Math.abs(land[from.x][from.y] - land[to.x][to.y]);
			if (populationDiff < l || r < populationDiff) continue;

			queue.enqueue(to);
			// 연합에 추가
			union.push(to);
			// 연합의 인구수 총합 갱신
			unionPopulation += land[to.x][to.y];
			// 방문 처리
			visited[to.x][to.y] = true;
		}
	}

	// 만약 연합 배열에 처음 시작점밖에 없다면, 이는 연합이 없는 것
	if (union.length === 1) return [false, null, null];
	else return [true, union, unionPopulation];
};

// 인구 이동을 표현하는 함수
const movePopulation = (land, unions, unionPopulations) => {
	for (let i = 0; i < unions.length; i++) {
		// (연합의 인구수) / (연합을 이루고 있는 칸의 개수) 연산을 통해서 각 칸의 인구수를 계산
		const avg = parseInt(unionPopulations[i] / unions[i].length);

		// 각 칸마다 계산한 인구수로 수정
		for (let j = 0; j < unions[i].length; j++) {
			const [x, y] = [unions[i][j].x, unions[i][j].y];
			land[x][y] = avg;
		}
	}
};

const solution = (input) => {
	const [n, l, r] = input[0].split(" ").map(Number);
	const land = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	// 인구 이동의 횟수
	let cnt = 0;
	while (true) {
		const visited = Array.from({length: n}, () => Array(n).fill(false));
		const unions = [];
		const unionPopulations = [];

		// 땅의 각 칸마다 BFS 탐색을 통해서 연합 구하기
		for (let i = 0; i < n; i++) {
			for (let j = 0; j < n; j++) {
				if (visited[i][j] === true) continue;

				const [isExist, union, unionPopulation] = BFS(
					n,
					l,
					r,
					land,
					visited,
					new Dot(i, j)
				);

				if (isExist) {
					unions.push(union);
					unionPopulations.push(unionPopulation);
				}
			}
		}

		// BFS 탐색을 수행해도 연합이 하나도 나오지 않는다면 인구 이동 종료
		if (unions.length === 0) break;

		// 인구 이동
		movePopulation(land, unions, unionPopulations);
		// 인구 이동의 횟수 갱신
		cnt++;
	}

	return cnt;
};

console.log(solution(input));
```
