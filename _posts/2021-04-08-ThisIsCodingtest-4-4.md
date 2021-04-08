---
layout: post
title: "이것이 코딩테스트다 - [Implementation] 게임 개발 by JavaScript"
date: 2021-04-08 15:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 게임 개발

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 문제에서 주어진 대로 구현하는 구현 유형의 문제입니다.
- 해결을 위해 특별한 무언가를 생각해야하는 것이 아닌 문제에서 주이진 매뉴얼 3 개를 오류없이 구현해내면 됩니다.
- 전형적인 시뮬레이션 유형의 문제라고 저자님께서 말씀을하시는데, 문제가 어렵다기 보다는 문제를 이해하고 그 내용을 소스코드로 옮기는 그 과정이 어려운 것 같습니다.

## 후기

- 다시 풀어볼 문제입니다.
- 문제를 이해하는 것도 연습이 필요하고, 이해한 내용을 소스코드로 옮기는 것도 연습이 필요할 것 같습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `4 4
// 1 1 0
// 1 1 1 1
// 1 0 0 1
// 1 1 0 1
// 1 1 1 1`.split("\n");

const cx = [-1, 0, 1, 0];
const cy = [0, 1, 0, -1];

const isPossibleRoute = (n, m, x, y) => 0 <= x && x < n && 0 <= y && y < m;

const nextPosition = (x, y, d) => [x + cx[d], y + cy[d]];

const checkFourSide = (n, m, arr, visited, x, y, d) => {
	for (let i = 0; i < 4; i++) {
		const [nx, ny] = [x + cx[i], y + cy[i]];
		if (!isPossibleRoute(n, m, nx, ny)) continue; // 불가능한 경로
		if (arr[nx][ny] === 1 || visited[nx][ny]) continue; // 바다거나 방문했거나

		// 4 방향 중에 갈 곳이 있다.
		return false;
	}

	// 4 방향 중에 갈 곳이 없다.
	return true;
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	let [x, y, d] = input[1].split(" ").map(Number);
	const arr = [];
	for (let i = 2; i < 2 + n; i++) {
		arr[i - 2] = input[i].split(" ").map(Number);
	}

	let ans = 0;

	const visited = Array.from({length: n}, () => Array(m).fill(false));
	visited[x][y] = true;
	ans += 1;

	while (true) {
		// 왼쪽으로 회전
		d -= 1;
		if (d < 0) d += 4;

		let [nx, ny] = nextPosition(x, y, d);
		if (
			isPossibleRoute(n, m, nx, ny) &&
			!visited[nx][ny] &&
			arr[nx][ny] === 0
		) {
			[x, y] = [nx, ny];
			visited[x][y] = true;
			ans += 1;
		}

		if (checkFourSide(n, m, arr, visited, x, y, d)) {
			// 뒤로 이동
			let nd = d + 2;
			if (nd > 3) nd -= 4;

			[nx, ny] = nextPosition(x, y, nd);
			// 뒤에가 바다인 경우
			if (isPossibleRoute(n, m, nx, ny) && arr[nx][ny] === 1) break;
			else if (isPossibleRoute(n, m, nx, ny) && visited[nx][ny])
				[x, y] = [nx, ny];
		}
	}

	return ans;
};

console.log(solution(input));
```

---

## 다른 방식의 문제 풀이 step 1

- 위의 소스 코드는 제 방식대로 풀어봤는데 조금 가독성이 떨어진다는 느낌을 받았습니다.
- 반면에 나동빈 저자님께서 올려주신 소스코드가 더 깔끔하고 직관적으로 느껴져서 JS 코드로 옮겨봤습니다.
- 단, 저자님께서는 맵을 벗어나는 경우는 고려하지 않으셨습니다. 그래저 저도 그 부분은 제외했습니다.
  - 위의 코드에는 작성되어 있습니다.
  - `isPossibleRoute()`

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `4 4
// 1 1 0
// 1 1 1 1
// 1 0 0 1
// 1 1 0 1
// 1 1 1 1`.split("\n");

// 북, 동, 남, 서 방향 정의
const cx = [-1, 0, 1, 0];
const cy = [0, 1, 0, -1];

// 왼쪽으로 회전하는 함수
const turnLeft = (d) => (d - 1 === -1 ? 3 : d - 1);

const solution = (input) => {
	// n 과 m 을 공백으로 구분하여 입력받기
	const [n, m] = input[0].split(" ").map(Number);

	let [x, y, d] = input[1].split(" ").map(Number);
	// 방문한 위치를 저장하기 위한 맵을 생성하여 0 으로 초기화
	const visited = Array.from({length: n}, () => Array(m).fill(0));
	visited[x][y] = 1; // 현재 좌표 방문 처리

	// 전체 맵 정보를 입력하기
	const arr = [];
	for (let i = 2; i < 2 + n; i++) {
		arr[i - 2] = input[i].split(" ").map(Number);
	}

	// 시뮬레이션 시작
	let count = 1;
	let turnTime = 0;
	while (true) {
		// 왼쪽으로 회전
		d = turnLeft(d);
		let [nx, ny] = [x + cx[d], y + cy[d]];

		// 회전한 이후 정면에 가보지 않은 칸이 존재하는 경우 이동
		if (visited[nx][ny] === 0 && arr[nx][ny] === 0) {
			[x, y] = [nx, ny];
			visited[x][y] = 1;
			turnTime = 0;
			count += 1;
			continue;
		} else {
			// 회전한 이후 정면에 가보지 않은 칸이 없거나 바다인 경우
			turnTime += 1;
		}

		// 네 방향 모두 갈 수 없는 경우
		if (turnTime === 4) {
			[nx, ny] = [x - cx[d], y - cy[d]];

			if (arr[nx][ny] === 0) [x, y] = [nx, ny];
			else break;

			turnTime = 0;
		}
	}

	return count;
};

console.log(solution(input));
```
