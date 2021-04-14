---
layout: post
title: "이것이 코딩테스트다 - [Sort] 삽입 정렬 by JavaScript"
date: 2021-04-14 17:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 삽입 정렬

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 이번에 알아볼 것은 삽입 정렬입니다.
- [Insertion Sort (삽입 정렬)](<https://qkrrlgh519.github.io/algorithm(sort)/2021/01/17/Algorithm-Sort-Insertion.html>) 에 대해서 포스트를 한 적이 있는데 복습 차원에서 간단하게 정리하고 가겠습니다.

## 문제 풀이 step 2

- (제가 생각하는) 삽입 정렬의 키포인트는
  1.  **손 안의 카드를 재정렬**하는 알고리즘 (카드 게임시 새로운 카드가 들어오면, 기존의 정렬된 카드 사이에서 올바른 위치에 삽입)
  2.  특정한 데이터를 적절한 위치에 삽입하는 알고리즘
- 두 가지 키포인트 모두 같은 내용을 말하고 있기에, 더욱 기억하기 쉬운 것으로 선택하시면 될 것 같습니다.
- 저는 삽입 정렬이 손 안의 카드를 재정렬하는 방식과 거의 똑같이 동작하기에 1 번을 키포인트로 잡았습니다.
  - **카드 재정렬** : 손 안의 카드를 재정렬할 때, 손 안에는 카드가 이미 정렬이 되어 있습니다. 그리고 카드를 하나 집으면 손 안의 카드들 중에서 올바른 위치에 삽입을 합니다.
  - **삽입 정렬** : 이미 왼쪽의 수들은 정렬이 되어 있고, 오른쪽에서 새로운 수를 보고 왼쪽의 정렬된 수들 사이에 적합한 위치에 삽입합니다.
  - 방식이 거의 똑같죠??
- 키포인트의 의미대로 구현하면 되겠지만, 구현적인 측면에서 조금 달라지는 게 있습니다.
  - 사람은 손 안의 카드 중에서 적합한 위치에 삽입하는 것이 쉽지만, 컴퓨터는 적합한 위치에 삽입을 하면 나머지 카드들을 오른쪽으로 미는 과정이 필요합니다. 그래서 이 부분만 조금 달라집니다.
  - 간단하게 오른쪽에서 하나씩 비교하면서 새로운 수가 적합한 위치에 도달할 때까지 SWAP 해주면 되겠습니다.
- 삽입 정렬의 시간복잡도는 `O(N^2)` 입니다.

## 소스 코드

```jsx
const array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8];

const insertionSort = (array) => {
	// 1. 0 번 index 는 이미 정렬되어 있다고 생각하고 1 번 index 부터 시작
	for (let i = 1; i < array.length; i++) {
		// 2. 왼쪽의 정렬된 수들 중에서 비교
		for (let j = i; j > 0; j--) {
			if (array[j - 1] < array[j]) break;

			// 3. 삽입할 위치에 도달할 때까지 SWAP
			[array[j - 1], array[j]] = [array[j], array[j - 1]];
		}
	}

	console.log(array);
};

insertionSort(array);
```
