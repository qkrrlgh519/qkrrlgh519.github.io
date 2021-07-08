---
layout: post
title: "BOJ[18290] - NM과 K (1) by JavaScript"
date: 2021-07-08 13:30:00 +0900
categories: BOJ(BackTracking)
---

# NM과 K (1)

## 문제

- [백준 18290번 - NM과 K (1)](https://www.acmicpc.net/problem/18290)

## 언어

- JavaScript

## 순서도

1. 재귀 함수를 이용해서 N x M 크기의 격자에서 K 개의 칸을 고르는 모든 경우 만들기
2. 각 경우의 칸에 들어있는 수를 모두 더한 값을 구하기
3. 2 번 과정 중에서 더한 값 중에서 가장 큰 값을 출력하기

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건을 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- [백준 15649번 - N과 M (1) 풀이](<https://qkrrlgh519.github.io/boj(backtracking)/2021/03/18/BOJ-BackTracking-15649.html>) 시리즈 와 유사한 문제입니다.
- 다만, 이번에는 1 차원 배열이 아닌 2 차원 배열에서 선택을 해야 하는 문제입니다.
- 본 문제는 N x M 크기의 2 차원 배열에서 칸 K 개를 선택하고, 선택한 칸에 들어있는 수를 모두 더한 값의 최댓값을 구하는 문제입니다.
  - 단, 선택한 두 칸이 인접하면 안됩니다.
  - N, M 은 1 이상 10 이하의 수입니다.
  - K 는 최대 4 이하의 수입니다.

## 문제 풀이 step 2

- 1 차원 배열이 아닌 2 차원 배열에서 선택해야 한다는 점 말고는, 기존의 N과 M 시리즈의 문제들과 큰 차이점은 없습니다.
- 재귀 함수를 이용해서 2 차원 배열에서 K 개의 칸을 선택하는 모든 경우를 만들어봅니다.
- 그리고 각 경우 중에서 칸에 들어있는 수를 모두 더한 값이 가장 큰 경우, 그 값을 출력하면 정답이 됩니다.
- 추가적으로 [백준 15650번 - N과 M (2) 풀이](<https://qkrrlgh519.github.io/boj(backtracking)/2021/03/18/BOJ-BackTracking-15650.html>) 의 오름차순 아이디어를 적용해서, 이전에 탐색했던 행이나 열은 다시 탐색하지 않음으로써 반복의 횟수를 더욱 줄일 수 있습니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 이전에 탐색했던 행이나 열을 다시 탐색하기 때문에 시간이 오래걸린다는 것을 알았지만, 그 문제를 어떻게 해결해야 할지는 떠올리지 못했습니다.
- 그 해결법은 제가 예전에 풀었던 N과 M (2) 문제에서의 before 변수를 이용해 오름차순으로 탐색을 해서 이전에 탐색한 행이나 열을 다시 탐색하지 않는 방법이었습니다.
- 1 차원에서 2 차원으로 늘어났지만, 이 문제의 본질은 똑같다는 것을 깨닫는 것이 중요할 것 같습니다.
- 그리고 삼항 연산자를 이용해서 행이 바뀌었을 때는 열은 0 부터 시작하고, 행이 바뀌지 않았다면 이전 열부터 시작하게 하는 방법이 중요했던 것 같습니다.

## 소스 코드 1

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 칸에 들어있는 수의 최솟값이 -10000 이며, 최대 4 칸을 선택 가능하므로, -40000 으로 초기화
let max = -40000;

// 방문 처리할 칸이 격자를 넘어가는지 검사하는 함수
const isPossibleRoute = (n, m, nx, ny) =>
	0 <= nx && nx < n && 0 <= ny && ny < m;

// 인접한 칸을 선택할 수 없으니
// 선택한 칸과 상하좌우까지 방문처리 하는 함수
const visit = (n, m, x, y, visited, plus) => {
	const cx = [0, 0, 1, -1];
	const cy = [1, -1, 0, 0];

	// 선택한 칸 방문처리
	visited[x][y] += plus;
	// 상하좌우 방문처리
	for (let i = 0; i < 4; i++) {
		const [nx, ny] = [x + cx[i], y + cy[i]];
		if (!isPossibleRoute(n, m, nx, ny)) continue;

		visited[nx][ny] += plus;
	}
};

// N x M 크기의 격자에서 K 개의 칸을 선택하는 모든 경우 만들어보기
const rec = (n, m, k, grid, visited, startX, startY, sum, depth) => {
	if (depth === k) {
		// 최댓값 갱신
		max = Math.max(sum, max);
		return;
	}

	// 이전에 검사했던 행을 다시 검사하지 않기 위해서 startX 변수를 이용
	for (let i = startX; i < n; i++) {
		// 이전에 검사했던 열을 다시 검사하지 않기 위해서 startY 변수를 이용
		// 이 때, 삼항연산자를 이용해서 행이 바뀌지 않았다면, starY 부터 시작하고,
		// 행이 바뀌었다면, 0 부터 시작
		for (let j = i === startX ? startY : 0; j < m; j++) {
			if (visited[i][j] !== 0) continue;

			visit(n, m, i, j, visited, 1);
			rec(n, m, k, grid, visited, i, j, sum + grid[i][j], depth + 1);

			visit(n, m, i, j, visited, -1);
		}
	}
};

const solution = (input) => {
	const [n, m, k] = input[0].split(" ").map(Number);
	const grid = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	// 방문 처리를 할 배열 생성
	const visited = Array.from({length: n}, () => Array(m).fill(0));
	// N x M 크기의 격자에서 K 개의 칸을 고르는 모든 경우 만들어보기
	rec(n, m, k, grid, visited, 0, 0, 0, 0);

	return max;
};

console.log(solution(input));
```
