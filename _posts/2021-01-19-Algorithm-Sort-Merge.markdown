---
layout: post
title: "Merge Sort (합병 정렬)"
date: 2021-01-19 23:00:00 +0900
categories: Algorithm(Sort)
---

### 01. 요약

- 하나의 리스트를 **두 개의 균등한 크기**로 **분할**하고, 분할된 부분 리스트를 **정렬한 다음**, 두 개의 정렬된 **부분 리스트를 합하여** 전체가 정렬된 리스트가 되게 하는 방법이다.
- **안정 정렬**에 속하며, **분할 정복 알고리즘**의 하나이다.
- **분할 정복 기법**
  - 문제를 작은 2개의 문제로 분리하고 각각을 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 전략
  - 분할 정복 기법은 대개 재귀(순환) 호출을 이용하여 구현된다.
- **포인트**는 **두 가지**로, **합병 정렬의 단계**와 **합병 방법**이다.

### 02. 원리 (작동 방식)

- 합병 정렬에서 **실제로 정렬이 이루어지는 시점**은 2개의 리스트를 **합병(merge)하는 단계**이다.
- 추가적인 리스트를 필요로 한다.

  ![합병 정렬 각 단계](/public/img/Sort/mergesort1.JPG)

- **합병 정렬의 각 단계**

  1. **분할 (Divide)** : 입력 배열을 **같은 크기의 2개의 부분 배열**로 분할한다. (크기가 1이면 더이상 분할하지 않는다.)
  2. **정복 (Conquer)** : 부분 배열을 정렬한다. 부분 배열의 크기가 **충분히 작지 않으면 재귀(순환) 호출**을 이용해 다시 분할 정복 기법을 적용한다.
  3. **결합 (Combine)** : 정렬된 부분 배열들을 하나의 배열에 합병한다.


      ![합병 과정](/public/img/Sort/mergesort2.JPG)

- **합병(merge) 방법**
  - 2개의 리스트의 요소들을 처음부터 **하나씩 비교**하여, 두 개의 리스트의 요소 중에서 **더 작은 요소를 새로운 리스트(sorted)로** 옮긴다.
  - 둘 중에서 **하나가 끝날 때까지** 이 과정을 되풀이한다.
  - 만약 둘 중에서 하나의 리스트가 먼저 끝나게 되면 **나머지 리스트의 요소들을 전부 새로운 리스트로 복사**하면 된다. (이전에 정렬을 했으니까)
  - 새로운 리스트(sorted)를 원래의 리스트(list)로 옮긴다.

### 03. 순서도 (알고리즘)

- **논리적 순서도**
  1. 재귀적으로 MergeSort 함수를 구현한다. (입력 배열을 2등분 해서 각각 배열을 매개변수로 MergeSort를 호출하고, 그 다음 합병하는 Merge 함수를 호출한다.)
  2. Merge 함수는 왼쪽 부분리스트와 오른쪽 부분리스트의 원소들을 맨 왼쪽부터 하나씩 비교하면서 작은 값들을 sorted 배열에 넣어준다. (이때 왼쪽 또는 오른쪽 중 하나가 끝나면 마치지 못한 배열을 그대로 sorted 배열에 넣어준다.) 그리고 sorted 배열을 원래 배열에 옮겨준다.
  3. index를 이용해서 처리하는 방식이기에 추가 메모리는 sorted 배열밖에 없다.

```jsx
// 방법 1. 정석적인 방법

const sorted = [];

const merge = (array, left, mid, right) => {
	let i = left; // 왼쪽 분할 리스트의 맨 왼쪽 index
	let j = mid + 1; // 오른쪽 분할 리스트의 맨 왼쪽 index
	let k = left; // sorted 배열의 맨 왼쪽 index
	let l;

	while (i <= mid && j <= right) {
		// 왼쪽 or 오른쪽 분할리스트 중 하나라도 다 돌면 break
		if (array[i] <= array[j]) {
			// 오름차순 정렬이므로 더 작은 요소를 sorted에 넣는다.
			sorted[k++] = array[i++];
		} else {
			sorted[k++] = array[j++];
		}
	}

	if (i <= mid) {
		// 왼쪽 분할리스트를 덜 돌은 경우 (이미 정렬된 상태이니 옮기기만 하면 됨)
		for (l = i; l <= mid; l++) {
			sorted[k++] = array[l];
		}
	} else {
		// 오른쪽 분할리스트를 덜 돌은 경우
		for (l = j; l <= right; l++) {
			sorted[k++] = array[l];
		}
	}

	for (l = left; l <= right; l++) {
		// 정렬된 리스트를 원래 리스트에 옮긴다.
		array[l] = sorted[l];
	}
};

const MergeSort = (array, left, right) => {
	let mid;

	if (left < right) {
		mid = parseInt((left + right) / 2);
		MergeSort(array, left, mid);
		MergeSort(array, mid + 1, right);
		merge(array, left, mid, right);
	}
};

const array = [5, 3, 1, 2, 6, 4];
MergeSort(array, 0, 5);
console.log(array);
```

```java
// 방법 2. 더 짧고, 더 직관적인 방식이다. 허나 동작 방식이 조금 다름

const merge = (leftArray, rightArray) => {
	let sortedArray = [];

	while (leftArray.length && rightArray.length) {
		if (leftArray[0] <= rightArray[0]) {
			sortedArray.push(leftArray.shift());
		} else {
			sortedArray.push(rightArray.shift());
		}
	}

	while (leftArray.length) {
		sortedArray.push(leftArray.shift());
	}
	while (rightArray.length) {
		sortedArray.push(rightArray.shift());
	}

	return sortedArray;
};

const MergeSort = (array) => {
	if (array.length < 2) return array;
	let mid = parseInt(array.length / 2);
	const leftArray = array.slice(0, mid);
	const rightArray = array.slice(mid, array.length);
	return merge(MergeSort(leftArray), MergeSort(rightArray));
};

const array = [5, 3, 1, 2, 6, 4];
console.log(MergeSort(array));
```

- **합병 정렬 단계**와 **합병 방법**을 기반으로 구현
- **방법 1**은 실제로 함수의 매개변수로 index 값을 넘김으로써 추가적인 배열은 sorted만 생기는 반면, **방법 2**는 매개변수로 배열을 넘김으로 추가적인 배열이 계속 생성된다는 단점이 존재 (헷갈릴 경우 정석적인 방법 1을 추천)

### 04. 특징

- **장점**
  - **안정 정렬**이다.
  - 데이터 분포의 영향을 덜 받는다. 즉, **입력 데이터가 무엇이든 간 정렬시간은 O(nlogn)**으로 동일하다.
  - **만약 레코드를** **연결리스트로 구성하여 합병 정렬할 경우**, 링크 인덱스만 변경되므로 **데이터의 이동은 무시할 수 있을 정도로 작아**진다.
  - 따라서 크기가 큰 레코드를 정렬할 경우 연결리스트를 사용한다면, 합병 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법보다 효율적이다.
- **단점**
  - **임시 배열이 필요**하다. (즉, 제자리 정렬이 아니다)
  - 만약 레코드의 크기가 큰 경우 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래한다.

### 05. 복잡도

- **시간 복잡도**
  - 분할 단계
    1. 비교 연산과 이동 연산이 수행되지 않는다.
  - 합병 단계
    1. 비교 연산은 순환 호출의 깊이 \* 각 합병 단계의 비교 연산 수로 **nlogn**이다.
    2. 이동 연산은 순환 호출의 깊이 \* 각 합병 단계의 이동 연산 수로 **2nlogn**이다.
  - 결론, **O(nlogn)**이다.

### 06. 시간 복잡도 더 자세히 알아보기!

![합병 정렬 시간복잡도](/public/img/Sort/mergesort3.JPG)

- **순환 호출의 깊이**
  - 레코드의 개수 n이 2의 거듭제곱이라고 가정 (n = 2^k) 했을 때, n = 2^3인 경우
  - **2^3 ⇒ 2^2 ⇒ 2^1 ⇒ 2^0** **순으로 줄어들어 순환 호출의 깊이가 3임**을 알 수 있다.
  - 이것을 일반화 하면 n = 2^k인 경우, **깊이가 k(logn)**임을 알 수 있다.
- **각 합병 단계에서 비교 연산**
  - 크기가 1인 부분 배열 2개를 합병하는 데 최대 2번의 비교연산이 필요하고, 부분 배열의 쌍이 4개이므로 2 \* 4 = 8로 **총 8번**의 비교 연산이 필요
  - 다음 단계) 크기가 2인 부분 배열 2개를 합병하는 데 최대 4번의 비교연산이 필요하고, 부분 배열의 쌍이 2개 이므로 4 \* 2 = 8로 **총 8번**의 비교 연산이 필요하다.
  - 다음 단계) 크기가 4인 부분배열 2개를 합병하는 데 최대 8번의 비교 연산이 필요하고, 부분 배열의 쌍이 1개 이므로 8 \* 1 = 8로 **총 8번**의 비교 연산이 필요하다.
  - 이것을 일반화하면 **하나의 합병 단계**에서 **최대 n번의 비교 연산**을 수행
  - 순환 호출의 깊이(logn) \* 각 합병 단계의 비교 연산(n) = **nlogn**
- **각 합병 단계에서 이동 연산**
  - 임시 배열에 복사했다가 다시 가져와야 되므로, 이동 연산은 총 부분 배열에 들어있는 요소의 개수가 n인 경우, **레코드의 이동이 2n번 발생**한다.
  - 순환 호출의 깊이(logn) \* 각 합병 단계의 이동 연산(2n) = **2nlogn**
- **T(n)** = nlogn(비교) + 2nlogn(이동) = 3nlogn ⇒ **O(nlogn)**

### 07. 참고

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html](https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html)
- [https://www.zerocho.com/category/Algorithm/post/57ee1fc107c0b40015045cb4](https://www.zerocho.com/category/Algorithm/post/57ee1fc107c0b40015045cb4)
