---
layout: post
title: "BOJ[17140] - 이차원 배열과 연산 by JavaScript"
date: 2021-09-13 12:00:00 +0900
categories: BOJ(Implementation)
---

# 이차원 배열과 연산

## 문제

- [백준 17140번 - 이차원 배열과 연산](https://www.acmicpc.net/problem/17140)

## 언어

- JavaScript

## 순서도

1. 100 x 100 크기의 배열에 입력으로 주어진 3 x 3 크기의 배열 입력하기
2. 연산을 하나도 하지 않은 상태에서 r 행 c 열의 값이 k 인 경우 0 return
3. 배열의 행의 길이와 열의 길이를 갱신해가며 r 연산 혹은 c 연산을 100 번 수행하기
   1. 각 행 또는 각 열마다
   2. Map 을 이용해서 각각의 수의 등장 횟수 세기
   3. Map 을 [수, 등장 횟수] 형식으로 배열로 변환하기
   4. 만든 배열 정렬하고, 평탄화 하기
   5. 임시 배열에 넣어두기
   6. 위의 연산이 모두 끝나면 원래 배열에 복사하기
4. 만약 연산 도중에 r 행 c 열의 값이 k 라면 소요 시간 출력
5. 만약 100 번 연산을 했음에도 r 행 c 열의 값이 k 가 아니라면 -1 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 크기가 3 x 3 인 배열 A 가 있습니다.
- R 연산은 배열 A 의 모든 행에 대해서 정렬을 수행합니다. (행의 개수가 열의 개수 이상인 경우)
- c 연산은 배열 A 의 모든 열에 대해서 정렬을 수행합니다. (행의 개수가 열의 개수 미만인 경우)
- 정렬하는 방법은 기존의 정렬과는 조금 다릅니다.
  - 우선, 각각의 수가 몇 번 나왔는지 세어줍니다.
  - 그다음, 수의 등장 횟수가 커지는 순으로 정렬을 합니다.
  - 만약 수의 등장 횟수가 동일한 수가 여러개라면 수가 커지는 순으로 정렬합니다.
  - 그 다음에는 배열 A 에 정렬된 결과를 다시 넣어줍니다. (넣을 때는 수와 등장 횟수를 모두 넣으며, 순서는 수가 먼저입니다.)
    - 예를 들어, [3, 1, 1] = [3, 1, 1, 2]
    - 예를 들어, [3, 1, 1, 2] = [2, 1, 3, 1, 1, 2]
    - 예를 들어, [2, 1, 3, 1, 1, 2] = [3, 1, 2, 2, 1, 3]
- 정렬된 결과를 배열에 다시 넣으면 행 또는 열의 크기가 달라질 수 있습니다.
  - R 연산이 적용된 경우에는 가장 큰 행을 기준으로 모든 행의 크기가 변하고,
  - C 연산이 적용된 경우에는 가장 큰 열을 기준으로 모든 열의 크기가 변합니다.
  - 행 또는 열의 크기가 커진 곳에는 0 이 채워집니다.
  - 단, 수를 정렬할 때 0 은 무시해야 합니다.
  - 행 또는 열의 크기가 100 을 넘어가는 경우에는 처음 100 개를 제외한 나머지는 버립니다.
- 배열 A 와 r, c, k 가 주어질 때, A[r][c] 에 들어있는 값이 k 가 되기 위한 최소 시간을 구하는 문제입니다.

## 문제 풀이 step 2

- 실제로 로직은 위의 문제에 대해서 설명한 대로 구현하면 됩니다.
- 단, 주의해야 할 점이 몇 가지 있습니다.

1. 연산을 할 필요가 없는 경우츨 처리해줘야 합니다.
   - 이 경우는 이미 A[r][c] = k 인 경우입니다. 0 을 return 하면 되겠습니다.
2. 100 초가 지나도 A[r][c] = k 인 경우 -1 을 출력합니다.
   - 100 초를 지났을 때 -1 을 출력해야 하며, 100 초에 A[r][c] = k 인 경우는 100 을 return 해야 합니다.
3. 아예 처음부터 배열 A 를 100 x 100 크기로 만들고 0 으로 초기화한 후에 로직을 작성하는 것이 안전합니다.
   - 대신 행의 크기와 열의 크기를 나타내는 별도의 변수 2 개를 사용하면 크기 문제는 해결할 수 있습니다.
   - R 연산 또는 C 연산을 하는 도중에 배열의 크기가 늘어나는 경우가 생깁니다.
   - 이 때, 열이 늘어나는 것은 크게 문제되진 않는데, 행이 늘어나는 경우 그 행에 배열을 새로 생성한 후에 로직을 풀어나가야 합니다.
   - 이 경우 값이 잘 못 들어가는 문제가 생길 수 있기 때문에 차라리 미리 생성해놓고 사용하는 것이 안전합니다.
4. 정렬을 구현할 때 배열을 사용하는 것 보다는 Map 을 사용하는 것이 더 빠릅니다.
   - 본 문제에서 정렬 방식이 CountingSort 와 비슷해서 배열로 하려 했으나,
   - 이렇게 하면 배열의 값 중에서 undefined 인 공간을 탐색해야 하기 때문에 Map 이 더 빠르다는 것을 알게 되었습니다.
5. 별도의 배열에 정렬한 값들을 넣어놓고, 마지막에 원래의 배열에 옮기는 것이 더 안전합니다.
   - 매번 정렬하자마자 배열 A 에 넣을 때, 정렬한 배열의 길이가 더 작은 경우 0 으로 채우는 로직이 추가로 필요한 데,
   - 차차리 별도의 0 으로 초기화된 배열에 정렬한 배열의 값을 넣고, 마지막에 배열 A 에 옮기는 것이 더 간단합니다.

- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 본 문제의 제한 시간이 0.5 초라고 되어 있어서, 최대한 시간을 사용하는 로직을 피하려다 보니까 계속해서 "런타임 에러(TypeError)" 가 발생했습니다.
- 결국, 조금 시간이 걸리더라도 로직을 간단하게 하는 방향으로 바꾸니 풀 수 있었습니다.
- 사람이 작성하는 것이다보니 로직이 복잡할수록 실수가 나오는 것 같습니다. 차라리 로직을 간단하게 해서 실수가 최대한 나오지 않도록 로직을 작성하는 것이 더 낫겠다 라는 생각을 하게 되었습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const operatorR = (rowLen, colLen, arr) => {
	const copied = Array.from({length: 100}, () => Array(100).fill(0));

	let max = 0;

	// 각 행마다
	for (let i = 0; i < rowLen; i++) {
		const countMap = new Map();

		// 각각의 수가 몇 번 나왔는지 세기
		for (let j = 0; j < colLen; j++) {
			const num = arr[i][j];
			// 0 은 무시
			if (num === 0) continue;

			if (countMap.get(num) === undefined) countMap.set(num, 1);
			else countMap.set(num, countMap.get(num) + 1);
		}

		// [수, 등장 횟수] 원소 형태로 배열로 만들기
		const temp = [];
		for (let [key, value] of countMap) {
			temp.push([key, value]);
		}

		// 만든 배열 정렬하고, 평탄화
		const sorted = temp
			.sort((a, b) => {
				if (a[1] - b[1] === 0) return a[0] - b[0];
				return a[1] - b[1];
			})
			.flat();

		// 100 넘어가는 경우 처리
		const sortedLen = sorted.length > 100 ? 100 : sorted.length;

		// 임시 배열에 넣어두기
		for (let j = 0; j < sortedLen; j++) {
			copied[i][j] = sorted[j];
		}

		max = Math.max(max, sortedLen);
	}

	// 원래 배열에 복사
	for (let i = 0; i < rowLen; i++) {
		for (let j = 0; j < 100; j++) {
			arr[i][j] = copied[i][j];
		}
	}

	return max;
};

const operatorC = (rowLen, colLen, arr) => {
	const copied = Array.from({length: 100}, () => Array(100).fill(0));

	let max = 0;

	for (let j = 0; j < colLen; j++) {
		const countMap = new Map();

		// 각각의 수가 몇 번 나왔는지 세기
		for (let i = 0; i < rowLen; i++) {
			const num = arr[i][j];
			// 0 은 무시
			if (num === 0) continue;

			if (countMap.get(num) === undefined) countMap.set(num, 1);
			else countMap.set(num, countMap.get(num) + 1);
		}

		// [수, 등장 횟수] 원소 형태로 배열로 만들기
		const temp = [];
		for (let [key, value] of countMap) {
			temp.push([key, value]);
		}

		// 만든 배열 정렬하고, 평탄화
		const sorted = temp
			.sort((a, b) => {
				if (a[1] - b[1] === 0) return a[0] - b[0];
				return a[1] - b[1];
			})
			.flat();

		// 100 넘어가는 경우 처리
		const sortedLen = sorted.length > 100 ? 100 : sorted.length;

		// 임시 배열에 넣어두기
		for (let i = 0; i < sortedLen; i++) {
			copied[i][j] = sorted[i];
		}

		max = Math.max(max, sortedLen);
	}

	// 원래 배열에 복사
	for (let j = 0; j < colLen; j++) {
		for (let i = 0; i < 100; i++) {
			arr[i][j] = copied[i][j];
		}
	}

	return max;
};

const solution = (input) => {
	let [r, c, k] = input[0].split(" ").map(Number);
	r -= 1;
	c -= 1;

	const arr = Array.from({length: 100}, () => Array(100).fill(0));
	for (let i = 1; i < 4; i++) {
		const temp = input[i].split(" ").map(Number);
		for (let j = 0; j < 3; j++) {
			arr[i - 1][j] = temp[j];
		}
	}

	// 연산이 필요 없는 경우
	if (arr[r][c] === k) return 0;

	let time = 0;
	let [rowLen, colLen] = [3, 3];
	for (time = 1; time < 101; time++) {
		if (rowLen >= colLen) colLen = operatorR(rowLen, colLen, arr);
		else rowLen = operatorC(rowLen, colLen, arr);

		if (arr[r][c] === k) break;
	}

	if (time > 100) return -1;
	return time;
};

console.log(solution(input));
```
