---
layout: post
title: "BOJ[15683] - 감시 by JavaScript"
date: 2021-06-30 13:00:00 +0900
categories: BOJ(Implementation)
---

# 감시

## 문제

- [백준 15683번 - 감시](https://www.acmicpc.net/problem/15683)

## 언어

- JavaScript

## 순서도

1. CCTV 에 관한 정보를 수집 (각 CCTV 의 종류와 위치 및 모든 CCTV 의 개수)
2. 5 번 타입의 CCTV 는 감시하는 방향의 경우가 한 가지이므로 바로 사무실에 감시 영역 적용
3. 각각의 CCTV 로 감시할 수 있는 모든 경우를 확인하며, 각 경우마다 사무실에 감시 영역 적용하고, 사각 지대의 개수 세기
4. 각각의 사각 지대의 개수 중에서 최솟값을 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 사무실과 CCTV 에 대한 정보는 다음과 같습니다.
  - 사무실에서 0 은 빈 칸, 6 은 벽, 1 ~ 5는 CCTV 를 나타냅니다.
  - CCTV 는 회전시킬 수 있는데, 회전은 항상 90 도 방향으로 해야하며, 감시하려고 하는 방향이 가로 또는 세로 방향이어야 합니다.
  - CCTV 는 감시할 수 있는 방향에 있는 칸 전체를 감시할 수 있습니다. 단, 벽을 통과할 수는 없습니다.
  - CCTV 가 감시할 수 없는 영역은 사각 지대라고 합니다.
  - 1 번 CCTV 는 한 방향만 감시할 수 있으며, `[상, 하, 좌, 우]` 이렇게 4 개의 경우가 있습니다.
  - 2 번 CCTV 는 두 방향을 감시할 수 있으며, `[상하, 좌우]` 이렇게 2 개의 경우가 있습니다.
  - 3 번 CCTV 는 두 방향을 감시할 수 있으며, `[상우, 우하, 하좌, 좌상]` 이렇게 4 개의 경우가 있습니다.
  - 4 번 CCTV 는 세 방향을 감시할 수 있으며, `[좌상우, 상우하, 우하좌, 하좌상]` 이렇게 4 개의 경우가 있습니다.
  - 5 번 CCTV 는 네 방향을 감시할 수 있으며, `[상하좌우]` 이렇게 1 개의 경우가 있습니다.
- 위의 CCTV 의 성질과 브루트 포스 방식으로 푸는 문제입니다.

## 문제 풀이 step 2

- 우선 CCTV 에 관한 정보를 수집합니다.
  - cctvType : 각 CCTV 의 종류 (1 ~ 4)
  - cctvPos : 각 CCTV 의 위치 (좌표)
  - cctvCnt : 모든 CCTV 의 총 개수
- 정보를 수집함과 동시에 5 번 타입의 CCTV 는 바로 적용합니다.
  - 왜냐하면, 5 번 타입의 CCTV 는 다른 타입의 CCTV 와 다르게 감시하는 방향의 경우가 한 가지이기 때문입니다.
- 수집한 CCTV 로 만들 수 있는 모든 경우의 수를 탐색하며 각 경우마다 사각지대의 개수를 구합니다.
  - 예를 들여, 사무실에 1 번 타입의 CCTV 와 2 번 타입의 CCTV 이렇게 총 2 개 있을 때
  - 만들 수 있는 경우의 수는 8 가지 입니다.
    - 1 번 (상) x 2 번 (상하)
    - 1 번 (우) x 2 번 (상하)
    - 1 번 (하) x 2 번 (상하)
    - 1 번 (좌) x 2 번 (상하)
    - 1 번 (상) x 2 번 (좌우)
    - 1 번 (우) x 2 번 (좌우)
    - 1 번 (하) x 2 번 (좌우)
    - 1 번 (좌) x 2 번 (좌우)
  - 각 경우마다 사무실에서 CCTV 가 감시하는 영역을 표시하고, 사각 지대의 개수를 구합니다.
- 위 과정을 통해서 구한 각 경우의 사각지대의 개수 중에서 최솟값을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 순서도는 조금 간결하게 나왔는데, 로직 안의 내용은 상당히 복잡하게 짠 느낌입니다.
- 작은 함수로 분리하는 과정이 깔끔하지 않은 느낌입니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 사각지대의 최솟값 선언
let min = Number.MAX_SAFE_INTEGER;

// 사무실을 깊은 복사하는 함수
const copyOffice = (office) => {
	const copied = [];
	for (let i = 0; i < office.length; i++) {
		copied[i] = [...office[i]];
	}

	return copied;
};

// 사무실에 CCTV 가 감시하는 영역을 표시하는 함수
const watch = (n, m, copiedOffice, x, y, cases) => {
	const cx = [-1, 0, 1, 0];
	const cy = [0, 1, 0, -1];

	// x 와 y 가 사무실을 벗어나지 않으며, 벽을 만나면 종료
	// 감시하는 영역은 7 로 표시
	while (0 <= x && x < n && 0 <= y && y < m) {
		if (copiedOffice[x][y] === 6) break;
		if (copiedOffice[x][y] === 0) copiedOffice[x][y] = 7;
		[x, y] = [x + cx[cases], y + cy[cases]];
	}
};

// CCTV 의 타입에 맞게 사무실에 CCTV 가 감시하는 영역을 표시하는 함수
const watchCamera = (n, m, copiedOffice, cctvType, cctvPos, cases) => {
	let [x, y] = cctvPos;

	// 현재 CCTV 의 위치 표시
	copiedOffice[x][y] = 7;

	// 각 CCTV 의 타입에 맞게 표시하는 함수 호출
	if (cctvType === 1) {
		watch(n, m, copiedOffice, x, y, cases);
	} else if (cctvType === 2) {
		watch(n, m, copiedOffice, x, y, cases);
		watch(n, m, copiedOffice, x, y, cases + 2);
	} else if (cctvType === 3) {
		watch(n, m, copiedOffice, x, y, cases);
		watch(n, m, copiedOffice, x, y, (cases + 1) % 4);
	} else {
		watch(n, m, copiedOffice, x, y, cases);
		watch(n, m, copiedOffice, x, y, (cases + 1) % 4);
		watch(n, m, copiedOffice, x, y, (cases + 3) % 4);
	}
};

// 사각지대의 개수 세기
const countBlindSpots = (n, m, office, cctvType, cctvPos, cctvCnt, cases) => {
	const copiedOffice = copyOffice(office);

	// 재귀 함수를 통해서 나온 경우로 사무실에 CCTV 가 감시하는 영역 표시
	for (let i = 0; i < cctvCnt; i++) {
		watchCamera(n, m, copiedOffice, cctvType[i], cctvPos[i], cases[i]);
	}

	// 사무실에서 사각지대의 개수 세기
	let cnt = 0;
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			if (copiedOffice[i][j] === 0) cnt += 1;
		}
	}

	return cnt;
};

// 모든 경우의 수를 탐색하는 함수
const rec = (n, m, office, cctvType, cctvPos, cctvCnt, cases, depth) => {
	if (depth === cctvCnt) {
		// 사각지대의 개수의 최솟값 갱신
		min = Math.min(
			min,
			countBlindSpots(n, m, office, cctvType, cctvPos, cctvCnt, cases)
		);
		return;
	}

	for (let i = 0; i < 4; i++) {
		// 2 번 타입의 CCTV 는 [상하, 좌우] 두 경우밖에 없으므로, 추가 조건 설정
		if (cctvType[depth] === 2 && i > 1) continue;
		cases[depth] = i;
		rec(n, m, office, cctvType, cctvPos, cctvCnt, cases, depth + 1);
	}
};

// 사무실에서 5 번 타입의 CCTV 가 감시하는 영역을 표시하는 함수
const watchCameraType5 = (n, m, office, i, j) => {
	office[i][j] = 7;

	// 상
	for (let k = i; k >= 0; k--) {
		if (office[k][j] === 6) break;
		if (office[k][j] === 0) office[k][j] = 7;
	}
	// 하
	for (let k = i; k < n; k++) {
		if (office[k][j] === 6) break;
		if (office[k][j] === 0) office[k][j] = 7;
	}
	// 좌
	for (let k = j; k >= 0; k--) {
		if (office[i][k] === 6) break;
		if (office[i][k] === 0) office[i][k] = 7;
	}
	// 우
	for (let k = j; k < m; k++) {
		if (office[i][k] === 6) break;
		if (office[i][k] === 0) office[i][k] = 7;
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const office = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	// cctv 정보 수집
	const cctvType = [];
	const cctvPos = [];
	let cctvCnt = 0;
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			if (0 < office[i][j] && office[i][j] < 5) {
				cctvType.push(office[i][j]);
				cctvPos.push([i, j]);
				cctvCnt += 1;
			}

			// 5 번 타입의 CCTV 는 [상하좌우] 한 경우밖에 없으므로 바로 사무실에 감시 영역 표시
			if (office[i][j] === 5) {
				watchCameraType5(n, m, office, i, j);
			}
		}
	}

	// 모든 경우의 수를 확인해보고 최솟값 구하기
	rec(n, m, office, cctvType, cctvPos, cctvCnt, [], 0);

	// 사각지대의 최솟값 출력
	return min;
};

console.log(solution(input));
```
