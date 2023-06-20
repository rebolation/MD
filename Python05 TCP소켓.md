---
date: 2021.10.18
title: 파이썬 TCP소켓
tag: 
---

# 파이썬 TCP소켓

### 헷갈리기 쉬운 개념들

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20211019134530749.png" alt="image-20211019134530749" style="zoom:67%;" />

##### 동시성/병렬성, 프로세스/스레드/코루틴

- 동시성 : 물리적 동시 실행 아님. A를 일부 처리 -> B 일부 -> A 일부 ... (예: 싱글코어에서 멀티태스킹)
- 병렬성 : 물리적 동시 실행. 문자 그대로 병렬로 여러 작업을 함께 처리 (예: 멀티코어, 분산컴퓨팅)
- 프로세스 : 프로그램을 실행하면 프로세스가 되어 메모리(코드/데이터/스택/힙)를 할당받음
- 스레드 : 프로세스 내의 실행 단위. 자신만의 스택 소유. 프로세스의 코드/데이터/힙 공유. OS 스케줄러가 관리
- 코루틴 : 함수 일시중지와 실행을 반복하여 동시성 실현. 스레드보다 스위칭 비용 적음. 프로그래머가 관리

##### 동기/비동기, 블로킹/논블로킹

X함수가 Y함수를 호출하였을 때, [관련 글](https://stackoverflow.com/a/31298006)

- 동기(sync) : Y의 결과가 나올 때까지 X는 기다린다. **X와 Y는 동기식이다.**
- 비동기(async) : **Y가 부르기 전까지** X는 다른 일을 한다. **X와 Y는 비동기식이다.**
- 블로킹(blocking) : Y의 결과가 나올 때까지 X는 기다린다. **X는 블록되었다.**
- 논블로킹(non-blocking) : Y의 결과가 나오기 전에도 X는 다른 일을 한다. **X는 블록되지 않았다.**
  - X는 Y의 결과가 나왔는지 **1초마다 확인**할 수도 있고...
  - X는 **Y가 부르기 전**까지는 아예 관심을 끌 수도 있고...

### 소켓, FD, SD

##### 소켓

- TCP나 UDP 같은 계층을 이용하는 API
- 두 컴퓨터가 통신할 때 엔드포인트(호스트와 포트로 표현되는 식별 값)
- 하나의 소켓은 다른 하나의 소켓과 일대일로만 연결할 수 있음(클라이언트 소켓)

##### FD(File Descriptor)

- 프로그램에서 파일을 열면 시스템은 파일 정보가 담긴 구조체를 파일 디스크립터 테이블이라는 곳에 보관
- 파일 디스크립터 테이블의 인덱스 값이 파일 디스크립터임 (예:FD=650)
- 리눅스에서 일반적으로 최대값이 1024로 설정되어 있으며 수정할 수 있다고 함
- 윈도우에서는 File Handle 이라고 부름

##### SD(Socket Descriptor)

- 리눅스에서 하드웨어 장치, 파이프, 소켓 등도 모두 파일로 간주
- 소켓을 가리키는 파일 디스크립터를 소켓 디스크립터라고 함
- 소켓 프로그램 작성 시 FD라는 표현은 SD를 의미

### 서버 소켓과 클라이언트 소켓

##### 클라이언트측

- 하나의 **클라이언트 소켓** : 서버에 접속하고 통신하는 용도

```python
import socket

# 소켓을 생성 : INET(IPv4), STREAMing(TCP)
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# "www.python.org" 호스트의 80 포트에 접속
s.connect(("www.python.org", 80)) # 결과로 서버측에 클라이언트 소켓이 만들어진다
```

##### 서버측

- 하나의 **서버 소켓** : 클라이언트들의 최초 접속을 받아들이고 클라이언트 소켓을 생성하는 용도
- 여러 개의 **클라이언트 소켓** : 클라이언트와 통신하는 용도. 클라이언트측의 소켓과 동일한 성격

```python
import socket

# 소켓을 생성 : INET(IPv4), STREAMing(TCP)
serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 소켓을 퍼블릭호스트(외부에서 볼수 있음)의 80포트에 바인딩
# 호스트명 
#  - socket.gethostname() : 소켓을 외부 세계에서 볼 수 있음
#  - 'localhost' 또는 '127.0.0.1' : 소켓을 같은 기계 내에서만 볼 수 있음
#  - '' : 시스템에 있는 모든 주소로 소켓에 연결할 수 있음
serversocket.bind((socket.gethostname(), 80))
# 이제 서버 소켓이 됨
# 외부 연결을 거부하기 전에 최대 5개의 연결 요청을 큐에 넣기를 원한다는 것을 소켓 라이브러리에 알림
serversocket.listen(5)

# 서버 메인 루프
while True:
    # 외부로부터 접속을 받아들임
    (clientsocket, address) = serversocket.accept()
    # 클라이언트 소켓으로 뭔가를 함: 여기서는 멀티스레드 서버라고 가정
    ct = client_thread(clientsocket)
    ct.run()
```

### 소켓 버퍼(커널 버퍼)

##### 특징

- 구조 : 선입선출(FIFO) 방식으로 동작하는 **큐 형태의 바이트 배열**
- 소켓 버퍼 운영에는 **OS가 개입**함. 소켓 버퍼를 커널 버퍼라고도 부름
  - 송신 : 데이터 전송 요청(send) → OS는 데이터를 커널 송신 버퍼에 복사하고 차례대로 네트워크로 보냄
  - 수신 : OS가 네트워크로부터 들어오는 데이터를 커널 수신 버퍼에 복사 → 데이터를 차례대로 꺼냄(recv)

##### 버퍼와 send, recv 동작

- TCP 소켓의 경우 버퍼 상태에 따라 **블로킹**이 발생할 수 있음
  - 송신 버퍼가 가득 찬 상태에서 send()를 호출하면 블로킹 발생
  - 수신 버퍼가 비어 있는 상태에서 recv()를 호출하면 블로킹 발생
  - 수신측의 수신 버퍼가 가득 찬 상태에서 송신측의 send()에 블로킹 발생
- UDP 소켓의 경우 블로킹 대신 **데이터그램 유실**이 발생할 수 있음
- 소켓 버퍼 크기의 기본값은 65,536 bytes, 최대값은 2,147,483,647 bytes

```python
import socket

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind(('localhost', 8000))
    s.listen()
    conn, addr = s.accept()
    print(conn.getsockopt(socket.SOL_SOCKET, socket.SO_SNDBUF)) # 65536
    print(conn.getsockopt(socket.SOL_SOCKET, socket.SO_RCVBUF)) # 65536

    conn.setsockopt(socket.SOL_SOCKET, socket.SO_SNDBUF, 2147483647)
    conn.setsockopt(socket.SOL_SOCKET, socket.SO_RCVBUF, 2147483647)
```

### 블로킹 소켓과 논블로킹 소켓

##### 블로킹 소켓

- ##### 논블로킹 소켓

- 실행 즉시 무조건 리턴

### 데이터 전송과 데이터 경계

### IO 다중화(멀티플렉싱)

- IO 다중화란?
  
  - 싱글 스레드에서... ... ??? ... ???

- select 모듈
  
  - select.select를 통해 소켓 리스트 중 변화가 발생한 소켓을 찾아 해당 소켓과 통신
  - 소켓 리스트에는 서버 소켓이 포함되어 있어 새로운 클라이언트 접속을 감지하여 소켓 리스트에 추가
  - 소켓 리스트의 모든 소켓(서버 소켓1 + 클라이언트 소켓n)을 지속적으로 스캔

- selectors 모듈
  
  - 단일스레드+비동기 소켓 : 다중 접속 서버 구현 시 멀티스레드+동기 소켓 조합에 대한 대안
  - DefaultSelector는 현재 운영체제에서 최적의 셀렉터를 반환. 운영체제에 따라 지원 셀렉터 종류가 다름
    - SelectSelector, PollSelector, EpollSelector, DevpollSelector, KqueueSelector
  - 
  
  ```python
  # Lib/socketserver.py
  with _ServerSelector() as selector: # 셀렉터 생성
    selector.register(self, selectors.EVENT_READ) # 셀렉터에 self(서버)와 EVENT_READ 등록
  
    while not self.__shutdown_request:
      ready = selector.select(poll_interval) # 변경된 FD를 찾는다
      # bpo-35017: shutdown() called during select(), exit immediately.
      if self.__shutdown_request:
        break
        if ready:
          self._handle_request_noblock()
  
          self.service_actions()
  ```

### 참고

- [프로세스와 스레드의 차이](https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html)
- [소켓(Socket) 정의](https://luckyyowu.tistory.com/71)
- [소켓 프로그래밍 HOWTO](https://docs.python.org/ko/3/howto/sockets.html)
- [socket - 저수준 네트워킹 인터페이스](https://docs.python.org/ko/3/library/socket.html)
- [서버의 기본 동작 방식 1](https://uiandwe.tistory.com/1240)
- [서버의 기본 동작 방식 2](https://uiandwe.tistory.com/1243)
- [selector를 사용한 소켓 멀티플렉싱](https://soooprmx.com/selector%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-%EC%86%8C%EC%BC%93-%EB%A9%80%ED%8B%B0%ED%94%8C%EB%A0%89%EC%8B%B1/)
- [Understanding Non Blocking I/O with Python — Part 1](https://medium.com/vaidikkapoor/understanding-non-blocking-i-o-with-python-part-1-ec31a2e2db9b)
- 
