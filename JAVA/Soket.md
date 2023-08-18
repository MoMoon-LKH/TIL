# Socket

## Network 란
- 사용자들이 바로 옆에 있는 장비와 데이터를 주고 받는 작업

## TCP (Transmission Control Protocol)
- 연결 지향적 프로토콜
- 신뢰성을 보장
- 흐름 제어(데이터의 속도 조절이 가능하여 수신자의 버퍼 오버플로우 방지)

### 종류
- HTTP (Hypertext Transfer Protocol)
- FTP (File Transfer Protocol)
- Telnet


## UDP (User Datagram Protocol)
- 비연결형 프로토콜
- 데이터 수신 여부 확인 X
- 신뢰성이 낮다
- TCP보다 속도가 빠르다
- 1:1, 1:N, N:N 통신이 가능

<br>

## Socket
- TCP 통신을 위한 클래스

### 서버 소스
```java
public void startServer() {
        ServerSocket server = null;
        Socket client = null;

        try {
            server = new ServerSocket(9999); // 포트 9999로 ServerSocket 생성
            while (true) {
                System.out.println("Server: Waiting for request");
                client = server.accept();  // 다른 소켓 연결 대기
                System.out.println("Server: Accepted");
                InputStream stream = client.getInputStream(); // InputStream를 통해 데이터를 받음
                BufferedReader in = new BufferedReader(new InputStreamReader(stream));
                String data = null;
                StringBuilder receivedData = new StringBuilder();
                while ((data = in.readLine()) != null) {
                    receivedData.append(data);
                }
                System.out.println("Received data: " + receivedData);
                in.close();
                stream.close();
                client.close(); // 소켓 연결 종료
                if (receivedData != null && "EXIT".equals(receivedData.toString())) {
                    System.out.println("Stop SocketServer");
                    break;
                }
                System.out.println("--------------------");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (server != null) {
                try {
                    server.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

### 클라이언트 소스
```java
public void sendSocketData(String data) {
        Socket socket = null;

        try {
            System.out.println("Client: Connecting");
            socket = new Socket("127.0.0.1", 9999); // 접속할 서버 소켓 생성
            System.out.println("Client: Connect status = " + socket.isConnected());
            Thread.sleep(1000);
            OutputStream stream = socket.getOutputStream(); // 데이터를 전달하기위한 객체 생성
            BufferedOutputStream out = new BufferedOutputStream(stream); 
            byte[] bytes = data.getBytes();
            out.write(bytes); // 데이터 쓰기
            System.out.println("Client: Sent Data");
    
            stream.close();
            out.close();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (socket != null) {
                try {
                 socket.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
}
```

### 주요 메소드
#### 1. accept()
- 새로운 소켓 연결을 기다리고 연결되면 Socket 객체를 리턴

#### 2. close()
- 소켓 연결 종료

<br>


## Datagram
- UDP 통신을 위한 클래스

### 서버 소스
```java
public void startServer() {
        DatagramSocket server = null;

        try {
            server = new DatagramSocket(9999);
            int bufferLength = 256;
            byte[] buffer = new byte[bufferLength];
            DatagramPacket packet
                    = new DatagramPacket(buffer, bufferLength); 
            while (true) {
                System.out.println("Server:Waiting for request");
                server.receive(packet); // 데이터를 받기위해 대기
                int dataLength = packet.getLength();
                System.out.println("Server: received. Data length=" + dataLength);
                String data = new String(packet.getData(), 0, dataLength);
                System.out.println("Received data: " + data);
                if (data.equals("EXIT")) {
                    System.out.println("Stop DatagramServer");
                    break;
                }

                System.out.println("------------");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (server != null) {
                try {
                    server.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

### 클라이언트 소스
```java
private void sendDatagramData(String data) {
        try {
            DatagramSocket client = new DatagramSocket();
            InetAddress address = InetAddress.getByName("127.0.01"); // 데이터를 주고받을 서버의 IP를 설정
            byte[] buffer = data.getBytes();
            DatagramPacket packet =
                    new DatagramPacket(buffer, 0, buffer.length, address, 9999); // 데이터 전송위한 객체 생성
            // 서버의 주소와 port 번호를 지정하면 전송하기 위한 객체가 된다
        
            client.send(packet); // 데이터 전송
            System.out.println("Client:Sent data");
            client.close();
            Thread.sleep(1000);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
<br>

### 주요 메소드
#### 1. receive(DatagramPacket packet)
- 메소드 호출 시 요청을 대기, 데이터를 받았을 경우 packet 객체에 저장

#### 2. send(DatagramPacket packet) 
- packet 객체에 있는 데이터 전송