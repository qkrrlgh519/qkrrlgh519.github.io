---
layout: post
title: "Topological Sort (위상 정렬)"
date: 2021-03-16 23:40:00 +0900
categories: Algorithm(Graph)
---

### 01. 정의

- **순서가 정해져 있는 작업**을 차례로 수행해야 할 때, **그 순서를 결정하는 알고리즘**
- 방향 그래프에 존재하는 각 정점들의 선행 순서를 위배하지 않으며, 모든 정점을 나열하는 것

![TopologicalSort 현실 예시](/public/img/Sort/topologicalsort1.JPG)

- **위상 순서** : 물 끓이기 → 면 넣기 → 스프 넣기 → 계란 넣기 → 라면 먹기
  - 위상 정렬에 따라 정렬된 순서를 위상 순서라고 한다.

### 02. 특징

1. 하나의 방향 그래프에는 **여러개의 위상 순서가 존재**할 수 있다.
2. **위상 정렬**은 **DAG (Directed Acyclic Graph)**에만 적용이 가능하다.

   - DAG : 사이클이 발생하지 않는 방향 그래프
   - 즉, 사이클이 발생하는 경우 위상 정렬을 수행할 수 없다.

   ![Directed Acyclic Graph 예시](/public/img/Sort/topologicalsort2.JPG)

   - 위상 정렬은 시작점을 찾는 방식으로 정렬을 수행하는 데, **사이클이 있는 그래프에서는 시작점을 찾을 수가 없다.** (모든 단계에 전제 조건이 있는 것이다.)

3. **두 가지 해결책**을 얻을 수 있다.
   1. 현재 그래프가 위상 정렬이 가능한 지
   2. 정상적으로 위상 정렬을 수행한 결과

### 03. 원리 (작동 방식)

![TopologicalSort 작동 방식](/public/img/Sort/topologicalsort3.JPG)

1. **진입 차수가 0인 정점**(들어오는 간선의 수가 0인 정점)을 **큐에 삽입**한다.
2. 큐에서 정점을 꺼내서, **꺼낸 정점에 연결된 모든 간선을 제거**한다.
   - 진입 차수가 0인 정점 여러 개 존재할 경우 어느 정점을 선택해도 무방 (따라서, 하나의 그래프에는 복수의 위상 순서가 있을 수 있음)
3. 위의 1, 2번 과정인 진입 차수 0인 정점의 선택과 삭제 과정을 반복해 모든 정점이 선택, 삭제되면 알고리즘 종료

---

- 위의 과정에서 **선택되는 정점의 순서들이 위상 순서**가 된다.
- 위의 과정 중 그래프에 남아 있는 정점 중에 진입 차수가 0인 정점이 없다면, 이러한 그래프는 위상 정렬을 수행할 수 없고, 알고리즘은 중단된다.

### 04. 순서도 (알고리즘)

![TopologicalSort 예제 그래프](/public/img/Sort/topologicalsort4.JPG)

- **inDegree** : 각 정점의 진입 차수 기록
- **queue** : 진입 차수가 0인 후보 정점들 저장

```jsx
// 위의 그래프와 이러한 데이터가 입력으로 들어온다고 가정한다.
const n = 6;
const edges = [
	[0, 2],
	[0, 3],
	[1, 3],
	[1, 4],
	[2, 3],
	[2, 5],
	[3, 5],
	[4, 5],
];

const TopologicalSort = (n, edges) => {
	// 위상 순서를 기록할 배열
	const topologicalOrder = [];

	// 각 정점의 진입 차수 기록
	const inDegree = Array.from({length: n}, () => 0);

	// 인접 리스트로 그래프 생성
	const graph = Array.from({length: n}, () => []);
	for (let i = 0; i < edges.length; i++) {
		const [from, to] = edges[i];

		// 단방향 그래프이므로 한 번만 넣는다.
		graph[from].push(to);
		// 진입 차수 기록
		inDegree[to]++;
	}

	// 후보 정점들 저장
	const queue = [];

	// 진입 차수가 0인 정점을 큐에 삽입
	for (let i = 0; i < n; i++) {
		if (inDegree[i] === 0) queue.push(i);
	}

	// 정점의 수만큼 반복
	for (let i = 0; i < n; i++) {
		// 위상 정렬을 수행할 수 없는 경우
		if (queue.length === 0) return false;

		// 큐에서 정점 추출
		const now = queue.shift();
		topologicalOrder.push(now);

		// 추출한 정점에 연결된 모든 간선 제거
		for (let j = 0; j < graph[now].length; j++) {
			const next = graph[now][j];
			inDegree[next]--;

			// 진입 차수가 0인 정점 큐에 삽입
			if (inDegree[next] === 0) queue.push(next);
		}
	}

	return topologicalOrder;
};

console.log(TopologicalSort(n, edges));
```

### 05. 시간 복잡도

- 정점의 개수와 간선의 개수만큼 소요되므로 **시간 복잡도**는 **O(V + E)**이다.

### 06. 문제

- [백준 2252번 - 줄 세우기](https://www.acmicpc.net/problem/2252)
- [백준 1766번 - 문제집](https://www.acmicpc.net/problem/1766)

### 07. 참고

- C언어로 쉽게 풀어쓰는 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/08/27/algorithm-topological-sort.html](https://gmlwjd9405.github.io/2018/08/27/algorithm-topological-sort.html)
- [https://m.blog.naver.com/ndb796/221236874984](https://m.blog.naver.com/ndb796/221236874984)
