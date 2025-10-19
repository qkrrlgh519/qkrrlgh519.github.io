---
layout: post
title: "DFSAndBFS Algorithm(Overview)"
date: 2025-10-21 12:00:00 +0900
categories: Algorithm(Overview)
---

## DFSAndBFS Algorithm - 깊이 우선 탐색 및 너비 우선 탐색

### 01. DFS - 깊이 우선 탐색

- DFS 정의

  - Depth-First Search
  - 그래프의 모든 정점을 발견하는 가장 단순하고 고전적인 방법
  - 현재 정점과 인접한 간선들을 하나씩 검사하다가, 아직 방문하지 않은 정점으로 향하는 간선이 있다면 그 간선을 무조건 따라간다. 이 과정에서 더 이상 갈 곳이 없는 막힌 정점에 도달하면 포기하고, 마지막에 따라왔던 간선을 따라 뒤로 돌아간다.

- 특징

  - 탐색의 각 과정에서 가능한 한 그래프 안으로 '깊이' 들어가려고 시도하며, 막힌 정점에 도달하지 않는 한 뒤로 돌아가지 않는다.
  - 더 들어갈 간선이 없을 경우 이전으로 돌아간다. 이를 구현하기 위해서 순환(재귀) 알고리즘 형태로 되어 있다.
  - 그래프가 인접 리스트로 표현되어 있다면 시간 복잡도는 O(n + e)이고, 인접 행렬로 포현되어 있다면 O(n^2) 이다. (n은 정점(v)을 의미, e는 간선을 의미)

- 구현

  ```javascript
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

  // 인접 리스트 구현
  const list = Array.from({length: v.length + 1}, () => []);

  for (let i = 0; i < e.length; i++) {
  	const from = e[i][0];
  	const to = e[i][1];

  	list[from].push(to);
  	list[to].push(from);
  }

  // 인접 리스트 기반의 DFS 구현
  const visited = Array.from({length: v.length + 1}, () => false);
  const dfs = (nowVertex) => {
  	// 방문 처리
  	visited[nowVerTex] = true;

  	for (let i = 0; i < list[nowVertex].length; i++) {
  		// 인접 리스트 기반 다음 방문 정점 찾기
  		const nextVertex = list[nowVertex][i];
  		if (visited[nextVerTex]) continue;

  		// 다음 탐색 처리
  		dfs(nowVertex);
  	}
  };

  dfs(1);
  ```

### 02. BFS - 너비 우선 탐색

- BFS 정의

  - Breadth-First Search
  - 시작 정점에서 가장 가까운 정점부터 순서대로 방문하는 탐색 알고리즘이다.

- 특징

  - 두 정점을 연결하는 경로 중 최단 경로를 찾을 때 사용한다.
  - 큐를 이용해 구현하며, 시작점에서 가까이 있는 정점이 큐에 먼저 추가되고, 멀리 있는 정점이 큐에 나중에 추가된다.
  - DFS 와 동일하게, 그래프가 인접 리스트로 표현되어 있다면 시간 복잡도는 O(n + e) 이고, 인접 행렬로 표현되어 있다면 O(n ^ 2) 이다.

- 구현

  ```javascript
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

  // 인접 리스트 구현
  const list = Array.from({length: v.length + 1}, () => []);

  for (let i = 0; i < e.length; i++) {
  	const from = e[i][0];
  	const to = e[i][1];

  	list[from].push(to);
  	list[to].push(from);
  }

  // 인접 리스트 기반의 BFS 구현
  const visted = Array.from({length: v.length + 1}, () => false);
  const queue = [];

  // 시작 정점 설정
  visited[1] = true;
  queue.push(1);

  while (queue.length) {
  	const nowVertex = queue.shift();

  	for (let i = 0; i < list[nowVertex].length; i++) {
  		// 인접 리스트 기반 다음 방문 정점 찾기
  		const nextVertex = list[nowVertex][i];
  		if (visited[nextVertex]) continue;

  		// 방문 처리 및 다음 탐색 처리
  		visited[nextVertex] = true;
  		queue.push(nextVertex);
  	}
  }
  ```

