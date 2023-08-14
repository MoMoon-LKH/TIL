# I/O 
- Input / Output의 약자
- 바이트 기반의 데이터를 처리하기 위해서 여러 종류의 스트림 클래스 제공 <br>
  - 스트림: 연속적인 데이터

<br>

## File, Files Class
### 1) File
- 파일 및 경로 정보를 통제하기 위한 클래스
- 객체 생성해서 데이터 처리

```java
public static void main(String[] args) {
    File file = new File("C:\\godofjava", "text.txt");
    try {
        System.out.println(file.createNewFile()); // 파일 생성
        
        System.out.println(file.getName()); // 파일 이름
        System.out.println(file.getPath()); // 파일 경로
        
        
    } catch(IOException e){
        e.printStackTrace;
    }
}
```
<br>

#### 1) mkdir과 mkdirs -> s의 차이?
- mkdir는 하나의 폴더만, mkdirs는 여러 개의 하위 폴더를 만들 수 있다.
```java
  File file = new File("C:\\godofjava\\d\\f");
  file.mkdir(); // d만 생성
  file.mkdirs(); // d/f 생성
```
<br>

#### 2) Absolute와 Canonical 경로 차이
Canonical는 절대적으로 유일하게 표현할 수 있는 경로
```java
ex) C:\godofjava\a -> ..\b
Absolute = C:\godofjava\a\..\b
Canonical = C:\godofjava\b
```
<br>

#### 3) FileFilter, FilenameFilter Interface
- 목록을 가져올 때 필터를 지정하는 생성자를 포함하여 필요한 파일만 가져올 수 있는 기능을 제공하는 인터페이스
```java
public class JPGFileFilter implements FileFilter {
    
    @Override
    public boolean accept(File file) {
        if(file.isFile()) {
            String fileName = file.getName();
            if(fileName.endsWith(".jpg")) return true;
        }
        return false;
    }
}


public class JPGFileFilter implements FilenameFilter {

  @Override
  public boolean accept(File file, String fileName) {
    if(fileName.endsWith(".jpg")) return true;
    return false;
  }
}
```
- FilenameFilter는 디렉터리와 파일 이름을 구분하지 못한다
  - 예를 들어 text.jpg라는 디렉터리가 있으면 해당 디렉터리도 같이 불러온다는 의미
- 디렉터리와 파일를 구분해야된다면 FileFilter Interface 사용을 권장

### 2) Files
- File과 동일하나 모든 메소드가 static으로 선언

<br>


## InputStream, OutputStream
- abstract 클래스를 통해 제공
- Closeable = 해당 인터페이스 구현하면 close 메소드로 무조건 닫아야된다
- Flushable = buffer를 통해 데이터를 저장해놓고 flush 메소드를 통해 한번에 저장
- 객체로 생성하여 사용 -> 생성자는 protected로 선언 되어 있어서 상속받은 클래스에서 객체 생성이 가능
```java
public abstract class OutputStream extends Object implements Closeable, Flushable
```


### 중요 메서드
#### 1) read()
- 스트림에서 다음 바이트를 읽는다.
- 유일한 abstract 메소드

#### 2) close()
- 스트림 작업중인 대상을 해제

<br>

## Reader, Writer
- char 기반의 문자열을 처리 하기 위한 클래스
```java
public abstract class Reader extends Object implements Readable, Closeable
```

<br>

## FileWriter
- char 기반의 내용을 파일로 쓰기 위한 클래스
```java
FileWriter writer = null;
BufferedWriter buffered = null;

try{
  fileWriter=new FileWriter(fileName);
  buffered=new BufferedWriter(writer);
  
  bufferd.write(2);
  bufferdWriter.newLine();
} catch(IOException io){
  io.printStackTrace();  
} finally {
    if(buffered != null) {
        try{
            buffered.close();
        } catch(IOException io){
            io.printStackTrace();
        }
    }
    if(writer != null) {
      try{
        writer.close();
      } catch(IOException io){
        io.printStackTrace();
      }
    }
}
```

<br>

## Scanner
- 텍스트 기반의 기본 자료형이나 문자열 데이터를 처리하기 위한 클래스
- 정규 표현식 사용이 가능하다
```java
File file = new File(fileName);
Scanner scanner = null;
try{
    scanner = new Scanner(file);
    ...
    
} catch(FileNotFoundException fnfe) {
    e.printStackTrace();
} finally{
        if(sacnner!=null)
        scanner.close();
}
```