---
layout: post
title: "이것이 코딩테스트다 - [ShortestPath] 미래 도시 by JavaScript"
date: 2021-12-14 12:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 미래 도시

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 많은 회사가 모요있는 공중 미래 도시가 있습니다. 방문 판매원 A 는 K 번 회사를 거쳐서 X 번 회사에 가기를 희망합니다. 이 판매원을 위해서 최단 경로를 구해주는 문제입니다.
- 플로이드 워셜 알고리즘을 통해서 모든 도시 간의 최단 경로를 구하고, `A - K - X` 의 최단 경로를 구하면 되겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");
// const input = `5 7
// 1 2
// 1 3
// 1 4
// 2 4
// 3 4
// 3 5
// 4 5
// 4 5`.split("\n");

const INF = Math.floor(Number.MAX_SAFE_INTEGER / 2);

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const [mid, end] = input[m + 1].split(" ").map(Number);

	const graph = Array.from(Array(n + 1), () => Array(n + 1).fill(INF));

	// 입력 정보를 통해서 거리 정보 초기화
	for (let i = 1; i < m + 1; i++) {
		const [from, to] = input[i].split(" ").map(Number);

		graph[from][to] = 1;
		graph[to][from] = 1;
	}

	// 자신에서 자신으로 가는 경로는 0 으로 초기화
	for (let i = 1; i < n + 1; i++) {
		graph[i][i] = 0;
	}

	// 플로이드 워셜 알고리즘 적용
	for (let k = 1; k < n + 1; k++) {
		for (let i = 1; i < n + 1; i++) {
			for (let j = 1; j < n + 1; j++) {
				graph[i][j] = Math.min(graph[i][j], graph[i][k] + graph[k][j]);
			}
		}
	}

	// A - K - X 경로의 최단 경로
	const res = graph[1][mid] + graph[mid][end];

	if (res >= INF) return -1;
	return res;
};

console.log(solution(input));
```
