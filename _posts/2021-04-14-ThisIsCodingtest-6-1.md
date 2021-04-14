---
layout: post
title: "이것이 코딩테스트다 - [Sort] 선택 정렬 by JavaScript"
date: 2021-04-14 16:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 선택 정렬

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 정렬이란 데이터를 특정한 기준에 따라서 순서대로 나열하는 것을 말합니다.
- 프로그램이나 문제에서 데이터를 가공할 때 오름차순이나 내림차순 등 대부분 어떤 식으로든 정렬해서 사용하는 경우가 많아서 정렬 알고리즘은 프로그램을 작성할 때 가장 많이 사용되는 알고리즘입니다.
- 처음으로 알아볼 것은 선택 정렬입니다.
- 저도 [Selection Sort (선택 정렬)](<https://qkrrlgh519.github.io/algorithm(sort)/2021/01/16/Algorithm-Sort-Selection.html>) 에 대해서 포스트를 한 적이 있는데 복습 차원에서 간단하게 정리하고 가겠습니다.
- 제 개인적인 생각으로는 정렬에 대한 개념과 코드를 이해를 해도 스스로 구현을 하려면 각 정렬의 키포인트에 대해서 알아야 한다고 생각합니다.
- 왜냐하면, 정렬의 종류가 많고 이름들이 비슷하기 때문입니다.

## 문제 풀이 step 2

- (제가 생각하는) 선택 정렬의 키포인트는
  1.  배열에서 **해당 자리(index) 를 선택**하고, 그 자리에 오는 값을 찾는 알고리즘
  2.  무작위 데이터 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸고, 그 다음 작은 데이터를 선택해서 앞에서 두 번째 데이터와 바꾸는 방식으로 매번 **'가장 작은 것을 선택'**하는 알고리즘
- 저는 1 번으로 기억을 했었는데, 나동빈 저자님께서 소개해주신 2 번이 더 직관적이고 원리를 담고 있어서 참고하시면 좋을 것 같습니다. 중요한 것은 어떤 것을 매개체로 하던 원리를 이해하고, 구현하는 것이라고 생각합니다.
- 구현은 간단합니다. 키포인트의 의미대로 구현하면 되겠습니다.
- 선택 정렬의 시간 복잡도는 `O(N^2)` 입니다.

## 소스 코드

```jsx
const array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8];

const selectionSort = (array) => {
	for (let i = 0; i < array.length; i++) {
		// 1. 맨 앞부터
		let minIdx = i;
		for (let j = i + 1; j < length; j++) {
			// 2. 가장 작은 값의 index 찾기
			if (array[minIdx] > array[j]) minIdx = j;
		}

		// 3. 맨 앞의 값과 SWAP 하기
		[array[i], array[minIdx]] = [array[minIdx], array[i]];
	}

	console.log(array);
};

selectionSort(array);
```
