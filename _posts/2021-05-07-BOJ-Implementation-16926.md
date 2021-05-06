---
layout: post
title: "BOJ[16926] - 배열 돌리기 1 by JavaScript"
date: 2021-05-07 01:30:00 +0900
categories: BOJ(Implementation)
---

# 배열 돌리기 1

## 문제

- [백준 16926번 - 배열 돌리기 1](https://www.acmicpc.net/problem/16926)

## 언어

- JavaScript

## 순서도

1. 배열에서 회전하는 원소들끼리 그룹만들기
2. 그룹끼리 r 번 회전하기
3. 회전한 원소들 다시 배열에 넣어주기
4. 배열의 테두리부터 안에까지 들어가면서 1, 2, 3 번 과정 반복하기

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- 문제에서 주어진 사진을 보면, 회전시켜야할 원소들이 마치 나무의 나이테처럼 배열의 테두리부터 한 칸씩 들어가면서 그룹을 맺고 있습니다.
- 그리고 회전도 그룹에 속한 원소들끼리만 진행됩니다.
- 따라서 배열에서 겉에서부터 한 칸씩 들어가면서 원소들을 그룹화하고, 회전하고, 다시 배열에 복사하면됩니다.

## 문제 풀이 step 2

- 그렇다면 그룹화를 어떻게 해야할까요??
- 저는 다음과 같이 그룹화를 했습니다.
  ![백준 16926번 문제 배열 사진](/public/img/BOJ-Implementation/BOJ-19926-1.JPG)
  - 좌상단, 우상단, 우하단, 좌하단 : `[1][1], [1][5], [4][5], [4][1]`
  - 좌상단에서 우상단까지의 원소 : `A[1][1], A[1][2], A[1][3], A[1][4]`
  - 우상단에서 우하단까지의 원소 : `A[1][5], A[2][5], A[3][5]`
  - 우하단에서 좌하단까지의 원소 : `A[4][5], A[4][4], A[4][3], A[4][2]`
  - 좌하단에서 좌상단까지의 원소 : `A[4][1], A[3][1], A[2][1]`
- 즉, 시계방향으로 돌아가면서 그룹화를 진행했습니다.
- Queue 클래스를 하나 만들고, Queue 를 이용했습니다. Queue 클래스 대신에 빈 배열로 해도 상관없습니다. (push, shift 함수)

## 문제 풀이 step 3

- 그룹화를 했으니 이제 회전을 진행합니다.
- Queue 에 넣어줬으니 r 번 만큼 Queue 의 앞의 원소를 빼서 Queue 의 뒤에 넣어주면 되겠습니다.
- 이 때, r 이 그룹의 원소의 양보다 크다면 의미없는 회전을 하게 됩니다.
- 그래서 MOD 연산을 통해서 r 을 그룹의 원소로 나눈 나머지 만큼만 회전하면 됩니다.
  ```jsx
  let count = r % queue.size();
  ```

## 문제 풀이 step 4

- 회전이 끝나면 다시 배열에 복사해주면 됩니다.
- 복사할 때는 "문제 풀이 step 2"에 작성한 순서대로 다시 넣어주면 됩니다.

## 문제 풀이 step 5

- 그룹화하고, 회전하고, 다시 배열에 복사하는 작업을 배열의 크기를 줄여가며 진행해야 합니다.
- 배열의 크기는 어떻게 줄일까요??
  ![백준 16926번 문제 배열 사진](/public/img/BOJ-Implementation/BOJ-19926-1.JPG)
  - 저는 좌상단, 우상단, 우하단, 좌하단에 집중했습니다.
  - 첫 그룹의 좌상단, 우상단, 우하단, 좌하단은 다음과 같습니다.
    - `[1][1], [1][5], [4][5], [4][1]`
  - 두번째 그룹의 좌상단, 우상단, 우하단, 좌하단은 다음과 같습니다.
    - `[2][2], [2][4], [3][2], [3][4]`
  - 이를 일반화 하면, k번째 그룹의 좌상단, 우상단, 우하단, 좌하단은 다음과 같습니다.
    - `[1 + k][1 + k], [1 + k][5 - k], [4 - k][5 - k], [4 - k][1 + k]`
- 이런식으로 줄이다 보면 좌상단과 좌하단 혹은 좌상단과 우상단의 index 가 같아지는 상황이 옵니다.
- 그 상황이 오면 회전을 멈추고, 배열을 문자열화해서 출력하면 정답이 됩니다.

## 후기

- 처음에는 그룹화하지 않고, 한 번씩 회전하는 방식으로 문제를 해결했었습니다. 하지만 상당히 오래걸렸습니다.
- 시간 차이는 다음 사진과 같습니다. (약 4.5 배 정도 차이 발생)
  ![백준 16926번 문제 풀이에 따른 경과 시간 차이](/public/img/BOJ-Implementation/BOJ-19926-2.JPG)
- 그리고 제가 한 방식은 queue를 이용해서, 실제로 queue 에서 삽입과 삭제를 통해서 회전을 구현했습니다.
- 하지만 이 부분도 더 개선할 점이 있습니다.
- queue 말고, 배열을 이용해서 그룹화를 하고, 실제로 회전하지 않고, 회전량 만큼 index 를 밀어주는 방식입니다.
- 그 방식은 아래에 작성하였습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

class Queue {
	constructor() {
		this.bucket = [];
		this.front = -1;
		this.rear = -1;
	}

	isEmpty() {
		return this.front === this.rear;
	}

	enqueue(data) {
		this.bucket[++this.rear] = data;
	}

	dequeue() {
		return this.bucket[++this.front];
	}

	size() {
		return this.rear - this.front;
	}
}

const rotate = (k, l, n, m, r, arr, resArr) => {
	const queue = new Queue();

	// 좌상단, 좌하단, 우상단, 우하단
	let [ltx, lty] = [k, l];
	let [lbx, lby] = [n - 1, l];
	let [rtx, rty] = [k, m - 1];
	let [rbx, rby] = [n - 1, m - 1];

	// 그룹화
	for (let i = lty; i < rty; i++) {
		queue.enqueue(arr[ltx][i]);
	}
	for (let i = rtx; i < rbx; i++) {
		queue.enqueue(arr[i][rty]);
	}
	for (let i = rby; i > lby; i--) {
		queue.enqueue(arr[rbx][i]);
	}
	for (let i = lbx; i > ltx; i--) {
		queue.enqueue(arr[i][lby]);
	}

	// 불필요한 회전수 제거
	let count = r % queue.size();

	// 회전
	while (count-- > 0) {
		queue.enqueue(queue.dequeue());
	}

	// 그룹을 다시 배열에 복사
	for (let i = lty; i < rty; i++) {
		arr[ltx][i] = queue.dequeue();
	}
	for (let i = rtx; i < rbx; i++) {
		arr[i][rty] = queue.dequeue();
	}
	for (let i = rby; i > lby; i--) {
		arr[rbx][i] = queue.dequeue();
	}
	for (let i = lbx; i > ltx; i--) {
		arr[i][lby] = queue.dequeue();
	}
};

const solution = (input) => {
	let [n, m, r] = input[0].split(" ").map(Number);
	let arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	let [k, l] = [0, 0];
	const resArr = Array.from({length: n}, () => []);
	while (n - k !== 0 && m - l !== 0) {
		rotate(k, l, n, m, r, arr, resArr);
		[k, l, n, m] = [k + 1, l + 1, n - 1, m - 1];
	}

	return arr.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```

---

## 개선한 방식의 문제 풀이

- Queue 를 이용하지 않고, 배열을 이용해서 실제로 회전하지 않고, 간단하게 index 를 밀어주는 방식으로 구현해보았습니다.
- 바뀌는 부분은 rotate 함수의 내부만 조금 바뀝니다.
- 풀어본 결과 속도 차이는 별로 나지 않지만, 코드의 길이가 상당히 줄어든 것을 확인할 수 있습니다.

## 개선한 방식의 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const rotate = (k, l, n, m, r, arr, resArr) => {
	const group = [];

	let [ltx, lty] = [k, l];
	let [lbx, lby] = [n - 1, l];
	let [rtx, rty] = [k, m - 1];
	let [rbx, rby] = [n - 1, m - 1];

	let idx = 0;
	for (let i = lty; i < rty; i++) {
		group[idx++] = arr[ltx][i];
	}
	for (let i = rtx; i < rbx; i++) {
		group[idx++] = arr[i][rty];
	}
	for (let i = rby; i > lby; i--) {
		group[idx++] = arr[rbx][i];
	}
	for (let i = lbx; i > ltx; i--) {
		group[idx++] = arr[i][lby];
	}

	let len = group.length;
	idx = r % len;

	for (let i = lty; i < rty; i++) {
		arr[ltx][i] = group[idx % len];
		idx++;
	}
	for (let i = rtx; i < rbx; i++) {
		arr[i][rty] = group[idx % len];
		idx++;
	}
	for (let i = rby; i > lby; i--) {
		arr[rbx][i] = group[idx % len];
		idx++;
	}
	for (let i = lbx; i > ltx; i--) {
		arr[i][lby] = group[idx % len];
		idx++;
	}
};

const solution = (input) => {
	let [n, m, r] = input[0].split(" ").map(Number);
	let arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	let [k, l] = [0, 0];
	const resArr = Array.from({length: n}, () => []);
	while (n - k !== 0 && m - l !== 0) {
		rotate(k, l, n, m, r, arr, resArr);
		[k, l, n, m] = [k + 1, l + 1, n - 1, m - 1];
	}

	return arr.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```
