---
layout: post
title: "BOJ[11404] - 플로이드 by JavaScript"
date: 2021-11-29 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 플로이드

## 문제

- [백준 11404번 - 플로이드](https://www.acmicpc.net/problem/11404)

## 언어

- JavaScript

## 순서도

1. 그래프 정보를 이용해서 간선 비용 초기화
2. 자신에서 자신으로 가는 간선 비용 0 으로 초기화
3. 플로이드 워셜 알고리즘을 수행해서 모든 간선간의 최단 경로 구하기
4. 갈 수 없는 경로는 0 으로 초기화

## 문제 풀이 step 1

- n 개의 도시가 있습니다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 m 개의 버스가 있습니다.
- 각 버스는 한 번 사용할 때 필요한 비용이 있습니다.
- 모든 도시의 쌍 (A, B)에 대해서 도시 A 에서 B 로 가는 필요한 비용의 최솟값을 구하는 문제입니다.

## 문제 풀이 step 2

- 본 문제는 제목에서도 알 수 있듯이 플로이드 워셜 알고리즘을 적용하는 문제입니다.
- 본 알고리즘은 [플로이드 워셜 알고리즘 포스트](<https://qkrrlgh519.github.io/algorithm(graph)/2021/11/24/Algorithm-Graph-Floyd.html>)에 정리해두었습니다.
- 플로이드 워셜 알고리즘을 적용 후에 갈 수 없는 경로는 0 으로 초기화한 후 적절히 문자열로 변환해서 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const INF = Number.MAX_SAFE_INTEGER;

	const n = Number(input[0]);
	const edges = Number(input[1]);

	// 주어진 입력을 통해서 간선 정보 초기화
	const dist = Array.from(Array(n), () => Array(n).fill(INF));
	for (let i = 2; i < edges + 2; i++) {
		const [from, to, weight] = input[i].split(" ").map(Number);

		if (dist[from - 1][to - 1] > weight) dist[from - 1][to - 1] = weight;
	}

	// 자신에서 자신으로 가는 경로는 0 으로 초기화
	for (let i = 0; i < n; i++) {
		dist[i][i] = 0;
	}

	// 플로이드 워셜 알고리즘 적용
	for (let k = 0; k < n; k++) {
		for (let i = 0; i < n; i++) {
			for (let j = 0; j < n; j++) {
				dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
			}
		}
	}

	// 갈 수 없는 경로는 0 으로 초기화
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			if (dist[i][j] === INF) dist[i][j] = 0;
		}
	}

	// 적절히 문자열로 변환 후 반환
	return dist.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```
