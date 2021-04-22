---
layout: post
title: "이것이 코딩테스트다 - [BinarySearch] 이진 탐색 by JavaScript"
date: 2021-04-22 15:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 이진 탐색

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 이번에 알아볼 것은 이진 탐색입니다.
- 예전에 [이진 탐색](<https://qkrrlgh519.github.io/algorithm(search)/2021/02/09/Algorithm-Search-Binary.html>) 에 대해서 포스트한 적이 있는데, 추가적으로 알게된 내용을 중점적으로 정리하고 넘어가겠습니다.

## 문제 풀이 step 2

- 이진 탐색은 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘입니다.
- 이번 예제는 정렬된 배열이 들어와서 상관없지만, 만약 정렬되지 않은 배열이 주어진다면, 정렬을 한 후에 이진 탐색을 적용하시면 되겠습니다.
- 이진 탐색의 동작 원리는 시작점, 끝점, 중간점 이 3 가지를 이용해서, 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는 것입니다.
- 이진 탐색의 시간 복잡도는 **O(logN)** 입니다. (정렬하는 과정 제외)
- 자세한 것은 소스코드로 알아보겠습니다.

## 소스 코드 (재귀 형식)

```jsx
const binarySearchByRecursion = (arr, target, start, end) => {
	if (start > end) return null;

	const mid = parseInt((start + end) / 2);
	// 찾은 경우 중간점 반환
	if (arr[mid] === target) return mid;

	// 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
	if (arr[mid] > target)
		return binarySearchByRecursion(arr, target, start, mid - 1);
	// 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
	else return binarySearchByRecursion(arr, target, mid + 1, end);
};
```

## 소스 코드 (반복문 형식)

```jsx
const binarySearchByLoop = (arr, target, start, end) => {
	while (start <= end) {
		const mid = parseInt((start + end) / 2);
		// 찾은 경우 중간점 반환
		if (arr[mid] === target) return mid;

		// 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
		if (arr[mid] > target) end = mid - 1;
		// 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
		else start = mid + 1;
	}

	return null;
};
```
