---
layout: post
title: "BOJ[1956] - 운동 by JavaScript"
date: 2021-12-01 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 운동

## 문제

- [백준 1956번 - 운동](https://www.acmicpc.net/problem/1956)

## 언어

- JavaScript

## 순서도

1. 자신에서 자신으로 가는 간선 비용 0 으로 초기화
2. 그래프 정보를 이용해서 간선 비용 초기화
3. 플로이드 워셜 알고리즘을 수행해서 모든 간선간의 최단 경로 구하기
4. 길이가 최소가 되는 사이클 찾기

## 문제 풀이 step 1

- V 개의 마을과 E 개의 도로로 구성되어 있는 도시가 있습니다.
- 도로는 마을과 마을 사이에 놓여 있으며, 일방 통행 도로입니다.
- 마을에는 1 번부터 V 번까지 번호가 매겨져 있습니다.
- 도로를 따라 운동을 하기 위한 경로를 찾으려고 합니다.
- 운동을 한 후에는 다시 시작점으로 돌아오는 것이 좋기 때문에, 사이클을 찾아야 합니다.
- 하지만 운동은 너무 귀찮기 때문에, 사이클을 이루는 도로의 길이의 합이 최소가 되도록 찾으려고 합니다.
- 도로의 정보가 주어졌을 때, 도로의 길이의 합이 가장 작은 사이클을 찾는 문제입니다.
- 두 마을을 왕복하는 경우도 사이클에 포함됩니다.

## 문제 풀이 step 2

- 본 문제는 플로이드 워셜 알고리즘을 적용하면 쉽게 풀 수 있습니다.
- 문제에서 두 마을을 왕복하는 경우도 사이클에 포함된다 했습니다.
- 따라서 플로이드 워셜 알고리즘을 이용해서 모든 도시 간의 최단 경로를 구합니다.
- 그리고 모든 두 마을의 쌍에 대해서 사이클 거리를 구하고 그 중에서 최솟값을 구하면 정답입니다.
- 만약, 사이클이 없다면 -1 을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = parseInt(Number.MAX_SAFE_INTEGER / 2);

const solution = (input) => {
	const [v, e] = input[0].split(" ").map(Number);

	const dist = Array.from(Array(v), () => Array(v).fill(INF));

	// 자신에서 자신으로 가능 경로 0 으로 초기화
	for (let i = 0; i < v; i++) {
		dist[i][i] = 0;
	}

	// 입력 정보를 통해서 거리 정보 초기화
	for (let i = 1; i < e + 1; i++) {
		const [a, b, w] = input[i].split(" ").map(Number);

		dist[a - 1][b - 1] = w;
	}

	// 플로이드 워셜 알고리즘 적용
	for (let k = 0; k < v; k++) {
		for (let i = 0; i < v; i++) {
			for (let j = 0; j < v; j++) {
				dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
			}
		}
	}

	// 모든 도시 쌍에 대해서 최단 사이클 찾기
	let min = INF;
	for (let i = 0; i < v; i++) {
		for (let j = 0; j < v; j++) {
			if (i === j) continue;

			min = Math.min(min, dist[i][j] + dist[j][i]);
		}
	}

	// 최단사이클이 없다면 -1 반환
	if (min === INF) min = -1;
	return min;
};

console.log(solution(input));
```
