---
layout: post
title: "BOJ[16935] - 배열 돌리기 3 by JavaScript"
date: 2021-05-06 17:30:00 +0900
categories: BOJ(Implementation)
---

# 배열 돌리기 3

## 문제

- [백준 16935번 - 배열 돌리기 3](https://www.acmicpc.net/problem/16935)

## 언어

- JavaScript

## 순서도

1. 문제에서 주어진 6 가지 연산을 구현
2. 주어진 입력에 따라서 구현한 연산을 수행

## 문제 풀이 step 1

- 문제에서 주어진 연산에 대한 설명을 보고 그대로 구현하면 됩니다.
- **1 번 연산** : 배열을 상하 반전시키기
  - 상하 반전이기 때문에 열의 변화는 없고, 행만 역순으로 바꿔주면 됩니다.
  ```jsx
  for (let i = 0; i < n; i++) {
  	for (let j = 0; j < m; j++) {
  		resArr[n - i - 1][j] = arr[i][j];
  	}
  }
  ```
- **2 번 연산** : 배열을 좌우 반전시키기
  - 좌우 반전이기 때문에 행의 변화는 없고, 열만 역순으로 바꿔주면 됩니다.
  ```jsx
  for (let i = 0; i < n; i++) {
  	for (let j = m - 1; j >= 0; j--) {
  		resArr[i][m - 1 - j] = arr[i][j];
  	}
  }
  ```
- **3 번 연산** : 오른쪽으로 90 도 회전시키기
  - 원본 배열의 맨 왼쪽의 하나의 열이 결과 배열의 맨 상단에 하나의 행으로 변환되는 것처럼 바꿔주면 됩니다.
  - 이 때, 결과 배열의 행의 크기와 열의 크기가 뒤바뀌기 때문에 그에 맞는 연산을 추가해줘야 합니다.
  ```jsx
  for (let j = 0; j < m; j++) {
  	for (let i = n - 1; i >= 0; i--) {
  		resArr[j][n - 1 - i] = arr[i][j];
  	}
  }
  // 행 과 열 크기 바꾸기
  [n, m] = [m, n];
  ```
- **4 번 연산** : 왼쪽으로 90도 회전시키기
  - 3 번 연산과 반대로, 원본 배열의 맨 상단의 하나의 행이 결과 배열의 맨 왼쪽의 하나의 열로 변환되는 것처럼 바꿔주면 됩니다.
  - 이 때도 결과 배열의 행의 크기와 열의 크기가 뒤바뀌기 때문에 그에 맞는 연산을 추가해줘야 합니다.
  ```jsx
  for (let j = 0; j < m; j++) {
  	for (let i = 0; i < n; i++) {
  		resArr[j][i] = arr[i][m - 1 - j];
  	}
  }
  // 행 과 열 크기 바꾸기
  [n, m] = [m, n];
  ```
- **5 번 연산** : 4 개의 부분 배열로 나누고, 시계 방향으로 돌리기
  - 각 부분 배열을 시계 방향으로 돌려주면 됩니다.
  - 좌측 상단을 우측 상단으로 : `arr[i][j] => arr[i][j + m/2]`
  - 우측 상단을 우측 하단으로 : `arr[i][j + m/2] => arr[i + n/2][j + m/2]`
  - 우측 하단을 좌측 하단으로 : `arr[i + n/2][j = m/2] => arr[i + n/2][j]`
  - 좌측 하단을 좌측 상단으로 : `arr[i + n/2][j] => arr[i][j]`
  ```jsx
  for (let i = 0; i < n / 2; i++) {
  	for (let j = 0; j < m / 2; j++) {
  		resArr[i][j + m / 2] = arr[i][j];
  		resArr[i + n / 2][j + m / 2] = arr[i][j + m / 2];
  		resArr[i + n / 2][j] = arr[i + n / 2][j + m / 2];
  		resArr[i][j] = arr[i + n / 2][j];
  	}
  }
  ```
- **6 번 연산** : 4 개의 부분 배열로 나누고, 반시계 방향으로 돌리기
  - 각 부분 배열을 반시계 방향으로 돌려주면 됩니다.
  - 좌측 상단을 좌측 하단으로 : `arr[i][j] => arr[i + n/2][j]`
  - 좌측 하단을 우측 하단으로 : `arr[i + n/2][j] => arr[i + n/2][j + m/2]`
  - 우측 하단을 우측 상단으로 : `arr[i + n/2][j + m/2] => arr[i][j + m/2]`
  - 우측 상단을 좌측 상단으로 : `arr[i][j + m/2] => arr[i][j]`
  ```jsx
  for (let i = 0; i < n / 2; i++) {
  	for (let j = 0; j < m / 2; j++) {
  		resArr[i + n / 2][j] = arr[i][j];
  		resArr[i + n / 2][j + m / 2] = arr[i + n / 2][j];
  		resArr[i][j + m / 2] = arr[i + n / 2][j + m / 2];
  		resArr[i][j] = arr[i][j + m / 2];
  	}
  }
  ```
- 이렇게 만든 연산 함수들을 이용해서 문제에서 주어진대로 연산한 결과를 문자열의 형태로 변환해서 출력하면 정답입니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 입력으로 주어진 연산 번호들을 switch-case 문을 이용해서 분기처리를 했었습니다.
- 하지만 다른 분(d**\*\*** 님)의 풀이를 보고 함수를 배열에 넣으면 더욱 코드가 깔끔해지는 것을 보고 참고했습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const calculate1 = (n, m, arr) => {
	const resArr = Array.from({length: n}, () => []);

	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			resArr[n - i - 1][j] = arr[i][j];
		}
	}

	return resArr;
};

const calculate2 = (n, m, arr) => {
	const resArr = Array.from({length: n}, () => []);

	for (let i = 0; i < n; i++) {
		for (let j = m - 1; j >= 0; j--) {
			resArr[i][m - 1 - j] = arr[i][j];
		}
	}

	return resArr;
};

const calculate3 = (n, m, arr) => {
	const resArr = Array.from({length: m}, () => []);

	for (let j = 0; j < m; j++) {
		for (let i = n - 1; i >= 0; i--) {
			resArr[j][n - 1 - i] = arr[i][j];
		}
	}

	return resArr;
};

const calculate4 = (n, m, arr) => {
	const resArr = Array.from({length: m}, () => []);

	for (let j = 0; j < m; j++) {
		for (let i = 0; i < n; i++) {
			resArr[j][i] = arr[i][m - 1 - j];
		}
	}

	return resArr;
};

const calculate5 = (n, m, arr) => {
	const resArr = Array.from({length: n}, () => []);

	for (let i = 0; i < n / 2; i++) {
		for (let j = 0; j < m / 2; j++) {
			resArr[i][j + m / 2] = arr[i][j];
			resArr[i + n / 2][j + m / 2] = arr[i][j + m / 2];
			resArr[i + n / 2][j] = arr[i + n / 2][j + m / 2];
			resArr[i][j] = arr[i + n / 2][j];
		}
	}

	return resArr;
};

const calculate6 = (n, m, arr) => {
	const resArr = Array.from({length: n}, () => []);

	for (let i = 0; i < n / 2; i++) {
		for (let j = 0; j < m / 2; j++) {
			resArr[i + n / 2][j] = arr[i][j];
			resArr[i + n / 2][j + m / 2] = arr[i + n / 2][j];
			resArr[i][j + m / 2] = arr[i + n / 2][j + m / 2];
			resArr[i][j] = arr[i][j + m / 2];
		}
	}

	return resArr;
};

const solution = (input) => {
	let [n, m, r] = input[0].split(" ").map(Number);
	let arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));
	const rArr = input[n + 1].split(" ").map(Number);

	const calculator = [
		null,
		calculate1,
		calculate2,
		calculate3,
		calculate4,
		calculate5,
		calculate6,
	];

	for (let i = 0; i < r; i++) {
		arr = calculator[rArr[i]](n, m, arr);
		if (rArr[i] === 3 || rArr[i] === 4) [n, m] = [m, n];
	}

	return arr.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```
