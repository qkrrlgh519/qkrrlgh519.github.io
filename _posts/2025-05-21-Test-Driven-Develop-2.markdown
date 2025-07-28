---
layout: post
title: "Test Driven Develop - 2"
date: 2025-05-21 12:00:00 +0900
categories: TestDrivenDevelop
---

# 화폐 예제

## 0장. 테스트 주도 개발(TDD)의 리듬

1. 재빨리 테스트를 하나 추가한다.
2. 모든 테스트를 실행하고 새로 추가한 것이 실패하는지 확인한다.
3. 코드를 조금 바꾼다.
4. 모든 테스트를 실행하고 전부 성공하는지 확인한다.
5. 리팩토링을 통해 중복을 제거한다.

## 1장. 다중 통화 지원 Money 객체

### 테스트 항목

- 통화가 다른 두 금액을 더해서 주어진 환율에 맞게 변한 금액을 결과로 얻을 수 있어야 한다.
- 어떤 금액(주가)을 어떤 수(주식의 수)에 곱한 금액을 결과로 얻을 수 있어야 한다.

### 테스트 코드

- 테스트 작성시 오퍼레이션의 완벽한 인터페이스에 대해 상상해보는 것이 좋다.
- 실패하는 테스트가 주어진 상태이고, 최대한 빨리 초록 막대를 보고 싶을 뿐이다.
- 당장의 목표는 완벽한 해법을 구하는 것이 아니라 테스트를 통과하는 것일 뿐이다.

```java
// 간단 곱셈 테스트 코드 예시
public void testMultiplication() {
	Dollar five = new Dollar(5);
	five.times(2);
	assertEquals(10, five.amount);
}

// 에러1. Dollar 클래스 없음
class Dollar

// 에러2. 생성자 없음
Dollar(int amount){}

// 에러3. times(int) 메서드 없음
void times(int multiplier){}

// 에러4. amount 필드 없음
int amount
```

### 의존성과 중복

- (스티븐 프리만) 테스트와 코드 간의 문제는 중복이 아니다. 문제는 테스트와 코드 사이에 존재하는 의존성이다.
- 즉, 코드나 테스트 중 한쪽을 수정하면 반드시 다른 한쪽도 수정해야만 한다는 것이다.
- 우리의 목표는 코드를 바꾸지 않으면서도 뭔가 의미 있는 테스트를 하나 더 작성하는 것인데, 현재의 구현으로는 불가능하다.
- 의존성이 문제 그 자체라면 중복(duplication)은 문제의 징후이다.
- 중복된 로직이란 동일한 문장이 코드의 여러 장소에 나타나는 것을 의미하며, 중복된 로직을 하나로 끄집어내는 일엔 객체를 이용하는 것이 최고다.
- 프로그램에서는 중복만 제거해주면 의존성도 제거된다. 이게 바로 TDD 의 두 번째 규칙이 존재하는 이유이다.

### 중복 제거

```java
// 5와 2가 두 곳에 존재 (테스트에 있는 데이터와 코드에 있는 데이터 사이 = 테스트와 코드 사이에 존재하는 의존성)
int amount = 5 * 2;
```

- 5와 2가 두 곳에 존재한다. 이 중복을 제거해야 한다.
- 객체의 초기화 단계에 있는 설정 코드를 times() 메서드 안으로 옮긴다.
- TDD의 핵심은 이런 작은 단계를 밟아 나가며, 이런 작은 단계를 밟을 능력을 갖추는 것이다.

```java
int amount

void times(int multiplier) {
	amount = 5 * 2;
}
```

```java
// 5를 어디서 얻을 수 있을까?? 이건 생성자에서 넘어오는 값이니 amonut 변수에 저장
Dollar(int amount) {
	this.amount = amount;
}

// amount 가져다 사용 가능
void times(int multiplier) {
	amount = amount * 2;
}

// multiplier 의 값이 2이므로, 상수를 이 인자로 대체
void times(int multiplier) {
	amount *= multiplier;
}
```

## 2장. 타락한 객체

### 일반적인 TDD 주기

- 테스트 작성
- 실행 가능하게 만들기 (가장 중요한 것은 빨리 초록 막대를 보는 것)
- 올바르게 만들기 (직전에 저질렀던 죄악을 수습)

### 작동하는 깔끔한 코드 얻기

```java
public void testMultiplication() { // 곱셈 테스트
	Dollar five = new Dollar(5);
	// [문제] times() 를 호출한 이후에 five 는 더 이상 5가 아니다. 그러면 새로운 객체를 반환하게 만들면 어떨까?
	five.times(2);
	assertEquals(10, product.amount);
	five.times(3);
	assertEquals(15, product.amount);
}
```

```java
Dollar five = new Dollar(5);

// 메서드에 반환값이 없기 때문에 컴파일조차 되지 않는다.
Dollar product = five.times(2);
assertEquals(10, product.amount);
product = five.times(3);
assertEquals(15, product.amount);
```

```java
// 아래 같이 수정해야 컴파일이 된다. 하지만 실행되지 않는다.
Dollar times(int multiplier) {
	amount *= multiplier;
	return null;
}
```

#### 빠르게 초록색 보는 전략

- 가짜로 구현하기 : 상수를 반환하게 만들고, 진짜 코드를 얻을 때까지 단계적으로 상수를 변수로 변경
- 명백한 구현 사용하기 : 실제 구현을 입력

#### 학습 내용 검토

- 설계상 결함(Dollar 부작용)을 그 결함으로 인해 실패하는 테스트로 변환
- 스텁 구현으로 빠르게 컴파일을 통과하도록 구현
- 올바르다고 생각하는 코드를 입력하여 테스트를 통과
- `느낌(부작용에 대한 혐오감)을 테스트(하나의 Dollar 객체에 곱하기 두 번 수행)로 변환하는 것은 TDD의 일반적 주제`

## 3장. 모두를 위한 평등

### 값 객체 패턴 (Value Object Pattern)

- 지금의 Dollar 객체같이 객체를 값처럼 쓸 수 있는 방식
- 객체의 인스턴스 변수가 생성자를 통해서 일단 설정된 후 결코 변하지 않는 것이 제약사항
- 값 객체를 사용하면 별칭 문제에 대해 걱정할 필요가 없다는 것이 아주 큰 장점
  - 두 번째 수표의 값 변경시 첫 번째 수표의 값까지 변경되는 것이 별칭 문제

### 값 객체는 equals() 를 구현

```java
// 빨간 막대
public void testEquality() {
	assertTrue(new Dollar(5).equals(new Dollar(5)));
}

// 가짜로 구현
public boolean equals(Object object) {
	return true;
}
```

### 삼각측량법

- 만약 라디오 신호를 두 수신국이 감지
- 수신국 사이의 거리가 알려져 있고, 각 수신국이 신호의 방향을 알고 있다면,
- 이 정보들만으로 충분히 신호의 거리와 방위를 알 수 있음

```java
public void testEquality() {
	assertTrue(new Dollar(5).equals(new Dollar(5)));
	assertFalse(new Dollar(5).equals(new Dollar(6)));
}

// 동치성 일반화 => Dollar 와 Dollar 를 직접 비교 가능
public boolean equals(Object object) {
	Dollar dollar = (Dollar) object;
	return amount == dollar.amount;
}
```

## 4장. 프라이버시

### amount 를 private 으로

```java
// 개념적으로 Dollar.times() 연산은 호출받은 객체의 값에 인자로 받은 곱수만큼 곱한 값을 갖는 Dollar 를 반환해야 함
// 하지만 테스트가 정확히 그것을 의미하지 않음
public void testMultiplication() {
	Dollar five = new DOllar(5);
	Dollar product = five.times(2);
	assertEquals(10, product.amount); // Dollar 를 반환하지 않는 것을 의미하는 부분
	product = five.times(3);
	assertEquals(15, product.amount);
}

// 단언(assertion)을 Dollar와 Dollar를 비교하는 것으로 재작성
public void testMultiplication() {
	Dollar five = new Dollar(5);
	assertEquals(new Dollar(10), five.times(2));
	assertEquals(new Dollar(15), five.times(3));
}

// 테스트를 고치고 나니 이제 Dollar의 amount 인스턴스 변수를 사용하는 코드는 Dollar 자신밖에 없어서, private 로 변경
```

- 만약 동치성 테스트가 동치성에 대한 코드가 정확히 작동한다는 것을 검증하는 데 실패한다면, 곱하기 테스트 역시 실패한다.
- 이것은 TDD 를 하면서 적극적으로 관리해야 할 위험요소다. 우리는 완벽함을 위해 노력하지 않는다.
- 모든 것을 두 번 말함으로써(코드와 테스트로 한 번씩) 자신감을 가지고 전진할 수 있을 만큼만 결함의 정도를 낮추길 희망한다.
- 그럼에도 결함이 생긴다면, 테스트를 어떻게 작성해야 했는지에 대한 교훈을 얻고 다시 앞으로 나아간다.

## 5장. 솔직히 말하자면

### 5달러 + 10프랑 = 10달러 (환율 고려)

```java
// Dollar 객체와 비슷하게 작동하는 Franc 라는 객체
public void testFrancMultiplication() {
	Franc five = new Franc(5);
	assertEquals(new Franc(10), five.times(2));
	assertEquals(new Franc(15), five.times(3));
}

// Dollar 코드를 복사해서 Dollar 를 Franc 로 변경

// 1. 테스트 작성
// 2. 컴파일되게 하기
// 3. 실패하는지 확인하기 위해 설명
// 4. 실행하게 만듦
// 5. 중복 제거
```

- 각 단게에는 서로 다른 목적이 있다.
- 처음 네 단계는 빨리 진행해야 한다. 그러면 새 기능이 포함되더라도 잘 알고 있는 상태에 이를 수 있다.
- 주기의 다섯 번째 단계 없이는 앞의 네 단계도 제대로 되지 않는다.
- 적절한 시기에 적절한 설계를 해라. 돌아가게 만들고, 올바르게 만들어라.

```java
class Franc {
	private int amount;

	Franc(int amount) {
		this.amount = amount;
	}

	Franc times(int multiplier) {
		return new Franc(amount * multiplier);
	}

	public boolean equals(Object object) {
		Franc franc = (Franc) object;
		return amount == franc.amount;
	}
}

// 중복이 엄청 많다. equals() 를 일반화하는 것부터 시작하자.
```

## 6장. 돌아온 '모두를 위한 평등'

### 선 테스트통과 후 청소

- 5장에서 실제로 새로운 테스트 케이스를 하나 작동하게 만듦
- 하지만 테스트를 빨리 통과하기 위해 몇 톤이나 되는 코드를 복사해서 붙이는 엄청난 죄를 저지름
- 이제 청소할 시간

```java
// !! 두 클래스의 공통 상위 클래스 찾기

// Money - Dollar, Franc
// Money 클래스가 공통의 equals 코드 갖게 하면 어떨까?
class Money

// Dollar 가 Money 를 상속
class Dollar extends Money {
	private int amount;
}

// amount 인스턴스 변수를 Money 로 옮김
// 하위 클래스에서도 변수를 볼 수 있도록 가시성을 private 에서 protected 로 변경
class Money {
	protected int amount;
}
class Dollar extends Money { }

// equals() 코드를 위(class Money)로 올리기
// 케스트(cast) 부분 변경 및 임시 변수 명칭 변경
// 이러면 이제 equals() 메서드를 Dollar 에서 Money 로 옮길 수 있다.
public boolean equals(Object object) {
	Money money = (Money) object; // before : Money dollar = (Dollar) object;
	return amount = money.amount;
}
```

### Franc.equals() 제거

```java
// 테스트 작성 (Dollar 테스트 복사)
// 또 중복 존재 (두 줄이나 더!)
public void testEquality() {
	assertTrue(new Dollar(5).equals(new Dollar(5)));
	assertFalse(new Dollar(5).equals(new Dollar(6)));
	assertTrue(new Franc(5).equals(new Franc(5)));
	assertFalse(new Franc(5).equals(new Franc(6)));
}

// Money 상속 및 amount 필드 제거
class Franc extends Money {
	private int amount;
}
class Franc extends Money { }

// Franc.equals() 는 Money.equals() 와 거의 비슷
// Franc 의 불필요한 코드 제거 (Money 로 옮김)
public boolean equals(Object object) {
	Money money = (Money) object; // before : Money franc = (Franc) object;
	return amount == money.amonut;
}
```

- 공통된 코드를 첫 번째 클래스(Dollar) 에서 상위 클래스(Money) 로 단계적으로 옮김 (Franc 도 마찬가지)
- 불필요한 구현을 제거하기 전에 두 equals() 구현을 일치시킴

## 7장. 사과와 오렌지 (서로 다른 걸 비교할 수 없다)

```java
// Franc 과 Dollar 를 비교
public void testEquality() {
	assertFalse(new Franc(5).equals(new Dollar(5)));
}

// 동치성 코드에서는 Dollar 가 Franc 과 비교되지 않는지 검사해야 함
// 두 객체의 클래스를 비교해서, 오직 금액과 클래스가 서로 동일할 때만 두 Money 가 서로 같은 것이다.
// 혼합된 통화 간의 연산에 대해 다루어야 함
public boolean equals(Object object) {
	Money money = (Money) object;
	return amount == money.amount && getClass().equals(money.getClass());
}

```

## 8장. 객체 만들기

### 팩토리 메서드 패턴 (Factory Method Pattern)

- 객체 생성(인스턴스화)을 서브클래스에 위임하여, 클래스 간의 결합도를 낮추는 생성 디자인 패턴
- 즉, 객체를 직접 생성하지 않고, 객체 생성을 위한 인터페이스만 정의하고, 실제 생성은 서브클래스에서 담당
- 객체 생성을 캡슐화하여, 코드의 유연성과 확장성을 높인다.
- 장점
  - 확장성 향상 : 새로운 객체 생성 로직이 필요할 때, 기존 코드를 수정하지 않고 새로운 팩토리 클래스만 만들면 됨
  - 느슨한 결합 : 클라이언트는 어떤 구체적인 클래스가 생성되는지 몰라도 됨
- 단점
  - 클래스 수 증가 : 구체 팩토리 및 제품 클래스가 많아질 수 있음
  - 과한 구조 : 단순 객체 생성에는 과한 구조

```java
// 1. Product
public interface Animal {}

// 2. Concrete Product
public class Dog implements Animal {}
public class Cat implements Animal {}

// 3. Creator
public abstract class AnimalFactory {
	public abstract Animal createAnimal();
}

// 4. Concrete Creator
public class DogFactory extends AnimalFactory {
	public Animal createAnimal() {
		return new Dog();
	}
}
public class CatFactory extends AnimalFactory {
	public Animal createAnimal() {
		return new Cat();
	}
}

// Client
public class Main {
	public static void main(String[] args) {
		AnimalFactory factory = new DogFactory(); // 팩토리 변경시 생성 객체 변경
		Animal animal = factory.createAnimal();
	}
}
```

### 두 times() 구현 코드가 거의 동일

```java
Franc times(int multiplier) {
	return new Franc(amount * multiplier);
}
Dollar times(int multiplier) {
	return new Dollar(amount * multiplier);
}

// 양쪽 모두 Money 반환하게 만들어 더 비슷하게 만들 수 있음
Money times(int multiplier) {
	return new Franc(amount * multiplier);
}
Money times(int multiplier) {
	return new Dollar(amount * multiplier);
}
```

- Money 의 두 하위 클래스의 역할이 거의 없어, 제거하면 좋아보임
- 하위 클래스에 대한 직접적인 참조가 적어진다면 하위 클래스를 제거하기 위해 한 발짝 더 다가선 것
- Money 클래스에 Dollar 를 반환하는 팩토리 메서드(Factory Method) 도입 가능

```java
// 테스트 코드
public void testMultiplication() {
	Dollar five = Money.dollar(5);
	assertEquals(new Dollar(10), five.times(2));
	assertEquals(new Dollar(15), five.times(3));
}

// Money.dollar()
static Dollar dollar(int amount) {
	return new Dollar(amount);
}

// Dollar 에 대한 참조가 사라지길 바라며 테스트 선언부 아래와 같이 변경
public void testMultiplication() {
	Money five = Money.dollar(5);
	assertEquals(new Dollar(10), five.times(2));
	assertEquals(new Dollar(15), five.times(3));
}

// Money 에는 times() 정의되지 않아서, Money 를 추상 클래스로 변경 후 times() 선언
abstract class Money
abstract Money times(int multiplier);

// 팩토리 메서드 변경
static Money dollar(int amount) {
	return new Dollar(amount);
}

// 테스트 코드에 팩토리 메서드 적용
public void testMultiplication() {
	Money five = Money.dollar(5);
	assertEquals(Money.dollar(10), five.times(2)); // 적용
	assertEquals(Money.dollar(15), five.times(3)); // 적용
}
public void testEquality() {
	assertTrue(Money.dollar(5).equals(Money.dollar(5))); // 적용
	assertFalse(Money.dollar(5).equals(Money.dollar(6))); // 적용
	assertTrue(new Franc(5).equals(new Franc(5)));
	assertFalse(new Franc(5).equals(new Franc(6)));
	assertFalse(new Franc(5).equals(Money.dollar(5))); // 적용
}
```

- 위처럼 어떤 클라이언트 코드도 Dollar 라는 이름의 하위 클래스가 있다는 사실을 모른다.
- 하위 클래스의 존재를 테스트에서 분리(decoupling)함으로써 어떤 모델 코드에도 영향을 주지 않고 상속 구조를 마음대로 변경할 수 있게 됐다.

```java
// Franc 도 팩토리 메서드 적용
static Money franc(int amount) {
	return new Franc(amount);
}
```

## 9장. 우리가 사는 시간(time = 곱하기)

### 통화 개념을 어떻게 테스트할까?

```java
public void testCurrency() {
	assertEquals("USD", Money.dollar(1).currency());
	assertEquals("CHF", Money.franc(1).currency());
}

// Money 에 currency() 메서드 선언 및 하위 클래스에 구현
// Money
abstract String currency();

// Franc
String currency() {
	return "CHF";
}

// Dollar
String currency() {
	return "USD";
}
```

- 두 클래스를 모두 포함할 수 있는 동일한 구현이 되어야 한다.
- 통화를 인스턴스 변수에 저장하고, 메서드에서는 그냥 그걸 반환하게 만들 수 있게 (equals 테스트를 위해)

```java
// Franc
private String currency;
Franc(int amount) {
	this.amount = amount;
	currency = "CHF";
}
String currency() {
	return currency;
}

// Dollar
private String currency;
Dollar(int amount) {
	this.amount = amount;
	currency = "USD";
}
String currency() {
	return currency;
}
```

- 두 currency() 가 동일하므로, 변수 선언과 currency() 구현을 둘다 위로 올릴 수 있게 됐다.

```java
// Money
protected String currency;
String currency() {
	return currency;
}
```

## 25년 7월 28일 일시정지

