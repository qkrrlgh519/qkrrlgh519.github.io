---
layout: post
title: "BOJ[15649] - N과 M (1) by JavaScript"
date: 2021-03-18 20:50:00 +0900
categories: BOJ(BackTracking)
---

# N과 M (1)

## 문제

- [백준 15649번 - N과 M (1)](https://www.acmicpc.net/problem/15649)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 1 ~ N 까지의 자연수 중에서 중복 없이 M 개를 골라야 합니다.
- 그러니 조건은 M 개입니다. 즉, M 개를 고른 상황이 되면 반복을 종료하면서 진행해나가면 됩니다.
- 그리고 중복 없이 M 개를 골라야 하니까 고른 수와 안 고른 수를 구별하기 위한 변수가 필요하겠습니다.
- 이렇게 적어놓고 보니 DFS 방식과 많이 유사한 것을 느낄 수 있습니다.
- 즉, 방문 처리(중복 처리)를 하면서 한 노드(1 ~ N 까지의 자연수)씩 탐색하다가 탐색한 노드의 개수가 M개가 되는 순간까지만 탐색을 진행하면 되겠습니다.
- 그리고 DFS와 다른 점은 다른 경로도 탐색해보기 위해서 탐색 후 돌아오면서 방문 처리를 취소하는 것과 시작 노드를 정해주진 않는 다는 것입니다.

## 후기

- 이 알고리즘은 모든 순열을 탐색할 때에도 사용이 가능합니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const rec = (n, m, res, step, visited, depth) => {
	// 조건 만족 시 반복 종료
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	// 탐색할 노드
	for (let i = 1; i < n + 1; i++) {
		if (visited[i]) continue;

		// 방문 처리
		visited[i] = true;
		// 탐색한 노드 기록
		step.push(i);
		rec(n, m, res, step, visited, depth + 1);
		visited[i] = false;
		step.pop();
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	const res = [];
	const step = [];
	const visited = Array(n + 1).fill(false);
	rec(n, m, res, step, visited, 0);

	return res.join("\n");
};

console.log(solution(input));
```
