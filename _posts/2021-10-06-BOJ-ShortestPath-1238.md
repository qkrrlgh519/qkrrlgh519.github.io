---
layout: post
title: "BOJ[1238] - 파티 by JavaScript"
date: 2021-10-06 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 파티

## 문제

- [백준 1238번 - 파티](https://www.acmicpc.net/problem/1238)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현
2. 방향 그래프 생성, 역 방향 그래프 생성
3. 두 개의 그래프에 대해서 다익스트라 알고리즘 수행
4. 왕복 시간이 가장 오래 걸리는 학생의 소요 시간 구하기

## 문제 풀이 step 1

- N 개의 숫자로 구분된 각각의 마을에 한 명의 학생이 살고 있습니다.
- 이 N 명의 학생이 X 번 마을에 모여서 파티를 벌일 예정입니다.
- 이 마을 사이에는 총 M 개의 단방향 도로들이 있고, i 번째 길을 지나는데 Ti 의 시간을 소비합니다.
- 각각의 학생들은 파티에 참석하기 위해 걸어가서 다시 그들의 마을로 돌아와야 합니다. 하지만 이 학생들은 워낙 게을러서 최단 시간에 오고 가기를 원합니다.
- 이 도로들은 단방향이기 때문에 아마 그들이 오고 가는 길이 다를지도 모릅니다. N 명의 학생들 중 오고 가는데 가장 많은 시간을 소비하는 학생의 소요 시간을 구하는 문제입니다.

## 문제 풀이 step 2

- 다익스트라 알고리즘을 이용하면 쉽게 풀 수 있습니다.
- 우선, 최소 힙을 구현하고, 주어진 입력에 대해서 인접리스트 형식의 방향 그래프를 생성합니다.
  - 추가로 간선의 방향을 바꾼 역방향 그래프를 생성합니다.
- 방향 그래프와 역방향 그래프에 대해서 다익스트라 알고리즘을 수행합니다.
  - 정방향 그래프에 알고리즘을 적용해 나온 결과 distance 배열은 X 번 마을에서 각 학생이 자신의 마을로 돌아가는 최소 소요 시간을 담고 있습니다.
  - 역방향 그래프에 알고리즘을 적용해 나온 결과 distance 배열은 각 학생이 자신의 마을에서 X 번 마을로 가는 최소 소요 시간을 담고 있습니다.
- 결과로 나온 두 개의 distance 배열을 각 index 에 맞게 더해주면 각 학생이 X 번 마을을 왕복하는데 걸리는 최소 소요 시간을 구할 수 있습니다.
  - 위 과정을 통해서 나온 왕복 시간 중에서 가장 오래 걸리는 왕복 시간을 구하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 처음에는 각 학생마다 다익스트라 알고리즘을 적용하는 방식을 통해서 문제를 해결했습니다. 이 방법은 불필요한 탐색을 한다는 점에서 좋지 않은 알고리즘이었습니다.
- 반면, 주어진 방향 그래프에서 역방향 그래프를 생성해서 다익스트라 알고리즘을 적용하는 방법은 불필요한 탐색을 하지 않기 때문에 메모리와 시간을 크게 줄일 수 있었던 해결책이었습니다.
- **역방향 그래프를 생성해서 다익스트라 알고리즘을 적용하면, 모든 정점에서 시작점으로 오는 최소 비용을 구할 수 있다는 것**이 명쾌하게 이해가 가지 않아서 고민을 좀 많이 했습니다.
  - 고민의 결과,
  - 간선의 방향을 뒤집은 역방향 그래프에서 다익스트라 알고리즘을 적용하면, 이 또한 시작점에서 모든 정점으로 도달하는 최소 비용을 구할 수 있습니다.
  - 하지만, 각 최소 비용이 구해지는데 사용된 간선들은 뒤집혀진 상태에서 구해진 것이고, 이를 다시 뒤집으면 각 도시에서 시작점으로 오는 간선들임을 알 수 있습니다. (다시 뒤집어야 원래 그래프인 것이구요)
  - 따라서 구한 최소 비용들은 모든 정점에서 시작점에 도달하는 최소 비용인 것입니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = Number.MAX_SAFE_INTEGER;

// 최소 힙 구현
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
		if (this.isEmpty()) return;

		let parentIdx = 1;
		let childIdx = 2;
		let item = this.bucket[1];
		let lastItem = this.bucket.pop();

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
const dijkstra = (n, graph, start) => {
	const minHeap = new MinHeap();
	const distance = Array(n + 1).fill(INF);

	minHeap.insert([0, start]);
	distance[start] = 0;

	while (!minHeap.isEmpty()) {
		const [fromCost, from] = minHeap.delete();

		if (distance[from] < fromCost) continue;

		for (let i = 0; i < graph[from].length; i++) {
			const to = graph[from][i][0];
			const toCost = fromCost + graph[from][i][1];

			if (distance[to] > toCost) {
				distance[to] = toCost;
				minHeap.insert([toCost, to]);
			}
		}
	}

	return distance;
};

const solution = (input) => {
	const [n, m, x] = input[0].split(" ").map(Number);

	// 정방향 그래프와 역방향 그래프 생성
	const graph = Array.from({length: n + 1}, () => []);
	const reverseGraph = Array.from({length: n + 1}, () => []);
	for (let i = 1; i < m + 1; i++) {
		const [from, to, cost] = input[i].split(" ").map(Number);

		graph[from].push([to, cost]);
		reverseGraph[to].push([from, cost]);
	}

	// 각 정점마다 X 로 가는 최단 시간 구하기
	const goingCost = dijkstra(n, reverseGraph, x);

	// X 에서 각 정점으로 가는 최단 시간 구하기
	const commingCost = dijkstra(n, graph, x);

	// 왕복 시간의 최댓값 찾기
	let max = 0;
	for (let i = 1; i <= n; i++) {
		let wholeCost = goingCost[i] + commingCost[i];

		if (max < wholeCost) {
			max = wholeCost;
		}
	}

	return max;
};

console.log(solution(input));
```
