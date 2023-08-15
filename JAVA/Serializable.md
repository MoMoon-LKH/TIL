# Serializable
- 객체를 통한 파일 저장 및 읽기, 서버 통신에 필요한 인터페이스
```java
public class SerialDTO implements Serializable { // 인터페이스 선언

    static final long serialVersionUID = 1L; // 해당 객체 버전 명시하면 해당 객체 내용이 변경되어도 이 값은 변하지 않는다
    private String name;
    private String phoneNumber;
    private boolean bookMark;

    public SerialDTO(String name, String phoneNumber, boolean bookMark) {
        super();
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.bookMark = bookMark;
    }

    @Override
    public String toString() {
        return "SerialDTO{" +
                "name='" + name + '\'' +
                ", phoneNumber=" + phoneNumber +
                ", bookMark " + soldPerDay +
                "}";
    }
}
```

## ObjectOutputStream통한 객체 저장
```java
String fullPath = "C:\\godofjava\\serial.obj";
SerialDTO dto = new SerialDTO("LEE", "010-2312-2312", false);

FileOutputStream fos = null;
ObjectOutputStream oos = null;

try {
    fos = new FileOutputStream(fullPath);
    oos = new ObjectOutputStream(fos); 
    oos.writeObject(dto); // 해당 메소드를 통해 객체 저장
    System.out.println("Write Success");
} catch ...
```
<br>

## ObjectInputStream를 통한 객체 읽기
```java
String fullPath = "C:\\godofjava\\serial.obj";

FileInputStream fis = null;
ObjectInputStream ois = null;

try {
    fis = new FileInputStream(fullPath);
    ois = new ObjectInputStream(fis);
    Object obj = ois.readObject();
    SerialDTO dto = (SerialDTO) obj;
    System.out.println(dto);
} catch (Exception e) { ...
```
<br>

### serialVersionUID
```java
 static final long serialVersionUID = 1L;
```
- 반드시 static final long으로 선언
- 객체의 버전을 명시하는데 사용
- 객체가 변경되면 컴파일 시 serialVersionUID가 새로 생성됨

<br>


### transient
```java
transient private int bookOrder;
```
- transient 예약어를 사용하여 선언한 변수는 Serializeable의 대상에서 제외
- 보안상 중요한 변수나 꼭 저장해야 할 필요가 없는 변수에 대하여 사용 가능

<br>

## NIO
- 속도로 인해 생겨난 new IO
- Channel과 Buffer를 사용하여 처리
- 파일 저장, 읽기 및 파일 복사, 네트워크 통신에 사용 가능

```java
public class NioSample {

    public void basicWriteAndRead() {
        String fileName = "C:\\godofjava\\nio.txt";
        try {
            writeFile(fileName, "MyFirst NIO sample");
            readFile(fileName);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void writeFile(String fileName, String data) throws Exception {
        FileChannel channel = new FileOutputStream(fileName).getChannel(); // Channel
        byte[] byteData = data.getBytes(); 
        ByteBuffer buffer = ByteBuffer.wrap(byteData); // Buffer
        channel.write(buffer);
        channel.close();
    }

    public void readFile(String fileName) throws Exception {
        FileChannel channel = new FileInputStream(fileName).getChannel();
        ByteBuffer buffer = ByteBuffer.allocate(1024); // 저장되는 메모리 크기 
        channel.read(buffer);
        buffer.flip(); // buffer 데이터의 가장 앞으로 이동
        while(buffer.hasRemaining()) { // 데이터가 더 남아 있는지 확인
            System.out.println((char) buffer.get());
        }
        channel.close();
    }
}

```
<br>

### Buffer
- 데이터를 담거나, 읽는 작업을 수행하면 현재의 '위치'가 이동

#### 메소드
- position() : 현재 위치
- limit() : 읽거나 쓸 수 없는 위치
- capacity() : 버퍼의 크기
```java
0 <= position <= limit <= capacity
```

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)