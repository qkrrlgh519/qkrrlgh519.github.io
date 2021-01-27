---
layout: post
title: "Counting Sort (계수 정렬)"
date: 2021-01-27 23:30:00 +0900
categories: Algorithm(Graph)
---

# 1. 깊이 우선 탐색 (Depth First Search)

### 01. 정의

- 그래프의 **모든 정점을 발견**하는 가장 단순하고 고전적인 방법
- 현재 정점과 인접한 간선들을 하나씩 검사하다가, **아직 방문하지 않은 정점으로 향하는 간선이 있다면 그 간선을 무조건 따라간다.** 이 과정에서 더이상 갈 곳이 없는 막힌 정점에 도달하면 포기하고, 마지막에 따라왔던 간선을 따라 뒤로 돌아간다.

### 02. 과정

![DFS](/public/img/Graph/DFS.JPG)

### 03. 특징

- 탐색의 각 과정에서 가능한 한 그래프 안으로 '깊이' 들어가려고 시도하며, **막힌 정점에 도달하지 않는 한 뒤로 돌아가지 않는다.**
- 더 따라갈 간선이 없을 경우 이전으로 돌아간다. 이를 구현하기 위해서 **순환(재귀) 알고리즘 형태**로 되어 있다.
- 그래프가 **인접 리스트**로 표현되어 있다면 시간 복잡도가 **O(n + e)**이고, **인접 행렬**로 표시되어 있다면 **O(n^2)**이다. (n은 정점(v)을 의미, e는 간선을 의미)

### 04. 구현

```jsx
const v = [1, 2, 3, 4, 5];
const e = [
	[1, 2],
	[1, 5],
	[2, 5],
	[2, 3],
	[3, 4],
	[2, 4],
	[4, 5],
];

const list = Array.from({length: v.length + 1}, () => []);

for (let i = 0; i < e.length; i++) {
	const from = e[i][0];
	const to = e[i][1];

	list[from].push(to);
	list[to].push(from);
}
```

```jsx
// 이전 시간에 구현한 인접 리스트를 이용해서 DFS를 구현해보겠습니다.

const visited = Array.from({length: v.length + 1}, () => false);
const dfs = (nowVertex) => {
	visited[nowVertex] = true;

	for (let i = 0; i < list[nowVertex].length; i++) {
		const nextVertex = list[nowVertex][i];
		if (visited[nextVertex]) continue;

		dfs(nextVertex);
	}
};

dfs(1);

// 1, 2, 5, 4, 3
```

# 2. 너비 우선 탐색 (Breadth First Search)

### 01. 정의

- 시작 정점에서 **가까운 정점부터 순서대로 방문**하는 탐색 알고리즘이다.

### 02. 과정

![BFS](/public/img/Graph/BFS.JPG)

### 03. 특징

- 두 정점을 연결하는 경로 중 **최단 경로를 찾을 때** 사용한다.
- **큐를 이용하여 구현**하며, 시작점에서 가까이 있는 정점이 큐에 먼저 추가되고, 멀리 있는 정점이 큐에 나중에 추가된다.
- DFS와 동일하게, 그래프가 **인접 리스트**로 표현되어 있다면 시간 복잡도가 **O(n + e)**이고, **인접 행렬**로 표시되어 있다면 **O(n^2)**이다.

### 04. 구현

```jsx
// 이전 시간에 구현한 인접 리스트를 이용해서 BFS를 구현해보겠습니다.

const visited = Array.from({length: v.length + 1}, () => false);
const queue = [];

visited[1] = true;
queue.push(1);

while (queue.length) {
	const nowVertex = queue.shift();

	for (let i = 0; i < list[nowVertex].length; i++) {
		const nextVertex = list[nowVertex][i];
		if (visited[nextVertex]) continue;

		visited[nextVertex] = true;
		queue.push(nextVertex);
	}
}
```

# 3. 출처

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호)
- 프로그래밍 대회에서 배우는 알고리즘 문제 해결 전략 (구종만)
