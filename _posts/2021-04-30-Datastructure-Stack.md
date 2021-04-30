---
layout: post
title: "Datastructure - Stack"
date: 2021-04-30 13:30:00 +0900
categories: Datastructure
---

# Stack

## 01. 개념

- **한 쪽 끝**에서 자료를 넣고 뺄 수 있는 **후입선출 (LIFO : Last-In First-Out) 형식**의 자료구조
- 나중에 들어간 데이터가 먼저 나가는 구조입니다.

## 02. 쓰이는 곳

- 함수 호출을 위한 시스템 스택
  - 함수 호출 후 복귀할 주소를 기억하는 데 스택이 사용됩니다.
- 텍스트 에디터의 되돌리기(undo) 기능
- 문자열을 역순으로 만들기
- 웹 브라우저 방문 기록 (뒤로가기)
- 수식의 괄호 검사
- 후위 표기법 계산

## 03. Stack 구현

- **언어** : JavaScript
- **설명**
  - top 변수는 스택이 비어있으면 -1 의 값을 가집니다. top 의 값이 0 이면 배열의 인덱스 0 에 데이터가 있다는 것을 의미합니다.
  - isFull 함수를 구현해보고자 MAX_STACK_SIZE 를 정해놓고 스택을 구현했습니다.

```jsx
class Stack {
	constructor() {
		this.MAX_STACK_SIZE = 100;
		this.bucket = [];
		this.top = -1;
	}

	isFull() {
		return this.top === this.MAX_STACK_SIZE - 1;
	}

	isEmpty() {
		return this.top === -1;
	}

	push(data) {
		if (!this.isFull()) this.bucket[++this.top] = data;
	}

	pop() {
		if (!this.isEmpty()) return this.bucket[this.top--];
	}

	peek() {
		if (!this.isEmpty()) return this.bucket[this.top];
	}
}
```

## Reference.

- C언어로 쉽게 풀어쓰는 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)
