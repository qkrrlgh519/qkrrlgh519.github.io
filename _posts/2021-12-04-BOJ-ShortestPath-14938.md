---
layout: post
title: "BOJ[14938] - 서강그라운드 by JavaScript"
date: 2021-12-04 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 서강그라운드

## 문제

- [백준 14938번 - 서강그라운드](https://www.acmicpc.net/problem/14938)

## 언어

- JavaScript

## 순서도

1. 자신에서 자신으로 가는 간선 비용 0 으로 초기화
2. 그래프 정보를 이용해서 간선 비용 초기화
3. 플로이드 워셜 알고리즘을 수행해서 모든 간선간의 최단 경로 구하기
4. 모든 정점에 대해서, 각 정점 별로 몇 개의 아이템을 얻을 수 있는지 파악
5. 얻을 수 있는 아이템 중 최댓값 반환

## 문제 풀이 step 1

- 예은이는 요즘 가장 인기가 있는 게임 서강그라운드를 즐기고 있습니다.
- 서강그라운드는 여러 지역중 하나의 지역에 낙하산을 타고 낙하하여, 그 지역에 떨어져 있는 아이템들을 이용해 서바이벌을 하는 게임입니다.
- 서강그라운드에서 1 등을 하면 보상으로 치킨을 주는데, 예은이는 단 한번도 치킨을 먹을 수가 없었습니다.
- 자신이 치킨을 못 먹는 이유는 실력 때문이 아니라 아이템운이 없어서라고 생각한 예은이는 낙하산에서 떨어질 때 각 지역에 아이템들이 몇 개 있는지 알려주는 프로그램을 개발했습니다.
- 하지만 어디로 낙하해야 자신이 수색 범위 내에서 가장 많은 아이템을 얻을 수 있는지 알 수 없었습니다.
- 각 지역은 일정한 길이 I (1 <= I <= 15) 의 길로 다른 지역과 연결되어 있고 이 길은 양방향 통행이 가능합니다.
- 예은이는 낙하한 지역을 중심으로 거리가 수색 범위 m (1 <= m <= 15) 이내의 모든 지역의 아이템을 습득 가능하다고 할 때, 예은이가 얻을 수 있는 아이템의 최대 개수를 구하는 문제입니다.

## 문제 풀이 step 2

- 본 문제는 플로이드 워셜 알고리즘을 적용하면 쉽게 풀 수 있습니다.
- 각 지역마다 얻을 수 있는 아이템을 확인하기 위해서는 각 지역별로 다른 지역까지의 이동 거리를 알아야 하기 때문입니다.
- 플로이드 워셜 알고리즘을 이용해서 각 지역별로 다른 지역까지의 이동 거리를 모두 구하고, 각 지역별로 몇 개의 아이템을 얻을 수 있는지 파악하고, 그 최대값을 반환하면 정답입니다.
  - 한 지역을 기준으로 각 지역별로 이동거리가 수색 범위 m 보다 작다면, 그 지역의 아이템을 획득하 수 있습니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = Math.floor(Number.MAX_SAFE_INTEGER / 2);

const solution = (input) => {
	const [n, m, r] = input[0].split(" ").map(Number);

	const items = input[1].split(" ").map(Number);

	const dist = Array.from(Array(n), () => Array(n).fill(INF));

	// 자신에서 자신으로 가는 경로는 0 으로 초기화
	for (let i = 0; i < n; i++) {
		dist[i][i] = 0;
	}

	// 입력 정보를 이용해서 간선 정보 입력
	for (let i = 2; i < 2 + r; i++) {
		const [x, y, weight] = input[i].split(" ").map(Number);

		dist[x - 1][y - 1] = weight;
		dist[y - 1][x - 1] = weight;
	}

	// 플로이드 워셜 알고리즘 적용
	for (let k = 0; k < n; k++) {
		for (let i = 0; i < n; i++) {
			for (let j = 0; j < n; j++) {
				dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
			}
		}
	}

	// 지역 별로 다른 지역과의 이동 거리가 수색 범위 m 보다 작다면 그 지역의 아이템을 획득할 수 있음
	// 이를 이용해서 각 지역 별로 얻을 수 있는 아이템의 개수 파악
	let max = 0;
	for (let i = 0; i < n; i++) {
		let cnt = 0;
		for (let j = 0; j < n; j++) {
			if (dist[i][j] <= m) cnt += items[j];
		}

		max = Math.max(max, cnt);
	}

	// 얻을 수 있는 아이템의 최대값 반환
	return max;
};

console.log(solution(input));
```
