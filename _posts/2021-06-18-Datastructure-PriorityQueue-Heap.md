---
layout: post
title: "Datastructure - Priority Queue, Heap"
date: 2021-06-18 13:30:00 +0900
categories: Datastructure
---

# Priority Queue, Heap

## 1. 우선 순위 큐 (Priority Queue) 의 개념

- **우선 순위 개념**을 **큐에 도입**한 **자료구조**입니다.
- 우선 순위 큐에서는 **데이터들이 우선 순위를 가지고 있고**, **우선 순위가 높은 데이터가 먼저 나가게 됩니다.**
- 우선 순위 큐는 **배열, 연결 리스트 등의 여러가지 방법**으로 구현이 가능한데, **가장 효율적인 구조는 힙 (Heap)** 입니다.
  - **배열** : 정렬 안된 배열 사용시, 삽입 O(1), 삭제 O(n)
  - **연결 리스트** : 정렬 안된 연결 리스트 사용시, 삽입 O(1), 삭제 O(n)
  - **히프** : 삽입 O(logn), 삭제 O(logn)

## 2. 우선 순위 큐 (Priority Queue) 가 쓰이는 곳

- **시뮬레이션 시스템**
- **네트워크 트래픽 제어**
- **운영체제에서의 작업 스케줄링**
- 수치 해석적인 계산

## 3. 힙 (Heap) 의 개념

- 힙 은 **완전 이진 트리의 일종**으로 **우선 순위 큐를 구현하기 위해 만들어진 자료구조**입니다.
- 힙은 여러 개의 값들 중에서 **최소값이나 최대값을 빠르게 찾아내기 위한 자료구조**입니다.
- 힙은 일종의 **느슨한 정렬 상태를 유지**합니다. 즉, **완전히 정렬된 것은 아니지만, 전혀 정렬이 안 된것도 아닌 상태**를 유지합니다.
  - 최대힙의 경우 **큰 값이 상위 레벨**에 있고, **작은 값이 하위 레벨에 있는 정도**입니다.
  - 힙의 목적은 **삭제 연산이 수행될 때**마다 **가장 큰 값을 찾아내기**만 하면 되는 것이므로 전체를 정렬할 필요는 없습니다.
- 이진 탐색 트리와 다르게 힙 트리는 **중복된 값을 허용**합니다.
- 종류에는
  - 부모 노드의 키 값이 자식 노드의 키 값보다 **크거나 같은** **최대 힙** (Max Heap)
  - 부모 노드의 키 값이 자식 노드의 키 값보다 **작거나 같은 최소 힙** (Min Heap) 이 있습니다.

## 4. 힙 (Heap) 구현

- 언어 : JavaScript
- 힙은 **완전 이진 트리**이기 때문에 **각각의 노드에 차례대로 번호를 붙일 수** 있습니다.
- 이 번호를 배열의 인덱스로 생각하면 배열에 힙의 노드들을 저장할 수 있습니다.
- 따라서 **힙을 저장하는 표준적인 자료 구조는 배열**입니다. 그리고 구현을 더욱 쉽게 하기 위해서 배열의 **첫 번째 인덱스인 0 은 사용되지 않**습니다.
- 배열을 이용해 힙을 저장하면 완전 이진 트리 처럼 자식 노드와 부모 노드를 쉽게 알 수 있습니다.
  - **왼쪽 자식의 인덱스 = (부모의 인덱스) \* 2**
  - **오른쪽 자식의 인덱스 = (부모의 인덱스) \* 2 + 1**
  - **부모의 인덱스 = (INT) (자식의 인덱스) / 2**
  - 특정 위치의 노드 번호는 새로운 노드가 추가되어도 변하지 않습니다.
- **삽입 연산**
  - 새로운 요소를 힙의 마지막 노드에 이어서 삽입합니다.
  - **부모 노드와 비교하면서 부모 노드보다 값이 크면 (or 작으면) 교환**합니다.
  - 부모 노드보다 작은 (or 큰) 상황이 올 때까지 교환합니다.
    - 실제 구현에서는 바로 교환하는 것이 아닙니다.
    - **부모 노드만 끌어 내린 다음, 삽입될 위치가 획실해 진 다음** **최종적으로 새로운 노드는 그 위치로 이동**합니다.
    - 이렇게 하는 것이 이동 횟수를 줄일 수 있습니다. (SWAP 3배 추정)
- **삭제 연산**

  - 힙의 최상위 노드인 루트 노드를 삭제합니다.
  - 삭제된 루트 노드에는 힙의 마지막 노드를 가져옵니다.
  - **새로운 루트 노드와 자식 노드들을 비교하면서 자식 노드가 값이 크면 (or 작으면) 교환**합니다.
  - 자식 노드보다 큰 상황이 올 때까지 교환합니다.

  ```javascript
  // 삽입 연산
  const insertMaxHeap = (array, item) => {
  	array.push(item); // 1. 마지막 노드로 삽입
  	let index = array.length - 1;

  	while (index != 1 && item > array[parseInt(index / 2)]) {
  		// 2. 부모 노드와 비교
  		array[index] = array[parseInt(index / 2)]; // 3. 부모 노드의 값을 끌어내린다.
  		index = parseInt(index / 2);
  	}

  	array[index] = item; // 부모를 끌어내리다가 적합한 위치를 찾으면 넣는다.
  };

  // 삭제 연산
  const deleteMaxHeap = (heapArray) => {
  	if (heapArray.length === 1) return;

  	let parentIdx = 1; // index
  	let childIdx = 2; // 우선, 왼쪽 자식 index를 keep!!
  	let item = heapArray[1]; // return 하기 위해서 기록
  	let last = heapArray.pop(); // 마지막 노드를 가져옴, 그리고 배열의 크기 1 줄임

  	while (childIdx < heapArray.length) {
  		// 오른쪽 자식이 있다면, 왼쪽 자식과 오른쪽 자식 비교
  		// childIdx 는 초기에는 왼쪽 자식을 가리킴
  		if (
  			childIdx < heapArray.length - 1 &&
  			heapArray[childIdx] < heapArray[childIdx + 1]
  		) {
  			childIdx++;
  		}

  		// 만약 마지막 노드가 자식보다 더 크면 반복 종료
  		if (last >= heapArray[childIdx]) {
  			break;
  		}

  		// 마지막 노드가 자식보다 작으면 부모를 자식 노드로 교환
  		heapArray[parentIdx] = heapArray[childIdx];

  		// 한 단계 아래로 이동
  		parentIdx = childIdx;
  		childIdx *= 2; // 한 단계 아래의 왼쪽 자식 노드로
  	}

  	// 마지막 item 을 pop 해도 heap 에 item 이 남아있다면,
  	// 마지막에 노드를 올바른 위치를 넣음으로 재구성 완료
  	if (heapArray.length !== 1) heapArray[parentIdx] = last;

  	// 아까 저장해놨던 루트 노드 값 반환
  	return item;
  };
  ```

  ```javascript
  // 최소힙 - 클래스 형태
  class MinHeap {
  	constructor() {
  		this.bucket = [null];
  	}

  	// index 를 현재 크기 + 1 로 초기화
  	// 부모 노드보다 작다면, 부모 자식 노드 교환
  	// 부모 노드보다 큰 위치를 찾으면 item 넣기
  	insert(item) {
  		let index = this.bucket.length;

  		while (index !== 1 && item < this.bucket[parseInt(index / 2)]) {
  			this.bucket[index] = this.bucket[parseInt(index / 2)];
  			index = parseInt(index / 2);
  		}

  		this.bucket[index] = item;
  	}

  	// 최상위 부모 index 와 왼쪽 자식 index 초기화
  	// return 할 item 과 가장 마지막 item 저장
  	// 자식 index 가 현재 배열을 벗어나지 않을 동안
  	// 왼쪽 자식과 오른쪽 자식 중 더 작은 자식 찾고
  	// 마지막 item 이 더 작다면 반복 종료
  	// 마지막 item 이 더 크다면 부모 노드에 자식 노드 넣고
  	// 부모 index 는 자식 index 가 되고, 자식 index 는 그의 왼쪽 자식 index 로 설정
  	// 반복이 종료되었다면, 부모 index 에 마지막 item 넣기
  	delete() {
  		if (this.isEmpty()) return;

  		let parentIdx = 1;
  		let childIdx = 2;
  		let item = this.bucket[1];
  		let lastItem = this.bucket.pop();

  		while (childIdx < this.bucket.length) {
  			if (
  				childIdx + 1 < this.bucket.length &&
  				this.bucket[childIdx] > this.bucket[childIdx + 1]
  			)
  				childIdx += 1;

  			if (lastItem <= this.bucket[childIdx]) break;

  			this.bucket[parentIdx] = this.bucket[childIdx];

  			parentIdx = childIdx;
  			childIdx *= 2;
  		}

  		// 마지막 item 을 pop 해서 heap 이 텅 빈 상태에서 이 작업을 수행하면,
  		// heap 에서 마지막 item 이 계속해서 남아있는 문제가 발생합니다.
  		if (!this.isEmpty()) this.bucket[parentIdx] = lastItem;

  		return item;
  	}

  	isEmpty() {
  		return this.bucket.length === 1;
  	}
  }
  ```

## 5. 힙 (Heap) 의 시간복잡도

- **삽입 연산**
  - 새로운 요소가 힙 트리를 타고 올라가면서 부모 노드들과 교환을 합니다.
  - 최악의 경우 루트 노드까지 올라가야 하므로 거의 트리의 높이에 해당하는 비교 연산 및 이동 연산이 필요합니다.
  - 힙은 완전 이진 트리로 높이는 logn 입니다.
  - 따라사 **시간복잡도는 O(logn)** 입니다.
- **삭제 연산**
  - 마지막 노드를 루트로 가져온 후에 자식 노드들과 비교하여 교환되는 부분이 가장 시간이 많이 걸립니다.
  - 삭제 연산도 마찬가지로 최악의 경우 가장 아래 레벨까지 내려가야 하므로 트리의 높이만큼의 시간이 걸립니다.
  - 따라서 **시간복잡도는 O(logn)** 입니다.

## Reference.

- C언어로 쉽게 풀어쓰는 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)
