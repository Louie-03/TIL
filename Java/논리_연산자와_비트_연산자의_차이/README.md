## 논리 연산자와 비트 연산자의 차이는?
* 평소에 논리 연산자와 비트 연산자가 무엇인지 알고 있었지만 비트 단위로 계산할 일이 거의 없어서 그런지 논리 연산자만 사용하게 됐다.
* 두 연산자의 차이가 궁금해져서 검색해 봤는데 "비트 연산자는 비트에 대해 작업하고 비트 단위로 작업을 수행하는 반면 논리 연산자는 여러 조건에 따라 결정을 내리는 데 사용된다."라는 내용을 찾았지만 이미 알던 내용이었다.
* 애초에 사용하는 용도가 다르다는 것을 알고 있었지만 차이점이 더 없는지 알아보고 드디어 내가 몰랐던 두 연산자의 차이점을 찾아냈다.
```java
@Test
void operator() {
    assertTrue(logicOperator() || logicOperator());
    assertTrue(bitOperator() | bitOperator());
}

boolean logicOperator() {
    System.out.println("logic");
    return true;
}

boolean bitOperator() {
    System.out.println("bit");
    return true;
}
```
* 위 코드는 두 연산자의 차이점을 비교하기 위해 작성한 코드이다.
* 둘 다 true를 반환하고 메소드가 실행될 때마다 연산자 이름이 출력된다.
* 아래는 테스트 코드를 실행하고 출력된 내용이다.

> logic   
bit   
bit

* 출력된 내용을 보면 보면 비트 연산자는 결과에 영향을 주지 않아도 모두 실행했고 논리연산자는 결과에 영향을 주지 않는다면 실행되지 않아서 한 번만 실행했다.
  
  * or 연산자에서 첫 번째 비교한 값이 참이면 두 번째 값을 비교하지 않아도 결과가 같기 때문에 이럴 때 결과에 영향을 주지 않는다고 말할 수 있다.
  
    이것을 [Short-circuit evaluation](https://en.wikipedia.org/wiki/Short-circuit_evaluation)이라고 한다.

* 비트 연산자는 or 연산자를 통해서 비트를 비교할 때 논리 연산자처럼 참과 거짓으로 나누어지는 것이 아니라 비트의 자리마다 비교하여 결과를 반환하기 때문에 Short-circuit evaluation이 발생하지 않는 것으로 생각한다.

## 참고 자료

* https://tomining.tistory.com/150

* https://en.wikipedia.org/wiki/Short-circuit_evaluation