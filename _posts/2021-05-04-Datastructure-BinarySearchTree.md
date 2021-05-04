---
layout: post
title: "Datastructure - BinarySearchTree"
date: 2021-05-04 16:30:00 +0900
categories: Datastructure
---

# BinarySearchtree (이진 탐색 트리)

## 01. 개념

- 이진 트리 기반의 탐색을 위한 자료구조입니다.
- **이진 탐색 트리 정의**
  - 모든 원소의 키는 유일한 키를 가진다.
  - 왼쪽 서브 트리의 키들은 루트의 키보다 작다
  - 오른쪽 서브 트리의 키들은 루트의 키보다 크다.
  - 왼쪽과 오른쪽 서브 트리도 이진 탐색 트리이다.
- 정의에 따라서, **찾고자 하는 키 값**이 이진 트리의 루트 노드의 키 값과 비교하여 **루트 노드보다 작으면** 원하는 키 값은 **왼쪽 서브 트리**에 있고, **루트 노드보다 크면** 원하는 키 값은 **오른쪽 서브 트리**에 있습니다.
- 이진 탐색 트리를 **중위 순회**하면, 트리의 노드들을 **오름차순으로 순회**를 할 수 있습니다.

## 02. 분석

- **탐색**, **삽입**, **삭제** 연산의 **시간복잡도**는 **트리의 높이**를 `h` 라고 했을 때, `O(h)` 가 됩니다.
- 따라서 `n` 개의 노드를 가지는 이진 탐색 트리의 경우, 일반적인 이진 트리의 높이는 `logn` 이므로, 이진 탐색 트리 연산의 **평균적인 경우**의 **시간복잡도**는 `O(logn)` 입니다.
- 그러나 위의 내용은 좌우의 서브 트리가 균형을 이룰 경우입니다.
- **최악의 경우**에는 **한쪽으로 치우치는 경사 트리**가 되어서 **트리의 높이**가 `n` 이됩니다. 이 경우에는 **탐색**, **삭제**, **삽입** 연산의 **시간복잡도**는 선형 탐색과 동일한 `O(n)` 이 됩니다.

## 03. 탐색 연산

- **언어** : JavaScript
- 이진 탐색 트리에서 특정한 키 값을 가진 노드를 찾기 위해서는 **먼저** **주어진 탐색키 값**과 **루트 노드의 키값**을 **비교**합니다. 그 결과에 따라서 **3 가지의 경우**로 나뉩니다.
  - 비교한 결과가 같으면 탐색이 성공적으로 종료
  - 비교한 결과가, 주어진 키 값이 루트 노드의 키 값보다 작으면 탐색은 이 루트 노드의 왼쪽 자식을 기준으로 다시 시작
  - 비교한 결과가, 주어진 키 값이 루트 노드의 키 값보다 크면 탐색은 이 루트 노드의 오른쪽 자식을 기준으로 다시 시작

```jsx
// 재귀 방식
const searchNode = (root, key) => {
	if (root === null) return null;

	if (root.key === key) return root;
	else if (root.key < key) return searchNode(root.right, key);
	else return searchNode(root.left, key);
};

// 반복 방식
const searchNode = (root, key) => {
	while (root !== null) {
		if (root.key === key) return root;
		else if (root.key < key) root = root.right;
		else root = root.left;
	}

	return null;
};
```

## 04. 삽입 연산

- **언어** : JavaScript
- 이진 탐색 트리에 원소를 삽입하기 위해서는 **먼저 탐색을 수행**하는 것이 필요합니다.
- **이유**는 이진 탐색 트리에서는 **같은 키 값을 갖는 노드가 없어야 하기 때문**입니다. 그리고 **탐색에 실패한 위치가 바로 새로운 노드를 삽입하는 위치**가 되기 때문입니다.
- 새로운 노드는 **항상 단말 노드에 추가**됩니다.

```jsx
// 재귀 방식
const insertNode = (root, key) => {
	if (root === null) return new Node(key);

	if (root.key < key) root.right = insertNode(root.right, key);
	else if (root.key > key) root.left = insertNode(root.left, key);

	return root; // root.key === key 인 경우, 중복된 키 값은 삽입하지 않습니다.
};

// 결론적으로, 중복된 키 값을 제외하고, null 을 만날 때까지 왼쪽 또는 오른쪽으로 내려갑니다
// null 을 만나면 node 를 생성하고 return 해서 부모 노드의 왼쪽 또는 오른쪽에 링크됩니다.
```

## 05. 삭제 연산

- **언어** : JavaScript
- 삽입 연산과 마찬가지로 **삭제 연산을 하기 위해서**는, **탐색**을 통해서 **삭제하려는 키 값**이 트리 안에서 **어디에 위치하는지** 알아내야 합니다.
- 위치를 찾은 후에는 **3 가지 경우**를 고려해야 합니다.
  - 삭제하려는 노드가 단말 노드인 경우
    - 이 경우에는 **단말 노드만 지우면** 됩니다.
    - 즉, 단말 노드의 부모 노드를 찾아서 부모 노드의 링크 필드를 null 로 만들면 됩니다.
  - 삭제하려는 노드가 하나의 왼쪽이나 오른쪽 서브 트리 중 하나만 가지고 있는 경우
    - 이 경우에는 **삭제하려는 노드는 지우고**, **서브 트리를 삭제하려는 노드의 부모 노드에 붙이면** 됩니다.
  - 삭제하려는 노드가 두 개의 서브 트리 모두 가지고 있는 경우
    - 가장 복잡합니다. 문제는 **서브 트리에 있는 어떤 노드를 삭제 노드의 위치로 가져올 것이냐** 입니다.
    - **삭제 노드와 가장 값이 비슷한 노드**를 가져와야 할 것입니다. 그래야만 다른 노드를 이동시키지 않아도 이진 탐색 트리가 그대로 유지됩니다.
    - **가장 가까운 값**은 **왼쪽 서브 트리에서 가장 큰 값** 혹은 **오른쪽 서브 트리에서 가장 작은 값**입니다.
    - 왼쪽 서브 트리에서 가장 큰 값은 **왼쪽 서브 트리의 가장 오른쪽에 있는 노드**입니다.
    - 오른쪽 서브 트리에서 가장 작은 값은 **오른쪽 서브 트리의 가장 왼쪽에 있는 노드**입니다.
    - 둘 중에 어느 것을 선택해도 상관이 없습니다.
    - **오른쪽 서브 트리에서 가장 작은 값**을 후계자로 선택한다면, 이 값은 **오른쪽 서브 트리**에서 **왼쪽 자식 링크를 타고 null 을 만날 때까지 내려**가면 됩니다.

```jsx
// 재귀 방식
const deleteNode = (root, key) => {
	if (root === null) return root;

	if (root.key < key) {
		root.right = deleteNode(root.right, key);
	} else if (root.key > key) {
		root.left = deleteNode(root.left, key);
	} else {
		if (root.left === null) {
			return root.right;
		} else if (root.right === null) {
			return root.left;
		} else {
			// 오른쪽 서브트리에서 가장 왼쪽에 있는 노드를 찾습니다. (후계자 노드 찾기)
			let closestInRight = root.right;
			while (closestInRight.left !== null) closestInRight = closestInRight.left;

			// 삭제할 노드에 후계자 노드의 값을 복사합니다.
			root.key = closestInRight.key;
			// 후계자 노드를 삭제합니다. (말단 노드이기 때문에 쉽게 삭제됨)
			root.right = deleteNode(root.right, closestInRight.key);
		}
	}

	return root;
};

// 삭제하려는 노드가 단말 노드인 경우와 왼쪽 혹은 오른쪽 서브트리 하나만 가진 경우,
// 둘의 경우는 다르지만 처리 방식은 같습니다.
// root.left === null or root.right === null 조건에서 두 경우 다 처리됩니다.
```

## 06. 전체 소스 코드

- **언어** : JavaScript

```jsx
class Node {
	constructor(key) {
		this.key = key;
		this.left = null;
		this.right = null;
	}
}

const searchNode = (root, key) => {
	if (root === null) return null;

	if (root.key === key) return root;
	else if (root.key < key) return searchNode(root.right, key);
	else return searchNode(root.left, key);
};

const insertNode = (root, key) => {
	if (root === null) return new Node(key);

	if (root.key < key) root.right = insertNode(root.right, key);
	else if (root.key > key) root.left = insertNode(root.left, key);

	return root;
};

const deleteNode = (root, key) => {
	if (root === null) return root;

	if (root.key < key) {
		root.right = deleteNode(root.right, key);
	} else if (root.key > key) {
		root.left = deleteNode(root.left, key);
	} else {
		if (root.left === null) {
			return root.right;
		} else if (root.right === null) {
			return root.left;
		} else {
			let closestInRight = root.right;
			while (closestInRight.left !== null) closestInRight = closestInRight.left;

			root.key = closestInRight.key;
			root.right = deleteNode(root.right, closestInRight.key);
		}
	}

	return root;
};

const inOrder = (root) => {
	if (root === null) return "";

	let res = "";
	res += inOrder(root.left);
	res += root.key + " ";
	res += inOrder(root.right);

	return res;
};

let root = new Node(3);

insertNode(root, 2);
insertNode(root, 1);
insertNode(root, 4);
insertNode(root, 5);
insertNode(root, 6);

console.log(searchNode(root, 3));
console.log(deleteNode(root, 3));
console.log(inOrder(root));
```

## Reference.

- C언어로 쉽게 풀어쓰는 자료구조 (천인국, 공용해, 하상호 지음)
