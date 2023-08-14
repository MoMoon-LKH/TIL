# 쓰레드
- 경량 프로세스 (Lightweight Process)
- 프로세스(프로그램)내에서 실제로 작업을 수행하는 주체


## 특징
- 자바는 JVM 내에서 쓰레드가 실행됨
- 대부분의 작업은 단일 쓰레드보다 멀티 쓰레드로 실행하는 것이 빠르다
- class 내에 run() 메소드가 종료되면 끝난다
- Thread의 우선 순위는 10이 가장 높으며 가장 낮은 순위는 1이다

<br>

## 상태
|      상태       |                 의미                  |
|:-------------:|:-----------------------------------:|
|      NEW      |          쓰레드 객체는 생성, 시작은 x          |
|   RUNNABLE    |            쓰레드가 실행중인 상태             |
|    BLOCKED    | 쓰레드가 실행 중지 상태이며, 모니터 락이 풀리기 기다리는 상태 |
|    WAITING    |            쓰레드가 대기중인 상태             |
| TIMED_WAITING |        특정 시간만큼 쓰레드가 대기중인 상태         |
|  TERMINATED   |             쓰레드가 종료된 상태             |

<br>

## 사용 방식
### 1) Runnable Interface
```java
public class RunnableSample implements Runnable{
    public void run() {
        // 실행 내용
    }
}

public static void main(String[] args) {
    RunnableSample sample = new RunnableSample();
    new Thread(sample).start();
}
```
- 해당 클래스가 다른 클래스를 확장할 필요(다중 상속)이 필요하다면 Runnable 사용

<br>

### 2) Thread Class
```java
public class ThreadSample extends Thread {
    public void run() {
        // 실행 내용
    }
}

public static void main(String[] args) {
    ThreadSample threads = new ThreadSample();
    threads.start();
}
```
- Thread만 상속 받는다면 해당 방식이 구현하기 쉽기 때문에 해당 방식을 권장

<br>

## 많이 쓰는 메소드
### 1) run 
- 쓰레드 실행 시 실행되는 메소드
- 개발자가 직접 구현해야되는 메소드

### 2) sleep
- 매개변수로 넘어온 A X 1ms 만큼 대기시키는 메소드
- 해당 메소드를 사용할 때 try-catch 안에 사용해야된다 <br>
  ㄴ InterruptedException을 던질 수 있다고 선언

### 3) join
- 쓰레드가 종료될 때까지 기다리는 메소드

### 4) interrupt
- 현재 수행중인 메소드를 중단시키는 메소드
<br>

## synchronized
- Thread-Safe를 시키기위한 예약어
- 같은 객체를 참조할 때만 유효

### Thread-Safe란
- 몇 개의 Thread가 와도 안전한 클래스나 메소드
```java
public class CommonCalculate {

    private int amount;

    public CommonCalculate() {
        amount = 0;
    }

    public void plus(int value) {
        amount += value;
        
    }
}
```
```java
public class ModifyAmountThread extends Thread {

    private CommonCalculate calculate;
    private boolean addFlag;

    public ModifyAmountThread(CommonCalculate calculate, boolean addFlag) {
        this.calculate = calculate;
        this.addFlag = addFlag;
    }

    public void run() {
        for (int loop = 0; loop < 1000; loop++) {
            if(addFlag) calculate.plus(1);
            else calculate.minus(1);
        }
    }
}
```
위 쓰레드를 100개의 쓰레드로 실행 시켰을 때 100 X 1000 = 100000이 정상적인 값이다. <br>
하지만 실제 출력되는 값은 99875, 99994 등 실행 시킬 때마다 다르다. <br> <br>
멀티 쓰레드는 동시에 작업을 처리한다. <br> 
동시에 처리하면 여러개의 쓰레드가 인스턴스 변수에 동시에 접근하여 같은 값으로 변경시키는 동시성 이슈가 발생할 수 있다. <br>
이를 Threed-unsafe 라고한다. <br> <br>
그러면 Thread-safe를 위한 전제 조건은 동시 접근을 막는 것이다. 즉, 한 쓰레드씩 메소드에 들어가게 해주면 된다. <br>
이를 해주는 예약어가 synchronized다.
<br>

### 사용 방법
#### 1) synchronized 메소드
```java
public class CommonCalculate {

    private int amount;

    public CommonCalculate() {
        amount = 0;
    }

    public synchronized void plus(int value) {
        amount += value;
        
    }
}
```
- syncronized를 메소드에 선언하면 해당 메소드를 들어올 때 하나의 쓰레드만 들어온다
<br>

#### 2) synchronized 블럭
```java
public class CommonCalculate {

    private int amount;

    public CommonCalculate() {
        amount = 0;
    }

    publicvoid plus(int value) {
        synchronized (this) {
            amount+=value;
        }
    }
}
```
- 해당 메소드에는 여러 쓰레드가 접근이 가능하지만 synchronized 블럭이 선언된 시점부터 하나의 쓰레드만 접근이 가능하다


<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)
