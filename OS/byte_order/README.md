## Byte Order
* Byte Order는 컴퓨터가 데이터를 읽는 방향에 따라서 두 가지 방식으로 나눠진다.   (붕어빵을 머리부터 먹는 사람과 꼬리부터 먹는 사람을 나누는 것과 비슷하다.)
* 우리가 '안녕하세요'를 거꾸로 말할때 '요세하녕안'이라고 말하는 것처럼   
  (자음과 모음까지 거꾸로 하지 않고 한글자씩 거꾸로 말한다.)
  컴퓨터는 Byte 단위로 데이터를 읽기 때문에 11111111 00000000 앞에 있는 2Byte의 데이터를 Little Endian 방식으로 읽는다면 00000000 11111111 처럼 Byte 단위로 값을 읽어낸다.
* 위와 같은 이유로 1Byte의 데이터는 Byte Order가 어떤 방식으로 읽어도 똑같이 읽을 것이다.

### Big Endian
* Big Endian은 더 큰 값부터 데이터를 저장한다.
    * 데이터를 표현하는 여러 방법(2진법, 8진법 등)을 생각하면 왼쪽에 있는 수가 더 큰 값을 나타내기 때문에 왼쪽부터 값을 읽는다고 볼 수 있다.

### Little Endian
* Little Endian은 더 작은 값부터 데이터를 저장한다.
    * 위와 같은 이유로 오른쪽에 있는 수가 더 작은 값을 나타내기 때문에 오른쪽부터 값을 읽는다고 볼 수 있다.