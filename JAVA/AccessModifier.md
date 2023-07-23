# 접근제어자
- 접근 권한에 대한 예약어

## 종류

|        종류         |클래스 안 | 같은 패키지 | 상속받은 클래스 | import 클래스 |
|:-----------------:|:-------:|:------:|:-----------:|:-------------:|
|      public       |   O   |   O    |     O     |       O     |
|     protected     |  O    |   O    |   O      |     X       |
| (package private) |  O    |   O    |     X     |    X  |
|      private      |   O   |   X    |     X     |       X     |


### 1) public
- 어떤 클래스든 접근 가능
<br>

### 2) protected
- 같은 패키지 내에 있거나 상속받은 경우 접근 가능


### 3) package-private
- 아무런 접근제어자를 적지않을 경우
- 같은 패키지 내에만 접근 가능

### 4) private
- 해당 클래스내에서만 접근 가능

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)