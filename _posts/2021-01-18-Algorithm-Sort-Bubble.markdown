---
layout: post
title: "Bubble Sort (거품 정렬)"
date: 2021-01-18 22:00:00 +0900
categories: Algorithm(Sort)
---

### 01. 요약

- **제자리 정렬** (추가 메모리 요구 X)
- **Selection Sort와 유사**한 알고리즘
- **서로 인접한 두 원소를 검사**하여 크기가 순서대로 되어 있지 않으면 서로 교환하는 **비교-교환 과정을 배열의 왼쪽 끝에서 시작해 오른쪽 끝까지 진행하며 정렬**하는 알고리즘

### 02. 원리 (작동 방식)

- 첫 번째 자료와 두 번째 자료를, 두 번째 자료와 세 번째 자료를 비교하는 식으로 (n-1)번째 자료와 n번째 자료를 비교하여 교환하며 자료를 정렬한다.
- **1회전을 수행하고 나면 가장 큰(작은) 자료가 맨 뒤로 이동**하므로 **2회전에서는 맨 끝에 있는 자료는 정렬에서 제외**된다.
- 이렇게 정렬을 1회전 수행할 때마다 정렬에서 제외되는 데이터가 하나씩 늘어난다.

### 03. 순서도 (알고리즘)

```jsx
const BubbleSort = (array) => {
	let length = array.length;
	let temp, i, j;

	for (i = length - 1; i > 0; i--) {
		// 1.
		for (j = 0; j < i; j++) {
			// 2.
			if (array[j] > array[j + 1]) {
				// 3.
				temp = array[j];
				array[j] = array[j + 1];
				array[j + 1] = temp;
			}
		}
	}

	return array;
};
```

1. 0 ~ (i - 1) 까지 반복하며 매 회전시마다 마지막 위치에는 정렬된 원소가 위치하므로 하나씩 줄여준다.
2. 매 회전마다 검사 범위가 1씩 줄어들면서 반복을 진행한다.
3. 인접한 두 요소의 값을 비교, 교환하며 정렬을 진행한다.

### 04. 특징

- **장점**
  - 구현이 **매우 간단**하다.
  - **안정 정렬**이다.
- **단점**
  - 순서에 맞지 않은 요소를 인접한 요소와 교환한다.
  - 하나의 요소가 **가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서 배열에서 모든 다른 요소들과 교환**되어야 한다.
  - 특히 **특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환**되는 일이 일어난다.
- 일반적으로 **자료의 교환 작업이 자료의 이동 작업보다 더 복잡**하기 때문에 버블 정렬은 그 단순성에도 불구하고 거의 쓰이지 않는다.

### 05. 복잡도

- **시간 복잡도**
  - **최선, 평균, 최악** 모두 **O(n^2)**으로 일정
    - (n-1) + (n-2) + ... + 2 + 1 = n(n-1)/2
- **공간 복잡도**
  - 주어진 배열 안에서 교환(SWAP)을 통해, 정렬이 수행되므로 **O(n)** 이다.

### 06. 참고

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html](https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html)
- [https://gyoogle.dev/blog/algorithm/Bubble Sort.html](https://gyoogle.dev/blog/algorithm/Bubble%20Sort.html)
