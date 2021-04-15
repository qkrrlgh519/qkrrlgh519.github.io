---
layout: post
title: "이것이 코딩테스트다 - [Sort] 퀵 정렬 by JavaScript"
date: 2021-04-15 12:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 퀵 정렬

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 이번에 알아볼 것은 퀵 정렬입니다.
- [Quick Sort (퀵 정렬)](<https://qkrrlgh519.github.io/algorithm(sort)/2021/01/21/Algorithm-Sort-Quick.html>) 에 대해서 포스트를 한 적이 있는데, 추가적으로 알게된 내용을 중점적으로 정리하겠습니다.

## 문제 풀이 step 2

- (제가 생각하는) 퀵 정렬의 키포인트는
  1. **pivot** 과 **partition** 함수
  2. **기준 데이터**를 설정하고, **그 기준보다 큰 데이터와 작은 데이터의 위치**를 바꾸는 알고리즘
- 저는 첫 번째 키포인트로 기억을 했었는데, 나동빈 저자님께서 설명해주신 두 번째 키포인트가 훨씬 직관적이고 구현 방법을 담고있어서 두 번째로 기억하는 것을 추천드립니다.

## 문제 풀이 step 3

- 퀵 정렬에서 중요한 것은 pivot 을 기준으로 분할하는 방식인데요. 이번에 구현한 퀵 정렬은 호어 분할 (Hoare Partition) 방식을 이용합니다.
  - **호어 분할 (Hoare Partition) 방식** : 리스트에서 첫 번째 데이터를 피벗으로 정합니다.
- pivot 을 정했으면 왼쪽에서부터 pivot 보다 큰 데이터를 찾고, 오른쪽에서부터 pivot 보다 작은 데이터를 찾습니다. 그 다음 큰 데이터와 작은 데이터의 위치를 서로 교환해줍니다.
  - 위 과정을 반복하면 pivot 을 기준으로 왼쪽에는 작은 데이터가 오른쪽에는 큰 데이터가 위치하게 됩니다.
- pivot 기준으로 왼쪽과 오른쪽 데이터들이 나뉘게되었으면, 왼쪽 데이터와 오른쪽 데이터에 대해서 각각 퀵 정렬을 수행합니다.
  - 이는 재귀 함수로 표현됨으로써 상당히 간결하게 표현할 수 있습니다.
- **재귀 형태**로 동작하는 퀵 정렬에서 중요한 것은 재귀의 종료 조건이겠습니다.
  - 재귀가 종료되는 시점은 주어지는 배열의 원소가 1 개 일 경우입니다.
  - 원소가 하나인 배열은 반으로 나눌 수 없겠죠??
- 나머지 퀵 정렬에 대한 내용은 소스코드에 주석으로 설명드리겠습니다.

## 문제 풀이 step 4

- 퀵 정렬의 평균 시간 복잡도는 `O(NlogN)` 입니다.
- 그리고 최악의 경우 시간 복잡도는 `O(N^2)` 입니다. 지금처럼 리스트의 가장 왼쪽 데이터를 피벗으로 삼을 때, **이미 데이터가 정렬되어 있는 경우** 매우 느리게 동작합니다.

## 소스 코드

```jsx
const array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8];

const partition = (array, start, end) => {
	// pivot 은 첫 번째 원소로 설정
	const pivot = start;
	let left = start + 1;
	let right = end;

	// 왼쪽과 오른쪽이 엇갈리게 될 때까지
	while (left <= right) {
		// pivot 보다 큰 데이터를 찾을 때까지 반복
		while (left <= end && array[left] <= array[pivot]) {
			left++;
		}

		// pivot 보다 작은 데이터를 찾을 때까지 반복
		// start < right 조건은 정말 중요합니다. 이 부분을 start <= right 라고 할 경우 무한 반복에 빠질 수 있습니다.
		// start 를 pivot 으로 정했으니 당연한 조건이겠죠??
		while (start < right && array[right] >= array[pivot]) {
			right--;
		}

		// 아직 엇갈리지 않았다면 큰 데이터와 작은 데이터를 SWAP
		if (left <= right)
			[array[left], array[right]] = [array[right], array[left]];
	}

	// 엇갈리고 나서 right 에는 pivot 보다 작은 데이터들 중에서 가장 오른쪽 값이므로, pivot 과 right 를 SWAP
	[array[pivot], array[right]] = [array[right], array[pivot]];

	return right;
};

const quickSort = (array, start, end) => {
	// 원소가 1 개일 경우 재귀 종료
	if (start >= end) return;

	const pivot = partition(array, start, end);

	// pivot 기준 왼쪽 리스트와 오른쪽 리스트에 대해서 각각 QuickSort 진행
	quickSort(array, start, pivot - 1);
	quickSort(array, pivot + 1, end);
};

quickSort(array, 0, array.length - 1);
console.log(array);
```
