---
layout: post
title: "BOJ[4485] - 녹색 옷 입은 애가 젤다지? by JavaScript"
date: 2021-10-14 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 녹색 옷 입은 애가 젤다지?

## 문제

- [백준 4485번 - 녹색 옷 입은 애가 젤다지?](https://www.acmicpc.net/problem/4485)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현
2. 다익스트라 알고리즘 함수 구현
3. [0, 0] 에서 [n - 1][n - 1] 까지 가는 모든 경로를 루피를 최소한으로 잃는 경로로 구하기
4. [0, 0] 에서 [n - 1][n - 1] 까지 가는데 잃은 루피 출력

## 문제 풀이 step 1

- 젤다의 전설 게임에서 화폐의 단위는 루피입니다.
- 그런데 간혹 도둑루피라 불리는 검정색 루피도 존재합니다. 이걸 획득하면 오히려 소지한 루피가 감소됩니다.
- 젤다의 전설 시리즈의 주인공 링크는 지금 도둑루피만 가득한 N x N 크기의 동굴에서 [0][0]번 칸에 위치하고 있습니다.
- 링크는 이 동굴의 반대편 출구인 [n - 1][n - 1]번 칸까지 이동해야 합니다.
- 동굴의 각 칸마다 도둑루피가 있는데, 이 칸을 지나면 해당 도둑루피의 크기만큼 소지금을 잃게 됩니다.
- 링크는 잃는 금액을 최소로 하여 동굴 건너편까지 이동해야 하며, 한 번에 상하좌우 인접한 곳으로 1 칸씩 이동할 수 있습니다.
- 링크가 잃을 수밖에 없는 최소 금액은 얼마일까요?

## 문제 풀이 step 2

- 본 문제는 다익스트라 알고리즘을 이용하면 쉽게 풀 수 있습니다.
- 각 칸의 도둑루피를 비용으로 간주해서, 이 비용(도둑루피)이 최소가 되는 경로를 구하면 되겠습니다.
- 우선, 최소 힙을 구현하고, 주어진 입력을 n x n 크기의 2 차원 배열로 만듭니다. 이 배열이 곧 그래프입니다.
- 그리고 [0, 0] 에서 상, 하, 좌, 우 이렇게 4 방향 중에서 갈 수 있는 칸으로 이동합니다.
- 이와 동시에 distnace 배열을 갱신하고, 최소힙에 원소를 넣는 방식으로 다익스트라 알고리즘을 적용합니다.
- 마지막으로 [n - 1][n - 1] 까지 최소 비용(최소 도둑루피)이 드는 경로룰 구하고, 최소 비용(최소 도둑루피)을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 여태까지 푼 다익스트라 문제는 정점과 간선이 주어지고 이를 인접리스트 형식의 그래프로 만들어서 푸는 방식이었습니다.
- 반면, 이번 문제는 n x n 크기의 2 차원 배열이 주어지고 이를 그래프로 간주해서 푸는 방식이었습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = Number.MAX_SAFE_INTEGER;

// 4 방향
const dir = [
	[0, 1],
	[0, -1],
	[-1, 0],
	[1, 0],
];

// n x n 배열을 넘어가는지 검사
const isPossibleRoute = (n, x, y) => 0 <= x && x < n && 0 <= y && y < n;

// 최소힙 구현
class MinHeap {
	constructor() {
		this.bucket = [null];
	}

	insert(item) {
		let index = this.bucket.length;

		while (index !== 1 && item[0] < this.bucket[parseInt(index / 2)][0]) {
			this.bucket[index] = this.bucket[parseInt(index / 2)];
			index = parseInt(index / 2);
		}

		this.bucket[index] = item;
	}

	delete() {
		let parentIdx = 1;
		let childIdx = 2;
		const item = this.bucket[1];
		const lastItem = this.bucket.pop();

		while (childIdx < this.bucket.length) {
			if (
				childIdx + 1 < this.bucket.length &&
				this.bucket[childIdx][0] > this.bucket[childIdx + 1][0]
			) {
				childIdx += 1;
			}

			if (lastItem[0] <= this.bucket[childIdx][0]) break;

			this.bucket[parentIdx] = this.bucket[childIdx];

			parentIdx = childIdx;
			childIdx *= 2;
		}

		if (!this.isEmpty()) this.bucket[parentIdx] = lastItem;

		return item;
	}

	isEmpty() {
		return this.bucket.length === 1;
	}
}

// 다익스트라 알고리즘 구현
const dijkstra = (n, cave, start) => {
	const minHeap = new MinHeap();
	const distance = Array.from({length: n}, () => Array(n).fill(INF));

	// 시작점 갱신
	const [sx, sy] = start;
	minHeap.insert([cave[sx][sy], [sx, sy]]);
	distance[sx][sy] = cave[sx][sy];

	while (!minHeap.isEmpty()) {
		const [fromCost, [fx, fy]] = minHeap.delete();

		// 이미 지난 경로라면 무시
		if (distance[fx][fy] < fromCost) continue;

		for (let i = 0; i < 4; i++) {
			const [tx, ty] = [fx + dir[i][0], fy + dir[i][1]];

			if (!isPossibleRoute(n, tx, ty)) continue;

			// 비용이 더 작다면, 갱신 및 힙에 넣기
			const toCost = fromCost + cave[tx][ty];
			if (distance[tx][ty] > toCost) {
				distance[tx][ty] = toCost;
				minHeap.insert([toCost, [tx, ty]]);
			}
		}
	}

	return distance;
};

const solution = (input) => {
	let res = "";

	let testCase = 1;
	let index = 0;
	while (true) {
		const n = Number(input[index++]);
		if (n === 0) break;

		const cave = input
			.slice(index, index + n)
			.map((v) => v.split(" ").map(Number));

		const distance = dijkstra(n, cave, [0, 0]);

		res += `Problem ${testCase++}: ${distance[n - 1][n - 1]}\n`;
		index += n;
	}

	return res;
};

console.log(solution(input));
```
