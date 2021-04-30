---
layout: post
title: "Datastructure - Queue"
date: 2021-04-30 15:30:00 +0900
categories: Datastructure
---

# Queue

## 01. 개념

- 삽입과 삭제가 다른 쪽에서 일어나는 **선입선출(FIFO : First-In First-Out)** 형식의 자료구조
  - 삽입이 일어나는 곳 : **후단 (rear)**
  - 삭제가 일어나는 곳 : **전단 (front)**
- 먼저 들어온 데이터가 먼저 나가는 구조입니다.

## 02. 쓰이는 곳

- CPU 와 프린터 사이의 프린팅 버퍼
- CPU 와 키보드 사이의 키보드 버퍼
- CPU 작업 스케줄링
- 선입선출이 필요한 대기열 (은행 고객 대기열)
- 캐시 구현

## 03. Queue 구현

- **언어** : JavaScript
- **설명**
  - 삽입과 관련된 변수를 rear 라고 하고, 삭제와 관련된 변수를 front 라고 합니다. (초기값 : -1)
    - rear 는 큐의 마지막 요소를, front 는 큐의 첫 번재 요소를 가리킵니다.
  - isFull 함수를 구현해보고자 MAX_QUEUE_SIZE 를 정해놓고 큐를 구현했습니다.

```jsx
class Queue {
	constructor() {
		this.MAX_QUEUE_SIZE = 100;
		this.bucket = [];
		this.front = -1;
		this.rear = -1;
	}

	isEmpty() {
		return this.front === this.rear;
	}

	isFull() {
		return this.front === this.MAX_QUEUE_SIZE - 1;
	}

	enqueue(data) {
		if (!this.isFull()) this.bucket[++this.rear] = data;
	}

	dequeue() {
		if (!this.isEmpty()) return this.bucket[++this.front];
	}

	peek() {
		if (!this.isEmpty()) return this.bucket[this.front + 1];
	}
}
```

## 04. Stack 2 개로 Queue 구현

- **Stack 코드**

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

- **Queue 코드**

```jsx
class TwoStackToQueue {
	constructor() {
		this.leftStack = new Stack();
		this.rightStack = new Stack();
	}

	isEmpty() {
		return this.leftStack.isEmpty() && this.rightStack.isEmpty();
	}

	enqueue(data) {
		this.leftStack.push(data);
	}

	dequeue() {
		if (this.isEmpty()) return;

		if (this.rightStack.isEmpty()) {
			while (!this.leftStack.isEmpty())
				this.rightStack.push(this.leftStack.pop());
		}

		return this.rightStack.pop();
	}
}
```

## Reference.

- C언어로 쉽게 풀어쓰는 자료구조 (천인국, 공용해, 하상호 지음)
- [https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)
