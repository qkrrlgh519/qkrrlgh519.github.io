---
layout: post
title: "Prim's MST Algorithm"
date: 2021-02-05 21:45:00 +0900
categories: Algorithm(Graph)
---

### 00. 사전 지식

- **MST**
- **탐욕적인 방법 (Greedy Method)**

### 01. 정의

- 하나의 시작 정점으로 구성된 트리에서 **인접한 정점 중 가장 가까운 정점을 우선적으로 연결**해서 Minimum Spannig Tree를 구하는 알고리즘이다.
- 탐욕적인 방법을 이용하는 알고리즘이다.
- Prim 알고리즘도 최적의 해답을 주는 것으로 증명되어 있다.

### 02. Kruskal vs Prim

- **Kruskal's Alg**은 여기저기서 **산발적으로 만들어진 트리의 조각들을 합쳐서** 스패닝 트리를 만드는 반면, **Prim's Alg**은 하나의 시작점으로 구성된 트리에 간선을 하나씩 추가하며 스패닝 트리가 될 때까지 **키워간다**.
- **Kruskal's Alg**은 **간선을 기반**으로 하는 알고리즘인 반면, **Prim's Alg**은 **정점을 기반**으로 하는 알고리즘이다.
- 둘 다 선택할 수 있는 간선들 중 가중치가 가장 작은 간선을 선택하는 과정을 반복한다는 점은 동일하다.

### 02. 동작 과정

1. 시작 단계에서는 시작 정점만이 MST 집합에 포함된다.
2. 앞 단계에서 만들어진 MST 집합에 sa해서 트리를 확장한다.
3. 위의 과정을 MST 집합의 간선의 수가 n - 1이 될 때까지 반복한다.

![prim 알고리즘 전체 동작 과정](/public/img/Graph/prim1.JPG)

### 03. 동작 과정 중 세부 과정

- **distance** : 현재까지 알려진 신장 트리의 정점 집합에서 각 정점까지의 거리를 가지고 있다. (**어떤 정점까지의 여러 경로 중 거리가 최소인 경로**를 담고 있다.)
- **selected** : 현재 신장트리에 추가된 정점 정보를 가지고 있다.

  ![prim 알고리즘 세부 동작 과정1](/public/img/Graph/prim2.JPG)
  ![prim 알고리즘 세부 동작 과정2](/public/img/Graph/prim3.JPG)
  ![prim 알고리즘 세부 동작 과정3](/public/img/Graph/prim4.JPG)

1. 시작 단계에서는 시작 정점(0번 정점)만 신장 트리에 추가된다. 이는 distance[0]을 0으로 표현하고, selected[0]을 true로 변경하는 것으로 표현한다.
2. **새롭게 추가된 정점(0번 정점)에 인접한 정점들의 distance값을 갱신**해준다. 1, 2번 정점이 연결되어 있으니 distance[1] = 14, distance[2] = 12로 갱신해준다. (인접하지 않은 정점은 Infinity)
3. distance는 현재 신장 트리에서 인접한 정점들의 거리를 나타내고, selected는 현재 신장 트리에 포함된 정점 정보를 나타내니까, 둘의 정보를 이용해 **현재 신장 트리에 포함되지 않으며, 가장 가까운 정점을 찾아서 신장트리에 추가**한다.
4. 위의 과정을 반복하면 최소 신장 트리를 얻을 수 있다.

### 04. 구현

```jsx
// 위의 그림과 같은 그래프가 주어진다고 가정한다.

// 입력 정보 (그래프의 정보)
// 정점의 개수
const n = 5;
// 간선 정보 [정점, 정점, 가중치]
const edges = [
	[0, 1, 14],
	[0, 2, 12],
	[0, 3, 45],
	[1, 2, 28],
	[1, 3, 9],
	[1, 4, 63],
	[2, 3, 32],
	[3, 4, 51],
];

// 현재 신장 트리에서 인접하지 않은 정점을 표현하기 위한 무한대 값
const INF = Number.MAX_SAFE_INTEGER;

// 간선 정보를 담기 위한 클래스 {정점, 가중치}
class Edge {
	constructor(v, weight) {
		this.v = v; // 맞은편 정점
		this.weight = weight; // 가중치
	}
}

const Prim = (edges, n) => {
	// 결과 MST의 가중치의 합을 알아보자!!
	let sum = 0;

	// 주어진 그래프를 인접 리스트로 만들기 (가중치 정보도 포함해서)
	const graph = Array.from({length: n}, () => []);
	for (let i = 0; i < edges.length; i++) {
		const [from, to, weight] = edges[i];

		graph[from].push(new Edge(to, weight));
		graph[to].push(new Edge(from, weight));
	}

	// 현재까지 알려진 신장 트리의 정점 집합에서 각 정점까지의 거리
	const distance = Array.from({length: n}, () => INF);

	// 현재 신장트리에 추가된 정점 정보
	const selected = Array.from({length: n}, () => false);

	// 1.시작 정점을 넣는다.
	distance[0] = 0;

	// 2. 현재 신장 트리에서 가장 가까운 정점을 추가하고, distance 갱신을 반복한다.
	for (let i = 0; i < n; i++) {
		let u = -1;

		// 현재 신장 트리에 포함되지 않은 정점들 중에서 인접한 정점 중 가장 가까운 정점 찾기
		for (let i = 0; i < n; i++) {
			if (!selected[i] && (u === -1 || distance[u] > distance[i])) {
				u = i;
			}
		}

		// 현재 신장 트리에 포함시킨다.
		selected[u] = true;
		sum += distance[u];

		// 추가한 정점(u)에 인접한 정점들(v)을 다 확인하며 distance를 갱신(더 짧은 거리로)
		for (let i = 0; i < graph[u].length; i++) {
			const {v, weight} = graph[u][i];
			if (!selected[v] && distance[v] > weight) {
				distance[v] = weight;
			}
		}
	}

	return [distance, sum];
};

const [distance, sum] = Prim(edges, n);
console.log(distance, sum); // [0, 14, 12, 9, 51] 86
```

### 05. 시간복잡도

- 주 반복문이 정점의 수 v만큼 반복하고, 내부 반복문이 v번 반복하므로 **Prim's Alg**의 시간 복잡도는 **O(v^2)**이다. (정점의 영향을 받는다)
- Kruskal's Alg의 시간 복잡도가 O(eloge)이므로, 그래프 내에 적은 숫자의 간선만을 가지는 '희소 그래프'의 경우 Kruskal's Alg이 적합하고,
- 그래프 내에 간선이 많이 존재하는 '밀집 그래프'의 경우는 Prim's Alg이 적합하다.

### 06. 출처

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- 알고리즘 문제 해결 전략 2 (구종만 저자)
- [https://gmlwjd9405.github.io/2018/08/30/algorithm-prim-mst.html](https://gmlwjd9405.github.io/2018/08/30/algorithm-prim-mst.html)
- [https://mattlee.tistory.com/46](https://mattlee.tistory.com/46)
- [https://programmers.co.kr/learn/courses/30/lessons/62050](https://programmers.co.kr/learn/courses/30/lessons/62050)
