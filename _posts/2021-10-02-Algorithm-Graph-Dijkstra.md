---
layout: post
title: "Dijkstra Shortest Path Algorithm"
date: 2021-10-02 12:00:00 +0900
categories: Algorithm(Graph)
---

# Dijkstra Shortest Path Algorithm

### 00. 사전 지식

- **최단 경로 알고리즘** : **가장 짧은 경로**를 찾는 알고리즘
- **탐욕적인 방법 (Greedy Method)**
  - 선택할 때마다 **그 순간 가장 좋다고 생각되는 것을 선택**함으로써 최종적인 해답에 도달하는 방법
- **음수 간선의 중요성**
  - 음수 간선이 있을 경우, 가중치의 합이 **음수인 사이클**이 등장할 수 있다.
  - 음수 사이클이 있는 경우 **최단 경로 문제는 제대로 정의되지 않는다.**

### 01. 정의

- 그래프에서 **하나의 시작 정점**으로부터 **모든 다른 정점까지의 최단 경로**를 찾는 알고리즘이다.
- 다익스트라 최단 경로 알고리즘은 '음의 간선'이 없을 때 정상적으로 동작한다. 음의 간선이란 0보다 작은 값을 가지는 간선을 의미한다.
- 다익스트라 최단 경로 알고리즘은 기본적으로 **그리디 알고리즘으로 분류**된다. 매번 '가장 비용이 적은 노드'를 선택해서 임의의 과정을 반복하기 때문이다.

### 02. 동작 방식

- **집합 S** : 시작 정점 v 로부터의 최단 경로가 이미 발견된 정점들의 집합
- **distance 배열** : 시작 정점 v 에서 집합 S 에 있는 정점만을 거쳐서 다른 정점으로 가는 최단 거리를 기록하는 배열
- **시작 정점**을 **v** 라고 한다면, **distance[v] = 0** 이고, 다른 정점에 대한 distance 값은 시작 정점과 해당 정점간의 가중치 값이 된다.
- 만약, 시작 정점 v 에서 집합 S 에 있는 정점들을 통해서 **갈 수 없는 정점**은 **무한대의 값**을 **저장**한다.
- 알고리즘의 각 단계에서는 **집합 S 안에 있지 않은 정점 중**에서 **가장 distance 값**이 **작은 정점**을 **S 에 추가**한다.
- **새로운 정점 u** 가 **집합 S** 에 **추가**되면, **S 에 있지 않은 다른 정점들의 distance 값**을 **수정**한다. 새로 추가된 정점 u 를 거쳐서 정점까지 가는 거리와 기존의 거리를 비교하여 더 작은 거리로 distance 값을 수정한다.
- 위 과정을 통해 **모든 정점**을 **방문**하면 **distance 배열**에는 **각 정점까지의 최단 거리**가 **기록**되어있다.

### 03. 동작 방식 부연 설명

- 다익스트라 알고리즘은 **우선순위 큐**를 사용하며, **너비 우선 탐색과 유사**한 형태를 가진 알고리즘이다.
- **우선순위 큐**에는 **정점의 번호**와 함께 지금까지 찾아낸 **해당 정점까지의 최단 거리**를 **쌍으로** 넣는다.
- **우선순위 큐**는 **정점까지의 최단 거리를 기준으로 정점을 정렬**함으로써, **아직 방문하지 않은 정점 중 시작점으로부터의 거리가 가장 가까운 점을 찾는 과정**을 **간단**하게 해준다.
- 이때, **유의해야 할 것**은 **각 정점까지의 최단 경로가 갱신될 수 있다는 점**이다. 이 경우 우선순위 큐 내부에 있는 최단 경로를 갱신하기 보다는 우선순위 큐에 **갱신된 최단 경로를 넣어줌으로써 이전의 최단 경로를 무시**하는 방법을 사용한다.
- 우선순위 큐에서 중복 원소를 무시하는 방법은 **꺼낸 원소의 최단 경로가 distance 배열에 기록된 최단 경로보다 작다면 무시**하면 됩니다.

### 04. 동작 과정 설명

1. 출발 노드를 설정한다.
2. 최단 거리 테이블을 초기화한다.
3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.
5. 위 과정에서 3 과 4 를 반복한다.

   ![Dijkstra 알고리즘 동작 과정1](/public/img/Graph/dijkstra1.JPG)
   ![Dijkstra 알고리즘 동작 과정2](/public/img/Graph/dijkstra2.JPG)
   ![Dijkstra 알고리즘 동작 과정3](/public/img/Graph/dijkstra3.JPG)

### 05. 구현

- 언어 : JavaScript

```javascript
// 위의 그림과 같은 그래프가 주어진다고 가정한다.
// 입력 정보 : 정점의 개수, 간선 정보, 시작 정점

// 정점의 개수
const v = 5;

// 간선 정보 [정점, 정점, 가중치]
const edges = [
	[0, 1, 7],
	[0, 4, 3],
	[1, 4, 2],
	[1, 3, 10],
	[4, 3, 11],
	[1, 2, 4],
	[2, 3, 3],
];

// 시작 정점
const start = 0;

const INF = Number.MAX_SAFE_INTEGER;

// Heap 기반 우선순위 큐 생성 {[간선 가중치, 연결된 정점 번호], ...}
class MinHeap {
	constructor() {
		this.bucket = [null];
	}

	// item = [간선 가중치, 연결된 정점 번호]
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

// 다익스트라 알고리즘
const DijkstraAlg = (v, edges, start) => {
	const graph = Array.from({length: v}, () => []);

	// 인접 리스트 형식의 그래프 생성 (연결된 정점 번호, 간선 가중치) 쌍
	for (let i = 0; i < edges.length; i++) {
		const [from, to, weight] = edges[i];

		graph[from].push([to, weight]);
		graph[to].push([from, weight]);
	}

	// 우선순위 큐 생성, distance 배열 생성
	const priorityQueue = new MinHeap();
	const distance = Array(v).fill(INF);

	// 시작 정점의 가중치는 0
	priorityQueue.insert([0, start]);
	distance[start] = 0;

	while (!priorityQueue.isEmpty()) {
		const [weight, to] = priorityQueue.delete();

		// 가중치가 더 크다면 이전의 최단 경로 무시
		if (weight > distance[to]) continue;

		for (let i = 0; i < graph[to].length; i++) {
			const next = graph[to][i][0];
			const nextWeight = graph[to][i][1] + weight;

			// distance 배열에 있는 값보다 작다면 갱신 후 우선순위 큐에 삽입
			if (distance[next] > nextWeight) {
				distance[next] = nextWeight;
				priorityQueue.insert([nextWeight, next]);
			}
		}
	}

	return distance;
};

const distance = DijkstraAlg(v, edges, start);
console.log(distance); // [0, 5, 9, 12, 3]
```

### 06. 시간복잡도

- 수행 시간은 크게 두 부분으로 나누어 볼 수 있다.
  1. **각 정점마다 인접한 간선들을 모두 검사하는 작업**
     1. 각 정점은 정확히 한 번씩 방문되고, 모든 간선은 한 번씩 검사되므로 **`O(|E|)`** 시간이 소요된다.
  2. **우선순위 큐에 원소를 넣고 삭제하는 작업**
     1. 최악의 경우 그래프의 모든 간선이 검사될 때마다 distance 배열이 갱신되고, 우선순위 큐에 정점의 번호가 추가되는 것
     2. 이 때, 추가는 각 간선마다 최대 한 번 일어나기 때문에 우선순위 큐에 추가되는 원소의 수는 최대 `O(|E|)`시간이 소요되고, `O(|E|)` 개의 원소에 대해 이 작업을 수행하려면 전체 시간 복잡도는 **`O(|E|log|E|)`** 가 된다.
- 이 두 작업에 걸리는 시간을 더하면 `O(|E| + |E|log|E|)` = **`O(|E|log|E|)`** 가 된다.
- 대부분의 경우 간선의 개수 `|E|` 는 `|V|^2` 보다 작기 때문에 `O(log|E|) = O(log|V|)` 라고 볼 수 있고, **시간복잡도**는 **`O(|E|log|V|)`** 가 된다.

### 07. 출처

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- 알고리즘 문제 해결 전략 2 (구종만 저자)
- 이것이 코딩테스트다 (나동빈 지음)
