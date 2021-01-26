---
layout: post
title: "Counting Sort (계수 정렬)"
date: 2021-01-26 23:30:00 +0900
categories: Algorithm(Sort)
---

### 00. 정의

- 원소 값들의 **갯수를 세고, 이를 이용해서 정렬**하는 알고리즘이다.

### 01. 요약

- **안정 정렬**이다.
- **추가적인 메모리를 요구**하는 알고리즘이다.
- 기수 정렬과 마찬가지로, 입력 데이터에 대해서 **어떤 비교 연산도 실행하지 않고**, 데이터를 정렬할 수 있는 **색다른** 정렬 기법
- 기수 정렬과 마찬가지로, 정렬에 기초한 방법들은 절대 **O(nlogn)이라는 이론적인 하한선**을 깰 수 없는데 반해, 계수 정렬도 이 **하한선을 깰 수 있는 유일한 기법**이다.

### 02. 원리 (작동 방식)

1. 먼저 배열 내에 원소 값들의 **갯수를 저장**하는 **카운팅 배열 (Counting Array)**를 만든다.

![CountingSort 전체 과정](/public/img/Sort/countingsort1.JPG)

---

2. Counting Array의 요소들을 0 ~ n-1까지 돌면서 **이전 값들을 누적해서 더해준다. (누적합이 index의 역할을 해준다.)**

![CountingArray 만들기 (누적합)](/public/img/Sort/countingsort2.JPG)

---

3. 입력배열과 동일한 크기의 출력배열을 만들고, **입력 배열의 역순으로 출력 배열에 요소들을 채워** 넣어준다. **한 요소를 넣으면 CountingArray의 값을 1만큼 줄인다.** (역순으로 하면 안정 정렬의 조건을 만족할 수 있다.)

![CountingArray 결과](/public/img/Sort/countingsort3.JPG)

- `CountingArray[0] = 1` : 1에 0을 넣는다.
- `CountingArray[1] = 5` : 2~5에 1을 넣는다.
- `CountingArray[2] = 6` : 6에 2을 넣는다.
- 위 과정을 배열의 끝까지 반복

![CountingArray를 이용한 배열 정렬](/public/img/Sort/countingsort4.JPG)

### 03. 순서도 (알고리즘)

- 입력 배열은 0 이상의 정수로 구성되어 있다고 가정한다.

1. 입력 배열에서 최댓값을 찾는다.
2. CountingArray를 선언한다. 각 숫자가 몇 번 등장하는지 세어준다.
3. CountingArray를 처음부터 끝까지 돌면서 이전 값을 합해주면서 누적합으로 바꿔준다.
4. 입력 배열을 역순으로 돌면서, 누적합을 참고해서 출력 배열에 요소를 넣어준다.

```jsx
// 입력 배열은 0 이상의 정수로 구성되어 있다고 가정한다.

const CountingSort = (array) => {
	const arrayLen = array.length;
	let max = -1;
	const outputArr = [];

	for (let i = 0; i < arrayLen; i++) {
		if (max < array[i]) max = array[i]; // 1.
	}

	const countingArray = Array.from({length: max + 1}, () => 0); // 2.
	for (let i = 0; i < arrayLen; i++) {
		countingArray[array[i]]++; // 각 숫자가 몇 번 등장하는지 세기
	}

	for (let i = 1; i < countingArray.length; i++) {
		countingArray[i] === 0 // 3.
			? (countingArray[i] = countingArray[i - 1])
			: (countingArray[i] += countingArray[i - 1]);
	}
	// 0인지 비교를 통해서 입력 배열에 없는 값들은 누적시키지 않는다.
	// (누적시키면 아주 끔찍한 결과가 나올 것이다.)

	for (let i = arrayLen - 1; i >= 0; i--) {
		outputArr[--countingArray[array[i]]] = array[i]; // 4. 요소를 넣을 때마다 누적합 감소
	}

	return outputArr;
};

const test = [3, 4, 0, 1, 2, 4, 2, 4, 10];
console.log(CountingSort(test));
```

### 04. 특징

- **장점**
  - **안정 정렬**이다.
  - 다른 정렬 방법에 비하여 비교적 **빠른 수행 속도**를 가진다. (비교하는 과정이 없으므로)
- **단점**
  - 대부분의 상황에 **엄청난 메모리 낭비**를 야기한다. 예를 들어 배열이 [0, 2, 0, 100, 2, 0]일 경우 CountingArray는 길이를 100으로 잡는 낭비를 해야한다.
- CountingSort는 정렬하는 숫자가 **특정한 범위 안에 있을 때 사용**하게 된다.

### 05. 복잡도

- **시간 복잡도**
  - 시간 복잡도는 **최상, 평균, 최악**의 경우 모두 **O(n)**이다.
  - T(n) = O(n)

### 06. 시간 복잡도 자세히 알아보기

- 시간 복잡도를 **정확히** 말하자면 **O(n + k)**이다. **k**는 리스트 중 **가장 큰 값**을 가리킨다.
  - 입력 배열에서 각 숫자가 몇 개씩 등장하는지 계산하는 데 **O(n)**
  - 입력 배열에서 역순으로 출력 배열에 입력하는데 **O(n)**
  - 하지만, 직전 요소 값을 누적하며 더하는데 **O(k)**
  - **n이 10이어도 k(최대값)가 100이면 반복문은 k에 영향**을 받는다.
  - 따라서 **최대값이 매우 큰 값**이라면 CountingSort는 **비효율적인 알고리즘**인 것을 알 수 있다.

### 07. 참고 링크

- [https://bowbowbow.tistory.com/8](https://bowbowbow.tistory.com/8)
- [https://soobarkbar.tistory.com/101](https://soobarkbar.tistory.com/101)
- [https://www.zerocho.com/category/Algorithm/post/58006da88475ed00152d6c4b](https://www.zerocho.com/category/Algorithm/post/58006da88475ed00152d6c4b)
- [https://ordo.tistory.com/91](https://ordo.tistory.com/91)
