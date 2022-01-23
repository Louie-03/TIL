## 논리 회로
* 디지털 전자 신호에서는 강한 전류와 약한 전류 두 가지 방법만으로 작동한다.
* 컴퓨터는 이 두 가지 방법을 참과 거짓으로 구분한다.
* 예시는 테스트 코드로 대신하겠다.

## 논리 게이트

* 논리 게이트는 논리 회로를 구성하는 가장 기본적인 요소이다.

### 1. or 게이트

* 두 개의 입력값 중 하나라도 참이면 참을 반환한다.
```java
@Test
void or() {
    assertTrue(Gate.or(true, true));
    assertFalse(Gate.or(false, false));
    assertTrue(Gate.or(true, false));
    assertTrue(Gate.or(false, true));
}
```

### 2. and 게이트
* 두 개의 입력값 모두 참이면 참을 반환한다.
```java
@Test
void and() {
    assertTrue(Gate.and(true, true));
    assertFalse(Gate.and(false, false));
    assertFalse(Gate.and(true, false));
    assertFalse(Gate.and(false, true));
}
```

### 3. inverter(not) 게이트
* 입력받은 값의 반대값을 반환한다.
* 한개의 값만 입력받는다.
* not 게이트 라고도 부른다.
```java
@Test
void and() {
    assertTrue(!false);
    assertFalse(!true);
}
```

### nand 게이트
* 두 입력값이 모두 참이면 거짓을 반환한다.
* and 게이트의 결과에 not 게이트를 실행한 것과 결과가 동일하다.
```java
@Test
void nand() {
    assertFalse(Gate.nand(true, true));
    assertTrue(Gate.nand(false, false));
    assertTrue(Gate.nand(true, false));
    assertTrue(Gate.nand(false, true));
}
```

### xor 게이트
* 두 입력값이 다르면 참을 반환한다.
* nand 게이트의 결과에 or 게이트를 실행한 것과 결과가 동일하다.
```java
@Test
void xor() {
    assertFalse(Gate.xor(true, true));
    assertFalse(Gate.xor(false, false));
    assertTrue(Gate.xor(true, false));
    assertTrue(Gate.xor(false, true));
}
```

