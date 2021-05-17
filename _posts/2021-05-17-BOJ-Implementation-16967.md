---
layout: post
title: "BOJ[16967] - 배열 복원하기 by JavaScript"
date: 2021-05-17 12:00:00 +0900
categories: BOJ(Implementation)
---

# 배열 복원하기

## 문제

- [백준 16967번 - 배열 복원하기](https://www.acmicpc.net/problem/16967)

## 언어

- JavaScript

## 순서도

1. 아래의 등식을 이용해서 겹쳐진 부분에 대한 값 되찾기
   - `B[i][j] = A[i][j] + A[i - X][j - Y]`
2. 되찾은 값과 배열 B 를 이용해서 배열 A 만들기
3. 출력 양식에 맞게 문자열로 변환해서 출력하기

## 문제 풀이 step 1

- 문제의 결론은 "주어진 배열 B 를 가지고 기존의 배열 A 를 찾아라" 입니다.
- 배열 B 의 (i, j) 에 들어있는 값은 다음 3 개와 같다고 합니다.
  - (i, j)가 두 배열 모두에 포함되지 않으면, `B[i][j] = 0` 이다.
    - **기존의 배열 A** 와 **아래로 X 칸, 오른쪽으로 Y 칸 이동시킨 배열 A** 중 두 곳에 포함되지 않는 부분에 대한 내용입니다.
  - (i, j)가 두 배열 모두에 포함되면, `B[i][j] = A[i][j] + A[i - X][j - Y]` 이다.
    - **기존의 배열 A** 와 **아래로 X 칸, 오른쪽으로 Y 칸 이동시킨 배열 A** 가 겹쳐진 부분에 대한 내용입니다.
    - 따라서 이 곳에 대한 값을 구하면 기존의 배열 A 를 찾을 수 있습니다.
  - (i, j)가 두 배열 중 하나에 포함되면, `B[i][j] = A[i][j]` 또는 `A[i - X][j - Y]` 이다.
    - **기존의 배열 A** 와 **아래로 X 칸, 오른쪽으로 Y 칸 이동시킨 배열 A** 중 한 곳에만 포함되는 부분에 대한 내용입니다.
    - 따라서 기존의 A 의 값에 변화없이 복사되는 부분에 대한 내용입니다.

## 문제 풀이 step 2

- 따라서 두 번재 내용을 가지고 겹쳐진 부분에 대한 값을 되찾으면 기존의 배열 A 를 찾을 수 있습니다.
- 이는 등식을 이용하면 됩니다. 주어진 등식은 다음과 같은데요.
  - `B[i][j] = A[i][j] + A[i - X][j - Y]`
- 좌변과 우변을 바꿔보면 기존의 A 의 원소에 대한 등식이 나오게 됩니다.
  - `A[i][j] = B[i][j] - A[i - X][j - Y]`
- 이 등식을 이용해서 겹쳐진 부분에 대한 값을 되찾고, 나머지 부분을 복사하면 기존의 배열 A 를 찾을 수 있습니다.
- 그리고 배열 A 를 문자열 형태로 변환해서 출력하면 정답입니다.
- 추가 설명은 주석에 작성하겠습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [h, w, x, y] = input[0].split(" ").map(Number);
	const bArr = input.slice(1).map((v) => v.split(" ").map(Number));

	for (let i = x; i < h; i++) {
		for (let j = y; j < w; j++) {
			// 등식을 이용해서 기존의 배열 A 의 원소 값 되찾기
			bArr[i][j] = bArr[i][j] - bArr[i - x][j - y];
		}
	}

	// 되찾은 값과 나머지 부분을 복사해서 기존의 배열 A 만들기
	const aArr = [];
	for (let i = 0; i < h; i++) {
		aArr[i] = [];
		for (let j = 0; j < w; j++) {
			aArr[i][j] = bArr[i][j];
		}
	}

	// 문자열 형태로 변환해서 출력하기
	return aArr.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```
