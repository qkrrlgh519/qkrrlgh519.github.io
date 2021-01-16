---
layout: post
title: "Selection Sort (선택 정렬)"
date: 2021-01-16 23:00:00 +0900
categories: Algorithm(Sort)
---

### 01. 요약

- **제자리 정렬** (추가 메모리 요구 X)
- 해당 순서에 원소를 넣을 위치(index)는 이미 정해져 있고, 어떤 원소를 넣을지 선택하는 알고리즘
- 쉽게 말해, **배열에서 해당 자리(index)를 선택하고, 그 자리에 오는 값을 찾는 알고리즘**

### 02. 원리 (작동 방식)

- 입력 배열에서 **최소값을 발견**한 다음, 이 최소값을 배열의 **첫 번째 요소와 교환**한다.
- 다음에는 **첫 번째 요소를 제외한 나머지 요소 중**에서 **가장 작은 값을 선택**하고, 이를 **두 번째 요소와 교환**한다.
- 위 절차를 (요소 개수 - 1)만큼 되풀이한다.

### 03. 순서도 (알고리즘)

```jsx
const SelectionSort = (array) => {
	const length = array.length;
	let minIndex, temp, i, j;

	for (i = 0; i < length - 1; i++) {
		// 1. (length - 1 주의!!)
		minIndex = i;
		for (j = i + 1; j < length; j++) {
			// 2.
			if (array[j] < array[minIndex]) {
				// 3.
				minIndex = j;
			}
		}

		// 4. swap
		temp = array[minIndex];
		array[minIndex] = array[i];
		array[i] = temp;
	}

	return array;
};
```

1. 위치(index)를 선택
2. 현 위치 다음(index + 1)부터 끝까지 반복문 돌면서 선택한 위치(index)의 값과 비교
3. (오름차순) 현재 선택한 위치(index)에 있는 값 보다 순회하고 있는 값이 작다면, 위치(minIndex) 갱신
4. '2'번 반복문이 끝난 뒤, minIndex에 '1'번에서 선택한 위치(index)에 들어가야할 값의 위치가 들어있으므로, 교환해준다.

### 04. 특징

- **장점**
  - **자료 이동 횟수가 미리 결정**된다.
  - Bubble Sort와 마찬가지로 **단순한 알고리즘**이다.
  - 정렬을 위한 비교 횟수는 많지만 Bubble Sort에 비해 실제로 교환하는 횟수는 적기 때문에 **많은 교환이 일어나야 하는 자료상태에서 비교적 효율적**이다.
- **단점**
  - 시간 복잡도가 **O(n^2)으로 비효율적**이다.
  - **불안정 정렬**이다.

### 05. 복잡도

- **시간 복잡도**
  - 데이터의 개수가 n개라고 했을 때,
    - 첫 번째 회전에서의 비교 횟수 : 1 ~ (n-1) = n-1
    - 두 번째 회전에서의 비교 횟수 : 2 ~ (n-1) = n-2
    - 총 (n-1) + (n-2) + ... + 2 + 1 = n(n-1)/2
  - 비교하는 것이 상수 시간에 이루어진다는 가정 아래, **n개의 주어진 배열을 정렬**하는 데 **O(n^2)** 만큼의 시간이 걸린다.
  - **최선, 평균, 최악** 모두 **O(n^2)**으로 동일
- **공간 복잡도**
  - **주어진 배열 안에서** 교환(SWAP)을 통해, 정렬이 수행되므로 **O(n)** 이다.

### 06. 참고

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/05/06/algorithm-selection-sort.html](https://gmlwjd9405.github.io/2018/05/06/algorithm-selection-sort.html)
- [https://gyoogle.dev/blog/algorithm/Selection Sort.html](https://gyoogle.dev/blog/algorithm/Selection%20Sort.html)
