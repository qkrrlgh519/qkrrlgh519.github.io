---
layout: post
title: "BOJ[14891] - 톱니바퀴 by JavaScript"
date: 2021-05-08 12:30:00 +0900
categories: BOJ(Implementation)
---

# 톱니바퀴

## 문제

- [백준 14891번 - 톱니바퀴](https://www.acmicpc.net/problem/14891)

## 언어

- JavaScript

## 순서도

1. 입력과 배열을 이용해서 톱니바퀴 정보 표현
2. 톱니바퀴를 회전하는 함수 구현
   1. 각 톱니바퀴의 회전 여부와 회전 방향 정보 수집
   2. 정보 수집은 매개변수로 주어진 톱니바퀴에서 4 번 톱니바퀴까지 그리고 매개변수로 주어진 톱니바퀴에서 1 번 톱니바퀴까지 진행
   3. 수집한 정보에 따라서 톱니바퀴 회전
3. 주어진 회전 방법에 따라서 톱니 회전
4. 1 ~ 4 번 톱니바퀴의 12 시 방향 톱니의 극이 무엇인지에 따라서 점수 계산 후 출력

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- 중요한 것은 **톱니바퀴 정보를 어떻게 저장할지, 어떻게 회전할지** 입니다.
- 우선 톱니바퀴가 총 4 개이기 때문에 **2 차원 배열**을 이용해서 각각의 톱니바퀴 정보를 저장했습니다.
  - 예를 들어, 1 번 톱니바퀴가 정보가 `10101111` 이라고 주어졌을 때,
  - 배열의 0 번부터 시계방향 순서대로 저장합니다.
  - `gears[1] = [1, 0, 1, 0, 1, 1, 1, 1]` 과 같은 결과를 얻을 수 있습니다.
  - 시계 방향으로 회전한다면 `gears[1] = [1, 1, 0, 1, 0, 1, 1, 1]` 이 될 것이고, 12 시 방향의 톱니는 1 입니다.
  - 반시계 방향으로 회전한다면 `gears[1] = [0, 1, 0, 1, 1, 1, 1, 1]` 이 될 것이고, 12 시 방향의 톱니는 0 입니다.

## 문제 풀이 step 2

- 톱니바퀴 정보를 저장했으면 이제 **회전하는 함수를 구현**해야 합니다.
- 회전하는 함수는 다음과 같은 순서로 로직이 진행됩니다.
  - 우선 각각의 톱니바퀴의 **회전 여부**와 **회전 방향 정보**를 **수집**할 것입니다.
    - 매개변수로 주어지는 톱니바퀴 번호를 기준으로 **오른쪽으로 한 개씩 톱니바퀴를 검사**하며, 각각의 톱니바퀴에 대한 정보를 수집합니다.
    - 그리고 매개변수로 주어지는 톱니바퀴 번호를 기준으로 **왼쪽으로 한 개씩 톱니바퀴를 검사**하며, 각각의 톱니바퀴에 대한 정보를 수집합니다.
  - 정보를 수집했으면, 수집한 정보에 따라서 각각의 톱니바퀴를 **회전**합니다.

## 문제 풀이 step 3

- **회전 여부**와 **회전 방향 정보**를 수집하는 과정부터 알아보겠습니다.
- 먼저 오른쪽으로 한 개씩 검사하는 과정은,

  - 예를 들어, 매개변수로 주어진 톱니바퀴가 2 번이고, 시계 방향 (1) 으로 회전한다고 가정하겠습니다.
  - 이 때, **2 번 톱니바퀴와 3 번 톱니바퀴의 맞물린 톱니**를 확인해야 합니다.
  - 2 번 톱니바퀴는 3 시 방향의 톱니를 3 번 톱니바퀴는 9 시 방향의 톱니를 확인합니다.
  - 이는 `gears[2][2]` 와 `gears[3][6]` 을 비교하면 되는데, 이 둘이 **같다면 회전하지 않고 (0)** , 이 둘이 **다르다면** 2 번 톱니바퀴가 시계 방향 (1) 으로 회전하니까 **3 번 톱니바퀴는 반시계 방향 (-1) 으로 회전**할 것입니다.
  - 이런 회전 정보를 기록하면서, 오른쪽에 남은 톱니바퀴들도 검사합니다.

  ```jsx
  while (now < 3) {
  	const before = now;
  	now = ++now;

  	// 왼쪽 톱니바퀴가 회전하지 않으면, 오른쪽 톱니 바퀴 회전 X
  	if (directions[before] === 0) continue;

  	// 왼쪽 톱니바퀴와 오른쪽 톱니바퀴의 맞닿은 톱니가 같을 경우 회전 X
  	if (gears[before][right] === gears[now][left]) continue;

  	// 왼쪽 톱니바퀴가 회전하며, 맞닿은 톱니가 다른 경우 회전 O
  	directions[now] = directions[before] * -1;
  }
  ```

- 그리고 왼쪽으로 한 개씩 검사하는 과정은,

  - 위의 예시와 같게, 매개변수로 주어진 톱니바퀴가 2 번이고, 시계 방향 (1) 으로 회전한다고 가정하겠습니다.
  - 이 때, **2 번 톱니바퀴와 1 번 톱니바퀴의 맞물린 톱니**를 확인해야 합니다.
  - 2 번 톱니바퀴는 9 시 방향의 톱니를 1 번 톱니바퀴는 3 시 방향의 톱니를 확인합니다.
  - 이는 `gears[2][6]` 와 `gears[1][2]` 를 비교하면 되는데, 이 둘이 **같다면 회전하지 않고 (0)** , 이 둘이 **다르다면** 2 번 톱니바퀴가 시계 방향 (1) 으로 회전하니까 **1 번 톱니바퀴는 반시계 방향 (-1) 으로 회전**할 것입니다.

  ```jsx
  while (0 < now) {
  	const before = now;
  	now = --now;

  	// 오른쪽 톱니바퀴가 회전하지 않으면, 왼쪽 톱니 바퀴 회전 X
  	if (directions[before] === 0) continue;

  	// 오른쪽 톱니바퀴와 왼쪽 톱니바퀴의 맞닿은 톱니가 같을 경우 회전 X
  	if (gears[before][left] === gears[now][right]) continue;

  	// 오른쪽 톱니바퀴가 회전하며, 맞닿은 톱니가 다른 경우 회전 O
  	directions[now] = directions[before] * -1;
  }
  ```

## 문제 풀이 step 3

- 위와 같은 과정을 통해서 각각의 톱니바퀴의 회전 여부와 회전 방향 정보를 수집했으면, 회전하면 되겠습니다.
- 회전 정보는 다음과 같습니다.
  - `0` : 회전 하지 않음
    - 추가적으로 할 것은 없습니다.
  - `1` : 시계 방향으로 회전
    - 배열을 오른쪽 방향으로 한 칸씩 밀면 됩니다.
    - `gears[i].unshift(gears[i].pop())`
  - `-1` : 반시계 방향으로 회전
    - 배열을 왼쪽 방향으로 한 칸씩 밀면 됩니다.
    - `gears[i].push(gears[i].shift())`

## 문제 풀이 step 4

- 위와 같은 과정을 통해서 회전하는 함수를 구현했으면, 문제에서 주어진 회전 방법에 따라서 톱니바퀴를 회전하면 되겠습니다.
- 그리고 **각각의 톱니바퀴의 12 시 방향의 극을 확인**해서 각각의 극에 맞게 점수를 매긴 후 그 합을 출력하면 정답입니다.
- 추가적인 내용은 주석에 작성해놓겠습니다.

## 후기

- 배열 돌리기 문제를 몇 개 풀어보니, 어떻게 풀어야 할 지 감을 좀 잡은 것 같습니다.
- 이런 유형의 문제를 풀면서 배열로 정말 많은 것을 표현할 수 있다는 것을 깨닫게 되었습니다.
- 문제가 길기 때문에 오타가 충분히 날 수 있고, 항상 오타를 조심해야 합니다.
  - ex. directions 배열 생성 후, direction 으로 사용하는 것 조심

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const rotate = (gears, num, direction) => {
	const [left, right] = [6, 2];

	// 1 : 시계, -1 : 반시계, 0 : 무회전
	const directions = [0, 0, 0, 0];
	directions[num] = direction;

	let now = num;
	while (now < 3) {
		const before = now;
		now = ++now;

		// 왼쪽 톱니바퀴가 회전하지 않으면, 오른쪽 톱니 바퀴 회전 X
		if (directions[before] === 0) continue;

		// 왼쪽 톱니바퀴와 오른쪽 톱니바퀴의 맞닿은 톱니가 같을 경우 회전 X
		if (gears[before][right] === gears[now][left]) continue;

		// 왼쪽 톱니바퀴가 회전하며, 맞닿은 톱니가 다른 경우 회전 O
		directions[now] = directions[before] * -1;
	}

	now = num;
	while (0 < now) {
		const before = now;
		now = --now;

		// 오른쪽 톱니바퀴가 회전하지 않으면, 왼쪽 톱니 바퀴 회전 X
		if (directions[before] === 0) continue;

		// 오른쪽 톱니바퀴와 왼쪽 톱니바퀴의 맞닿은 톱니가 같을 경우 회전 X
		if (gears[before][left] === gears[now][right]) continue;

		// 오른쪽 톱니바퀴가 회전하며, 맞닿은 톱니가 다른 경우 회전 O
		directions[now] = directions[before] * -1;
	}

	for (let i = 0; i < 4; i++) {
		if (directions[i] === 0) continue;

		if (directions[i] === 1) gears[i].unshift(gears[i].pop());
		else gears[i].push(gears[i].shift());
	}
};

const solution = (input) => {
	const gears = input.slice(0, 4).map((v) => v.split("").map(Number));
	const k = Number(input[4]);
	const commands = input.slice(5, 5 + k).map((v) => v.split(" ").map(Number));

	for (let i = 0; i < k; i++) {
		let [num, direction] = commands[i];
		rotate(gears, num - 1, direction);
	}

	let res = 0;
	for (let i = 0; i < 4; i++) {
		// 0 : N 극, 1 : S 극
		if (gears[i][0] === 0) continue;
		else res += Math.pow(2, i);
	}

	return res;
};

console.log(solution(input));
```
