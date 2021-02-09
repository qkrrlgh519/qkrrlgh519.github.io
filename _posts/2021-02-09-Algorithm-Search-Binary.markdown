---
layout: post
title: "Binary Search (이진 탐색)"
date: 2021-02-09 10:30:00 +0900
categories: Algorithm(Search)
---

## 1. 정의

- **배열의 중앙에 있는 값을 조사**하여 찾고자 하는 항목이 **왼쪽 또는 오른쪽 부분 배열에 있는지를 알아내어** 탐색의 범위를 반으로 줄인다.
- 이러한 방법에 의해 **매 단계에서 검색해야 할 리스트의 크기를 반으로 줄여**가며 찾고자 하는 항목을 탐색하는 알고리즘이다.

## 2. 특징

- **정렬된 배열의 탐색**에는 이진 탐색이 가장 적합하다.
- 이진 탐색에서는 비교가 이루어질 때마다 탐색의 범위가 급격하게 줄어든다. **찾고자 하는 항목이 속해있지 않은 부분은 전혀 고려할 필요가 없기 때문**이다.
- 이진 탐색을 적용하려면 **탐색하기 전에 배열이 반드시 정렬**되어 있어야 한다. 따라서 데이터 삽입이나 삭제가 빈번할 시에는 적합하지 않고, 주로 **고정된 데이터에 대한 탐색에 적합**하다.

## 3. 동작 과정

- 탐색키 = 34

![Binary Search 전체 과정](/public/img/Search/binarysearch.JPG)

1. 맨 처음에는 low = 0, high = 15이다.
2. **(low + high) / 2를 이용해 middle(7)**을 구한다.
3. middle(7)의 키(27)와 탐색키(34)를 비교한다. 탐색키가 더 크므로 오른쪽 리스트를 탐색한다.
4. low = 8(middle + 1), high = 15이다.
5. 위의 과정을 반복하며 탐색키를 찾는다.

## 4. 의사코드

1. 중앙에 있는 값을 조사
2. 탐색값이 중앙값이 같으면 중앙값 위치 리턴
3. 탐색값이 중앙값보다 작으면 왼쪽 리스트 탐색
4. 탐색값이 중앙값보다 크면 오른쪽 리스트 탐색

```
binary_search(list, low, high):

	middle <- low에서 high사이의 중간 위치
	if(탐색값 == list[middle])
		return middle;
	else if(탐색값 < list[middle])
		return list[0]부터 list[middle-1]]에서의 탐색;
	else if(탐색값 > list[middle])
		return list[middle+1]부터 list[high]에서의 탐색;
```

## 5. 구현

- **순환 호출**

```jsx
const BinarySearch = (arr, key, low, high) => {
	if (low <= high) {
		const middle = parseInt((low + high) / 2);

		if (key === arr[middle]) return middle;
		else if (key < arr[middle]) return BinarySearch(arr, key, low, middle - 1);
		else return BinarySearch(arr, key, middle + 1, high);
	}

	return -1;
};
```

- **반복**

```jsx
const BinarySearch = (arr, key, low, high) => {
	while (low <= high) {
		const middle = parseInt((low + high) / 2);

		if (key === arr[middle]) return middle;
		else if (key < arr[middle]) high = middle - 1;
		else low = middle + 1;
	}

	return -1;
};
```

## 6. 시간 복잡도

- 이진 탐색은 **탐색을 반복할 때마다 탐색 범위를 반으로** 줄인다.
- 이러한 탐색 범위가 더 이상 줄일 수 없는 1이 될 때의 탐색 횟수를 k라 하면, 1=2^k이다.
- 따라서, **시간 복잡도는 O(logn)**이다.

## 7. 참고

- C언어로 쉽게 풀어쓴 자료구조 (천인국, 공용해, 하상호 지음)
- [https://m.blog.naver.com/PostView.nhn?blogId=jkssleeky&logNo=220715543451&proxyReferer=http:%2F%2F203.233.19.54%2F](https://m.blog.naver.com/PostView.nhn?blogId=jkssleeky&logNo=220715543451&proxyReferer=http:%2F%2F203.233.19.54%2F)
