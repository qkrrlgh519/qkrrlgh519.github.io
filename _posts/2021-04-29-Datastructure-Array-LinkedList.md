---
layout: post
title: "Datastructure - Array, ArrayList, LinkedList"
date: 2021-04-29 15:30:00 +0900
categories: Datastructure
---

# Array, ArrayList, LinkedList

## 00. 사전 설명

- 여기서 다루는 내용은 Java 언어에서의 Array, ArrayList, LinkedList 입니다.

## 01. Array

- **정의**
  - 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것
- **개념**
  - 배열을 사용하면 많은 양의 데이터를 손쉽게 다룰 수 있습니다.
  - 배열을 사용하면 **"연속적인 공간"**이 할당되고, 인덱스(index) 번호를 사용하여 각 요소(element) 에 쉽게 접근이 가능합니다.
  - 인덱스(index)는 배열의 요소마다 붙여진 일련번호로 각 요소를 구별하는 데 사용됩니다.
- **특징**
  - 배열은 한번 선언되고 나면 길이를 변경할 수 없습니다. (즉, **길이가 고정**)
  - 인덱스(index)로 해당 요소(element) 에 접근할 수 있어서, **조회**시 **시간복잡도**가 **O(1)** 으로 **효율적**입니다.
  - 반면에 **중간에 데이터**를 **삽입**하거나 **삭제**시 배열의 "연속적인 특성"을 유지하기 위해서, 배열의 요소들을 **shift 하는 연산** 때문에 **시간복잡도**가 **O(N)** 으로 **비효율적**입니다.

## 02. ArrayList

- **정의**
  - List 인터페이스를 구현한 클래스로 크기가 가변적인 선형 리스트
- **개념**
  - Object 배열을 이용해서 데이터를 **순차적으로 저장**합니다.
  - 배열에 더 이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장합니다.
- **특징**
  - 선언된 배열의 타입이 모든 객체의 최고조상인 Object이기 때문에 모든 종류의 객체를 담을 수 있습니다.
  - 선언 이후, 길이가 변경될 수 있습니다. (즉, **길이가 가변적**)
    - 단, **용량을 변경할 때**, 새로운 배열을 생성 한 후 기존의 배열에 저장된 내용을 새로운 배열로 복사해야하기 때문에 **비효율적**입니다.
  - 인덱스(index)가 있어서, **조회**시 **시간복잡도가 O(1)** 으로 **효율적**입니다.
  - 반면에 **중간에 데이터**를 **삽입**하거나 **삭제**시, 요소들을 **shift 하는 연산** 때문에 **시간복잡도**가 **O(N)** 으로 **비효율적**입니다.

## 03. LinkedList

- **정의**
  - **불연속적**으로 존재하는 데이터를 **서로 연결(link)**한 형태
- **개념**
  - 배열의 단점을 보완하기 위해 고안된 자료구조입니다.
    1. 크기를 변경할 수 없다.
    2. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.
  - 각 요소(node)들은 자신과 연결된 다음 요소에 대한 참조(주소값)와 데이터로 구성되어 있습니다.
- **특징**
  - 데이터를 **삽입**하거나 **삭제**시, 각 요소들의 참조만 변경해주면 되기 때문에 **시간복잡도**가 **O(1)** 으로 **효율적**입니다.
  - 반면에 데이터를 **조회**할 때, 처음부터 N 번째 데이터까지 차례대로 따라가야하기 때문에 **시간복잡도**가 **O(N)**으로 **비효율적**입니다.
    - 차례대로 따라가야 하는 이유 : 불연속적으로 위치한 각 요소들이 서로 연결된 것이기 때문
  - **트리의 근간**이 되는 자료구조입니다.

## 04. Array vs LinkedList

- **크기**
  - Array 는 선언 후 크기가 고정됩니다.
  - LinkedList 는 선언 후 크기가 변경될 수 있습니다.
- **데이터 조회 (검색)**
  - Array 는 index(인덱스) 를 이용해서 시간복잡도 O(1) 만에 빠르게 조회가 가능합니다.
  - LinkedList 는 처음부터 N 번째 데이터까지 순차적으로 검색하기 때문에 시간복잡도 O(N) 으로 느립니다.
- **데이터 삽입, 삭제**
  - Array 는 중간에 데이터를 삽입, 삭제 시에 연속적인 특성을 유지하기 위해서 shift 연산이 수행되므로, 시간복잡도 O(N) 으로 느립니다.
  - LinkedList 는 중간에 데이터 삽입, 삭제 시에 각 요소의 참조만 변경해주면 되서, 시간복잡도 O(1) 로 빠릅니다.

## 05. Singly Linked List 구현

- **언어** : JavaScript

```jsx
const INDEX_OUT_OF_BOUNDS_EXCEPTION = "IndexOutOfBoundsException";
const NO_SUCH_ELEMENT_EXCPETION = "NoSuchElementException";

class Node {
	constructor(data) {
		this.data = data;
		this.next = null;
	}
}

class LinkedList {
	constructor() {
		this.head = null;
		this.size = 0;
	}

	addFirst(data) {
		const newNode = new Node(data);

		newNode.next = this.head;
		this.head = newNode;
		this.size++;
	}

	add(index, data) {
		if (this.size < index || index < 0) return INDEX_OUT_OF_BOUNDS_EXCEPTION;

		if (index === 0) this.addFirst(data);
		else {
			const newNode = new Node(data);

			let before = this.head;
			for (let i = 0; i < index - 1; i++) {
				before = before.next;
			}

			newNode.next = before.next;
			before.next = newNode;
			this.size++;
		}
	}

	removeFirst() {
		if (this.size === 0) return NO_SUCH_ELEMENT_EXCPETION;

		const removedNode = this.head;
		this.head = removedNode.next;
		this.size--;

		return removedNode.data;
	}

	remove(index) {
		if (this.size < index || index < 0) return INDEX_OUT_OF_BOUNDS_EXCEPTION;

		if (index === 0) return this.removeFirst();
		else {
			let before = this.head;
			for (let i = 0; i < index - 1; i++) {
				before = before.next;
			}

			const removedNode = before.next;
			before.next = removedNode.next;
			this.size--;

			return removedNode.data;
		}
	}

	size() {
		return this.size;
	}

	get(index) {
		if (this.size === 0) return INDEX_OUT_OF_BOUNDS_EXCEPTION;

		let node = this.head;
		for (let i = 0; i < index; i++) {
			node = node.next;
		}

		return node;
	}

	toString() {
		if (this.size === 0) return "[]";
		let node = this.head;

		let str = "[";
		while (node.next !== null) {
			str += node.data + ", ";
			node = node.next;
		}

		str += node.data + "]";
		return str;
	}
}
```

## Reference.

- C언어로 쉽게 풀어쓰는 자료구조 (천인국, 공용해, 하상호 지음)
- Java의 정석 3rd Edition (남궁 성 지음)
- [https://woovictory.github.io/2018/12/27/DataStructure-Diff-of-Array-LinkedList/](https://woovictory.github.io/2018/12/27/DataStructure-Diff-of-Array-LinkedList/)
- [https://coding-factory.tistory.com/551](https://coding-factory.tistory.com/551)
- [https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#array-vs-linked-list](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#array-vs-linked-list)
