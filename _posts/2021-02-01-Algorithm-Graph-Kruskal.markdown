---
layout: post
title: "Kruskal's MST Algorithm"
date: 2021-02-01 18:30:00 +0900
categories: Algorithm(Graph)
---

### 00. 사전 지식

- **MST** : 최소 비용 신장 트리로, **가중치(비용)의 합이 가장 작은 스패닝 트리**이다. 특징으로는 사이클이 없으며, 모든 정점이 서로 연결되어 있다.
- **Union-Find** : **상호 배타적인 부분집합들**로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조 (또는 알고리즘)
- **탐욕적인 방법 (Greedy Method)**
  - 선택할 때마다 **그 순간 가장 좋다고 생각되는 것을 선택**함으로써 최종적인 해답에 도달하는 방법
  - 최적이라고 생각했던 지역적인 해답들을 모아서 최종적인 해답을 만들었다고 해서, 그 해답이 반드시 전역적으로 최적이라는 보장이 없어서 검증이 필요하다.

### 01. 정의

- 탐욕적인 방법을 이용하는 알고리즘으로, **그래프에서 가장 비용이 작은 간선을 우선적으로 연결해서 Minimum Spanning Tree를 구하는 알고리즘**이다.
- Kruskal 알고리즘은 전역적으로 최적의 해답을 주는것으로 증명되어 있다.
- **"가중치가 가장 작은 간선과 가중치가 가장 큰 간선 중 어느쪽이 MST에 포함될 가능성이 높을까?"** 라는 의문에 착안해서 만들어진 알고리즘이다.

### 02. 동작 과정

1. 그래프의 **간선들**을 가중치(비용)를 기준으로 **오름차순으로 정렬**한다.
2. 정렬된 간선들의 리스트에서 **사이클을 형성하지 않는 간선을 찾는다.** (정렬되어 있으니 가중치가 가장 작은 간선이 찾아질 것이다.)
3. 현재의 **최소 비용 신장 트리의 집합에 추가**한다.
4. 최소 비용 신장 트리가 완성될 때까지 2와 3을 반복한다.

![kruskal 알고리즘 전체 동작 과정](/public/img/Graph/kruskal1.JPG)

### 03. 핵심 : 간선을 추가 시 사이클을 이루는지 여부를 판단하는 부분

- 어떤 간선을 추가해서 그래프에 **사이클이 생기려면 간선의 양 끝 점은 같은 컴포넌트에 속해**있어야 한다.
- 따라서 두 정점이 주어졌을 때 이들이 같은 컴포넌트에 속하는지 확인하고, 그렇지 않다면 두 컴포넌트를 합치는 연산을 빠르게 지원할 수 있는 자료구조가 있다면 효율적일 것이다.
- 그런 자료구조가 바로 **Union-Find**이다.
- **사이클이 생기는지 여부**는 두 정점이 이미 같은 집합에 속해 있는지 확인(Find)하면 되고, **간선을 트리에 추가**할 경우에는 이들이 포함된 두 집합을 합(Union)치면 된다.

### 04. 구현

![kruskal 알고리즘을 적용할 예제 그래프](/public/img/Graph/kruskal2.JPG)

```jsx
// 위의 그림처럼 그래프가 주어졌을 때

class DisjointSet {
	constructor(n) {
		this.parent = Array.from({length: n}, (_, i) => i); //init (편의상 i)
		this.rank = Array.from({length: n}, () => 1);
	}

	// u가 속한 트리의 루트의 번호를 반환
	find(u) {
		if (u === this.parent[u]) return u;
		return (this.parent[u] = this.find(this.parent[u]));
	}

	// u가 속한 트리와 v가 속한 트리를 합친다.
	union(u, v) {
		u = this.find(u);
		v = this.find(v);

		// u와 v가 이미 같은 트리에 속하는 경우를 걸러낸다.
		if (u === v) return;
		if (this.rank[u] > this.rank[v]) {
			const temp = u;
			u = v;
			v = temp;
		}
		// 이제 rank[v]가 항상 rank[u] 이상이므로 u를 v의 자식으로 넣는다.
		this.parent[u] = v;
		if (this.rank[u] === this.rank[v]) ++this.rank[v];
	}
}

// 간선 정보를 저장할 객체를 만들 클래스
class Edge {
	constructor(start, end, weight) {
		this.start = start;
		this.end = end;
		this.weight = weight;
	}
}

const Kruskal = () => {
	// 결과 배열
	const result = [];

	// 그림에 맞게 그래프 생성 (연결리스트나 인접행렬이 아닌 그저 간선 정보를 가지고 있는 배열)
	const graph = [];
	graph.push(new Edge(0, 1, 14));
	graph.push(new Edge(0, 2, 12));
	graph.push(new Edge(0, 3, 45));
	graph.push(new Edge(1, 2, 28));
	graph.push(new Edge(1, 3, 9));
	graph.push(new Edge(1, 4, 63));
	graph.push(new Edge(2, 3, 32));
	graph.push(new Edge(3, 4, 51));

	// union-find 자료구조 생성 (총 정점의 개수는 5개)
	const set = new DisjointSet(5);

	// 가중치 기준으로 오름차순 정렬
	graph.sort((a, b) => a.weight - b.weight);

	// 예를 들어, MST의 가중치의 합
	let sum = 0;
	for (let i = 0; i < graph.length; i++) {
		const {start, end, weight} = graph[i];

		// 만약 둘이 같은 컴포넌트에 있으면 (추가시 사이클 생김)
		if (set.find(start) === set.find(end)) continue;

		set.union(start, end);
		result.push([start, end, weight]);
		sum += weight;
	}

	console.log(sum); // 86
	console.log(result); // [[ 1, 3, 9 ], [ 0, 2, 12 ], [ 0, 1, 14 ], [ 3, 4, 51 ]]
};

Kruskal();
```

### 04. 시간복잡도

- 이전에 알아봤듯이 DisjointSet에 대해 하는 연산은 현실적으로 **상수 시간**이다.
- 그리고 실제 트리를 만드는 과정은 **O(E)**이다. (E는 간선의 개수)
- 따라서 **전체 시간 복잡도는 간선들을 정렬하는데 걸리는 시간인 O(ElogE)가 지배**하고, 시간 복잡도는 **O(ElogE)**이다.
- Prim 알고리즘의 시간복잡도는 **O(n^2)**이다.
  - 그래프 내에 적은 숫자의 간선만을 가지는 **'희소 그래프`**의 경우는 **Kruskal 알고리즘이 적합**하고,
  - 그래프 내에 간선이 많이 존재하는 **'밀집 그래프'**의 경우는 **Prim 알고리즘이 적합**하다.

### 05. 참고

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- 알고리즘 문제 해결 전략 2 (구종만 지음)
- [https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html](https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html)
- [https://m.blog.naver.com/ndb796/221230994142](https://m.blog.naver.com/ndb796/221230994142)
