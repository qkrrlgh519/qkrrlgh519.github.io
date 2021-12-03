---
layout: post
title: "BOJ[2660] - 회장뽑기 by JavaScript"
date: 2021-12-03 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 회장뽑기

## 문제

- [백준 2660번 - 회장뽑기](https://www.acmicpc.net/problem/2660)

## 언어

- JavaScript

## 순서도

1. 자신에서 자신으로 가는 간선 비용 0 으로 초기화
2. 그래프 정보를 이용해서 간선 비용 초기화
3. 플로이드 워셜 알고리즘을 수행해서 모든 간선간의 최단 경로 구하기
4. 모든 정점에 대해서 각 정점별로 가장 먼 정점의 거리가 몇인지 확인
5. 구한 거리들 중에서 거리가 가장 짧은 정점들을 후보로 뽑기

## 문제 풀이 step 1

- 월드컵 축구의 응원을 위한 모임에서 회장을 선출하려고 합니다.
- 이 모임은 만들어진지 얼마 되지 않았기 때문에 회원 사이에 서로 모르는 사람도 있지만, 몇 사람을 통하면 모두가 서로 알 수 있습니다.
- 각 회원은 다른 회원들과 가까운 정도에 따라 점수를 받게 됩니다.
- 예를 들어 어느 회원이 다른 모든 회원과 친구이면, 이 회원의 점수는 1 점입니다.
- 어느 회원의 점수가 2 점이면, 다른 모든 회원이 친구이거나 친구의 친구임을 말합니다.
- 또한 어느 회원의 점수가 3 점이면, 다른 모든 회원이 친구이거나, 친구의 친구이거나, 친구의 친구의 친구임을 말합니다.
- 4 점, 5 점 등은 같은 방법으로 정해집니다.
- 각 회원의 점수를 정할 때 주의할 점은 어떤 두 회원이 친구사이이면서 동시에 친구의 친구사이이면, 이 두 사람은 친구사이라고 봅니다.
- 회장은 회원들 중에서 점수가 가장 작은 사람이 됩니다. 회장의 점수와 회장이 될 수 있는 모든 사람을 찾는 문제입니다.

## 문제 풀이 step 2

- 본 문제는 플로이드 워셜 알고리즘을 적용하면 쉽게 풀 수 있습니다.
- 문제에서 각 회원의 점수는 각 회원 별로 다른 사람과의 친구 단계가 몇 단계인지에 따라서 결정됩니다.
  - 친구 단계 중에서도 가장 먼 단계의 값으로 결정됩니다.
- 친구 단계가 몇 단계인지는 그래프로 나타내면 각 정점 간의 거리라고 볼 수 있습니다.
- 따라서 각 친구 단계를 그래프로 나타낸 후에 플로이드 워셜 알고리즘을 적용하면 됩니다.
- 적용 후에 각 회원 별로 가장 거리가 먼 친구의 거리가 몇인지 확인하고, 그 값이 최소가 되는 회원들을 뽑아서 반환하면 정답입니다.
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
	const n = Number(input[0]);

	const dist = Array.from(Array(n), () => Array(n).fill(INF));

	// 자신에서 자신으로 가능 정점 초기화
	for (let i = 0; i < n; i++) {
		dist[i][i] = 0;
	}

	// 입력 정보에 따라서 친구 관계 (간선 정보) 입력
	let index = 1;
	while (true) {
		const [x, y] = input[index++].split(" ").map(Number);

		if (x === -1 && y === -1) break;

		dist[x - 1][y - 1] = 1;
		dist[y - 1][x - 1] = 1;
	}

	// 플로이드 워셜 알고리즘 적용
	for (let k = 0; k < n; k++) {
		for (let i = 0; i < n; i++) {
			for (let j = 0; j < n; j++) {
				dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
			}
		}
	}

	// 각 회원 별로 가장 단계가 먼 (거리가 먼) 친구 관계의 거리 조사
	let min = INF;
	const arr = Array(n).fill(0);
	for (let i = 0; i < n; i++) {
		let max = 0;

		for (let j = 0; j < n; j++) {
			if (i === j) continue;
			max = Math.max(max, dist[i][j]);
		}

		// 최소 단계 (최소 거리) 값 구하기
		arr[i] = max;
		min = Math.min(min, max);
	}

	// 최소 단계 (최소 거리) 값에 따라서 후보 추출
	let res = [];
	for (let i = 0; i < n; i++) {
		if (arr[i] === min) res.push(i + 1);
	}

	return `${min} ${res.length}\n${res.join(" ")}`;
};

console.log(solution(input));
```
