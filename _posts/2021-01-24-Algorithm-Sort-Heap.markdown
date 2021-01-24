---
layout: post
title: "Heap Sort (힙 정렬)"
date: 2021-01-24 20:30:00 +0900
categories: Algorithm(Sort)
---

### 00. Heap (힙)

- **완전 이진 트리의 일종**이다.
- **우선순위 큐를 완전 이진트리로 구현하는 방법**
- 힙은 **최댓값이나 최솟값을 쉽게 추출**할 수 있는 자료구조이다.
- 힙은 **일종의 느슨한 정렬 상태를 유지**한다. 즉, 완전히 정렬된 것은 아니지만, 전혀 정렬이 안 된것도 아닌 상태를 유지한다. (최대 힙의 경우 큰 값이 상위 레벨에 있고, 작은 값이 하위 레벨에 있는 정도)
- **종류**
  - **최대 힙 (Max Heap)** : 부모 노드의 키값이 자식 노드의 키값보다 크거나 같은 완전 이진 트리
  - **최소 힙 (Min Heap)** : 부모 노드의 키값이 자식 노드의 키값보다 작거나 같은 완전 이진 트리
- **구현**

  ![heap을 배열로 구현](/public/img/Sort/heapsort1.JPG)

  - 히프는 **완전 이진 트리**이기 때문에 **각각의 노드에 차례대로 번호를 붙일 수 있다.**
  - 이 번호를 배열의 인덱스로 생각하면 **배열에 히프의 노드들을 저장**할 수 있다.
  - 따라서 히프를 저장하는 표준적인 자료 구조는 배열이다.
  - 프로그램의 **구현을 쉽게 하기 위해 배열의 첫 번째 인덱스인 0은 사용되지 않는다.**
  - 배열을 이용해 힙을 저장하면 완전 이진 트리에서 처럼 자식 노드와 부모 노드를 쉽게 알 수 있다.
    - **왼쪽 자식**의 인덱스 = **(부모의 인덱스) \* 2**
    - **오른쪽 자식**의 인덱스 = **(부모의 인덱스) \* 2 + 1**
    - **부모**의 인덱스 = **(INT) (자식의 인덱스) / 2**

### 01. 힙 정렬 요약

- **불안정 정렬**이다.
- 최소 힙의 루트 노드는 가장 작은 값을 가지게 된다.
- 최소 힙의 이러한 특성을 이용하여 **정렬할 배열을 먼저 최소 힙으로 변환**한 다음, **가장 작은 원소부터 차례대로 추출하여 정렬하는 방법**을 힙 정렬이라 한다. (오름 차순)
- 내림 차순으로 정렬하기 위해서는 최대힙으로 변환하면 된다. 또는 최소 힙으로 구현하고, 원소를 추출할 때 배열의 뒤쪽부터 저장하면 최소힙으로도 내림차순 정렬을 할 수 있다.

### 02. 원리 (작동 방식)

- 힙 정렬은 **힙으로 변환하고, 하나씩 추출하여 정렬하는 방식**이기 때문에 힙을 구현하는 방법을 알아보자!!
- 여기서는 **최대 힙을 구현**해본다.

### 03. 최대 힙의 삽입 연산 구현

![최대 힙의 삽입 연산](/public/img/Sort/heapsort2.JPG)

1. 힙에 새로운 요소가 들어오면, 일단 새로운 노드를 **힙의 마지막 노드로 삽입**한다.
2. **부모 노드와 비교**하면서 **부모 노드보다 값이 크면 교환**한다. (부모 노드 index = 본인 index / 2)
3. 부모 노드보다 작은 상황이 올 때까지 교환한다.

```jsx
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
```

- 위의 코드를 보면, 실제 구현에서는 바로 교환하는 것이 아니고, **부모 노드만을 끌어내린 다음, 삽입될 위치가 확실해 진 다음** 최종적으로 새로운 노드는 **그 위치로 이동**한다. 이렇게 하는 것이 이동 횟수를 줄일 수 있다. (swap 하면서 올라가면 3배로 추정)

### 04. 최대 힙의 삭제 연산 구현

![최대 힙의 삭제 연산](/public/img/Sort/heapsort3.JPG)

1. 최대 힙에서 삭제 연산은 최대값을 가진 요소를 삭제하는 것이다. **최대 힙에서 최대값은 루트 노드**이므로 루트 노드가 삭제된다.
2. **삭제된 루트 노드에는 힙의 마지막 노드**를 가져온다.
3. 새로운 루트 노드와 자식 노드들을 비교하면서 **자식 노드가 값이 크면 교환**한다. (자식 노드 index = 본인 index _ 2 or 본인 index _ 2 + 1) 두 자식 중 큰 값과 교환이 일어난다.
4. **자식 노드보다 큰 상황이 올 때까지 교환**한다.

```jsx
// 삭제 연산
const deleteMaxHeap = (heapArray) => {
	let parent = 1; // index
	let child = 2; // 우선, 왼쪽 자식 index를 keep!!
	let item = heapArray[1];
	let last = heapArray.pop(); // 마지막 노드를 가져옴, 그리고 배열의 크기 1 줄임

	// heapArray.length - 1 로 해야 한다.
	// 왜냐하면 우리는 index를 1부터 사용하기 위해 heapArray의 맨 앞에 null을 넣어서
	while (child <= heapArray.length - 1) {
		// 오른쪽 자식이 있다면, 왼쪽 자식과 오른쪽 자식 비교
		if (
			child < heapArray.length - 1 &&
			heapArray[child] < heapArray[child + 1]
		) {
			child++;
		}

		// 만약 마지막 노드가 자식보다 더 크면 반복 종료
		if (last >= heapArray[child]) {
			break;
		}

		// 마지막 노드가 자식보다 작으면 부모를 자식 노드로 교환
		heapArray[parent] = heapArray[child];

		// 한 단계 아래로 이동
		parent = child;
		child *= 2;
	}

	// 마지막에 노드를 올바른 위치를 넣음으로 재구성 완료
	heapArray[parent] = last;

	// 아까 저장해놨던 루트 노드 값 반환
	return item;
};
```

### 05. HeapSort 알고리즘 및 코드

```jsx
// insertMaxHeap (위에 구현)
const insertMaxHeap = (heapArray, item) => {};
// deleteMaxHeap (위에 구현)
const deleteMaxHeap = (heapArray) => {};

const HeapSort = (array) => {
	const heapArray = [null]; // 1. 편의성을 위해 0번 index는 사용하지 않는다.
	const resultArray = [];

	for (let i = 0; i < array.length; i++) {
		// 2. 하나씩 다 삽입
		insertMaxHeap(heapArray, array[i]);
	}

	for (let i = 0; i < array.length; i++) {
		// 3. 하나씩 다 추출
		resultArray.unshift(deleteMaxHeap(heapArray));
	}

	return resultArray;
};

console.log(HeapSort([9, 7, 6, 5, 4, 3, 2, 2, 1, 3]));
```

1. 편의성을 위해 heapArray의 맨 앞에 null을 넣어준다.
   - 왼쪽 자식 index = 부모 index _ 2, 오른쪽 자식 index = 부모 index _ 2 + 1 이 성립된다.
2. MaxHeap에 배열의 값을 하나씩 넣어준다.
3. MaxHeap에서 값을 하나씩 빼면서 배열의 맨 앞에 계속 넣어준다.
   - 최대 힙이라서 추출시 최대값이 나와서, 계속 뒤로 밀어줌으로써 오름차순으로 만들기 위해

### 04. 특징

- **장점**
  - **시간 복잡도가 좋은 편**이다.
- 힙 정렬이 **유용할 경우**는 전체 자료를 정렬하는 것이 아니라 **가장 큰 값 몇 개만 필요할 때**이다.

### 05. 복잡도

- **시간 복잡도**
  - **힙 트리의 전체 높이는 거의 logn**이다. (완전 이진트리라서)
  - 따라서 하나의 요소를 힙에 삽입하거나 삭제할 때 힙을 재정비하는 시간이 logn만큼 소요된다.
  - 요소의 개수가 n개이므로 전체적으로 O(nlogn)의 시간이 걸린다.
  - **T(n) = O(nlogn)**

### 06. 참고

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html](https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html)
