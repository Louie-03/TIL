## ArrayList.add(E e)의 동작 방식

ArrayList는 내부에 elementData라는 배열에 값을 저장한다.

```java
transient Object[] elementData;
```

`ArrayList.add(E e)`의 구현 코드는 다음과 같다.

```java
public boolean add(E e) {
    modCount++;
    add(e, elementData, size);
    return true;
}
```

modCount에 대한 내용은 잘 이해하지 못해서 다음에 다뤄보겠다.

더 자세한 내용을 알아보기 위해 `add(e, elementData, size);` 를 살펴보자.

```java
private void add(E e, Object[] elementData, int s) {
    if (s == elementData.length)
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}
```

elementData의 길이가 size와 같아서 더 이상 값을 추가하지 못한다면 `grow()`를 통해 elementData의 길이를 늘려준다.

`elementData[s]` : elementData의 맨 뒤를 뜻한다. 즉, 맨 뒤에 e를 입력한다.

마지막으로 size를 1만큼 증가시킨다.

`grow()` 를 살펴보자.

```java
private Object[] grow() {
    return grow(size + 1);
}
```

매개변수가 없으면 `grow(int minCapacity)`에 매개변수로 size + 1을 넣어준다.

`grow(int minCapacity)`를 살펴보자.

```java
private Object[] grow(int minCapacity) {
    return elementData = Arrays.copyOf(elementData,
                                       newCapacity(minCapacity));
}
```

`newCapacity(int minCapacity)`의 반환값 만큼 길이를 가진 elementData를 반환하는 메서드이다.

`newCapacity(int minCapacity)`를 살펴보자.

```java
private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity <= 0) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}
```

`int newCapacity = oldCapacity + (oldCapacity >> 1);` :  쉬프트 연산자를 통해 elementData 길이의 1.5배가 되는 값을 할당한다. 

`if (newCapacity - minCapacity <= 0)` : newCapacity가 elementData의 길이 + 1 이하면 참이다. 안에 있는 조건문의 결과가 거짓이라면 minCapacity를 반환한다.

`if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)` : 참이면 DEFAULT_CAPACITY(10) 또는 minCapacity 둘 중 큰 값을 반환한다.

아래를 보면 `DEFAULTCAPACITY_EMPTY_ELEMENTDATA`는 ArrayList에 선언된 비어있는 배열인 것을 알 수 있다.

```java
/**
 * Shared empty array instance used for default sized empty instances. We
 * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
 * first element is added.
 */
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
```

` if (minCapacity < 0)` : minCapacity가 음수이면 OutOfMemoryError()를 반환한다.

`return (newCapacity - MAX_ARRAY_SIZE <= 0)` : 배열의 최대 크기보다 크지 않으면 newCapacity를 반환하고 아니면  hugeCapacity(minCapacity)의 반환값을 반환하는데, `newCapacity(int minCapacity)`의 내부 로직을 봤을 때 대부분 현재 elementData의 길이의 1.5배를 반환한다고 보면 된다.

## 결론

정리하자면 `ArrayList.add(E e)` 메서드는 더 이상 Element를 추가할 수 있는 공간이 없으면 여러 내부 메서드를 통해서 검증을 하고 대부분 현재 ArrayList의 크기를 1.5배 만큼 증가시키고 Element를 추가하도록 구현했다.