---
layout: post
title: "이것이 코딩테스트다 - [BinarySearch] 떡볶이 떡 만들기 by JavaScript"
date: 2021-04-23 16:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 떡볶이 떡 만들기

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 문제가 원하는 것은 떡을 절단할 수 있는 높이의 최댓값입니다.
- 앞에서 배웠던 [이진 탐색](<https://qkrrlgh519.github.io/thisiscodingtest(ndb)/2021/04/22/ThisIsCodingtest-7-2.html>) 을 이용해서 풀 수 있습니다.
- 하지만, 이번에는 우리가 했던 방식에서 한 차원 더 들어간다고 생각하시면 될 것 같습니다.
  - 이전에 했던 방식은 특정 범위에서의 **mid 값**과 target 값을 바로 비교하는 방식입니다.
  - 이번에 할 방식은 특정 범위에서의 **mid 값을 이용해서 주어진 떡들 절단한 값들을 모두 더한 값**과 target 값을 비교하는 방식입니다.

## 문제 풀이 step 2

- 우선 **특정 범위를 정합니다.** 특정 범위는 떡을 절단할 수 있는 높이의 범위입니다. 이는 **0 ~ 주어진 떡들 중 가장 긴 떡의 길이** 까지입니다.
  - 이진 탐색을 적용하기 위해서는 주어진 배열이 정렬되어 있어야 한다고 했습니다.
  - 하지만 이번 문제에서는 배열이 주어지지 않습니다. 대신 우리가 그 범위를 정해야 합니다.
- 특정 범위에서 **이진 탐색을 진행**합니다.
  - 전체적인 이진 탐색의 로직은 동일합니다. mid 값을 이용해서 비교하는 방식
  - 단, 위에서 설명했듯이 비교하는 부분에서 한 단계 더 들어가야 합니다.
  - mid 값을 이용해서 주어진 떡 들을 자르고 남은 부위를 다 더한 합을 구합니다.
  - 그 합이 **손님이 요청한 떡의 길이보다 작다면** 떡을 덜 잘라야하겠죠?? 그렇다면 다음 이진 탐색의 범위는 **0 ~ mid - 1** 입니다.
  - 반대로, **크다면** 떡을 더 잘라도 되겠죠?? 다음 이진 탐색의 범위는 **mid + 1 ~ 주어진 떡들 중 가장 긴 떡의 길이** 입니다.
- 이진 탐색을 진행 후 손님이 요청한 떡의 길이를 맞출 수 있는 mid 값이 나온다면 mid 를 return 합니다.
  - 그러나 손님이 요청한 떡의 길이를 완전히 맞출 수 없는 경우도 있습니다.
  - 이 때는, **"적어도"** 손님이 요청한 떡의 길이 이상을 제공해드리면 되니까 **손님이 요청한 떡의 길이보다 클 때의 mid 값** 을 기록한 값을 return 합니다.
    - `else { res = mid; start = mid + 1; }`
- 과정 설명은 소스 코드에 주석으로 달겠습니다.

## 이진 탐색과 파라메트릭 서치

- 파라메트릭 서치는 최적화 문제를 결정 문제 ('예' 혹은 '아니오' 로 답하는 문제) 로 바꾸어 해결하는 기법이라고 합니다.
- '원하는 조건을 만족하는 가장 알맞은 값을 찾는 문제' 에 주로 파라메트릭 서치를 사용한다고 합니다.
- 예를 들어, 범위 내에서 조건을 만족하는 가장 큰 혹은 가장 작은 값을 찾는 최적화 문제를 이진 탐색으로 결정 문제를 해결하면서 범위를 좁혀갈 수 있다는 것입니다.

## 후기

- 이 책을 보기 전에 이진 탐색 문제를 접할 때, 처음 들었던 생각은 "문제의 내용이 이진 탐색과 정말 연관이 없어보이는데 이진 탐색으로 풀리네" 였습니다.
- 그래서 어떤 문제를 접했을 때, 이진 탐색이라는 방법을 떠올릴 수 있을지에 대한 의문을 가지고 있었습니다.
- 근데 저자님이 책에 설명해주신 내용을 보고나니 문제와 이진 탐색 사이에 어떤 연관이 있는지 정확하게 알게되어서 감사하는 마음이 생겼습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `4 6
// 19 15 10 17`.split("\n");

const cutAndCount = (n, riceCakes, mid) => {
	let count = 0;

	// mid 값을 이용해서 떡을 자르고, 자른 나머지를 합한다.
	for (let i = 0; i < n; i++) {
		if (riceCakes[i] <= mid) continue;

		count += riceCakes[i] - mid;
	}

	return count;
};

const binarySearch = (n, m, riceCakes, start, end) => {
	let res = null;
	while (start <= end) {
		const mid = parseInt((start + end) / 2);
		const count = cutAndCount(n, riceCakes, mid);

		// 손님이 요청한 떡의 길이에 딱 맞춰서 자른 경우
		if (count === m) return mid;

		if (count < m) {
			end = mid - 1;
		} else {
			// 손님이 요청한 떡의 길이에 딱 맞춰서 못 자른 경우를 대비해 mid 값을 기록
			res = mid;
			start = mid + 1;
		}
	}

	// 손님이 요청한 떡의 길이에 딱 맞춰서 못 자른 경우
	return res;
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const riceCakes = input[1].split(" ").map(Number);
	// 이진 탐색의 범위를 특정하기 위해서 max 값을 생성
	const max = Math.max(...riceCakes);

	const res = binarySearch(n, m, riceCakes, 0, max);
	return res;
};

console.log(solution(input));
```
