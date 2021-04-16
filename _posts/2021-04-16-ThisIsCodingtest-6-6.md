---
layout: post
title: "이것이 코딩테스트다 - [Sort] 계수 정렬 by JavaScript"
date: 2021-04-16 12:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 계수 정렬

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 이번에 알아볼 것은 계수 정렬입니다.
- [Counting Sort (계수 정렬)](<https://qkrrlgh519.github.io/algorithm(sort)/2021/01/26/Algorithm-Sort-Counting.html>) 에 대해서 포스트를 한 적이 있는데, 추가적으로 알게된 내용을 중점적으로 정리하겠습니다.

## 문제 풀이 step 2

- (제가 생각하는) 계수 정렬의 키포인트는
  1. 원소 값들의 개수를 세고, 이를 이용해서 정렬하는 알고리즘
- 상당히 간단합니다. 하지만, 특정 조건이 부합할 때만 사용할 수 있습니다.
  - 데이터의 크기 범위가 제한되어 정수 형태로 표현할 수 있을 때만 사용할 수 있습니다.
  - 일반적으로 가장 큰 데이터와 가장 작은 데이터의 차이가 1000000 을 넘지 않을 때 효과적으로 사용할 수 있습니다.
- 기존에 다뤘던 정렬 방식과 다르게 비교 기반의 정렬 알고리즘이 아닙니다.
- 별도의 리스트를 선언하고 그 안에 정렬에 대한 정보를 담는다는 특징이 있습니다. 정렬에 대한 정보는 원소 값의 개수를 의미하겠죠??
- 자세한 내용은 소스 코드에 주석으로 설명하겠습니다.

## 문제 풀이 step 3

- 모든 데이터가 양의 정수인 상황에서 데이터의 개수를 N , 데이터 중 최대값의 크기를 K 라고 할 때, 계수 정렬의 시간복잡도는 `O(N + K)` 입니다.
  - n 이 10 이어도 K 가 100 이면 시간 복잡도는 K 에 영향을 받기 때문에 K 가 매우 크다면 효율이 많이 떨어지는 알고리즘이라고 볼 수 있겠습니다.

## 소스 코드

```jsx
const array = [7, 5, 9, 0, 3, 1, 6, 2, 9, 1, 4, 8, 0, 5, 2];

const countSort = (array) => {
	const countArray = [];
	for (let i = 0; i < array.length; i++) {
		if (countArray[array[i]] === undefined) countArray[array[i]] = 0;

		// 1. 각 원소들의 개수를 셉니다.
		countArray[array[i]] += 1;
	}

	const resultArray = [];
	for (let i = 0; i < countArray.length; i++) {
		// 2. 결과 배열에 각 원소들의 개수만큼 넣어줍니다. (차례대로)
		while (countArray[i]) {
			resultArray.push(i);
			countArray[i] -= 1;
		}
	}

	console.log(resultArray);
};

countSort(array);
```
