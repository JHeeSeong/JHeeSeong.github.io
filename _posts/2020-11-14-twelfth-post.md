---
title: "Java NIO와 멀티플렉싱 기반의 다중 접속 서버란?"
date: 2020-11-14 19:48:00 -0900
categories: NIO, 멀티플렉싱
---

## Java NIO와 멀티플렉싱 기반의 다중 접속 서버란?
### 소켓이란?

네트워크 소켓(network socket)은 컴퓨터 네트워크를 경유하는 프로세스 간 통신의 종착점이다. 오늘날 컴퓨터 간 통신의 대부분은 인터넷 프로토콜을 기반으로 하고 있으므로, 대부분의 네트워크 소켓은 인터넷 소켓이다. 네트워크 통신을 위한 프로그램들은 소켓을 생성하고, 이 소켓을 통해서 서로 데이터를 교환한다. 소켓은 RFC 147에 기술사항이 정의되어 있다.

### 인터넷 소켓은 다음과 같은 요소들로 구성되어 있다.

- 인터넷 프로토콜 (TCP, UDP, raw IP)
- 로컬 IP 주소
- 로컬 포트
- 원격 IP 주소
- 원격 포트

### 클라이언트 와 서버 소켓 연결 과정

- `socket()` : socket을 생성
- `bind()` : 서버가 사용할 IP, PORT를 생성한 소켓에 bind
- `listen()` : 클라이언트로 부터 연결 요청이 수신되는지 주시
- `accept()` : 요청이 수신되면 요청을 받아들여 데이터통신을 위한 소켓 생성
- `send()/recv()` : 데이터 송수신
- `close()` : 연결 종료(소켓 연결 끊음)

### 다중 접속 서버 구현 방법

- **멀티프로세스 기반 서버** : 다수의 프로세스를 생성하는 방식으로 서비스를 제공한다.
- **멀티스레드 기반 서버** : 클라이언트의 수만큼 스레드를 생성하는 방식으로 서비스를 제공한다.
- **멀티플렉싱 기반 서버** : 입출력 대상을 묶어서 관리하는 방식으로 서비스를 제공한다.

**자바 NIO (New IO)는 기존의 자바 IO API를 대체하기 위해 자바 1.4부터 도입이 되었습니다.** 새롭게 변화된 부분에 대해서 간략히 요약해보면 다음과 같습니다.

- **`Channels and Buffers`**기존 IO API에서는 byte streams character streams 사용했지만, NIO에서는 channels(채널)과 buffers(버퍼)를 사용합니다. 데이터는 항상 채널에서 버퍼로 읽히거나 버퍼에서 채널로 쓰여집니다.
- **`Non-blocking IO`**자바 NIO에서는 non-blocking IO를 사용할 수 있습니다. 예를 들면, 하나의 스레드는 버퍼에 데이터를 읽도록 채널에 요청할 수 있습니다. 채널이 버퍼로 데이터를 읽는 동안 스레드는 다른 작업을 수행할 수 있습니다. 데이터가 채널에서 버퍼로 읽어지면, 스레드는 해당 버퍼를 이용한 processing(처리)를 계속 할 수 있습니다. 데이터를 채널에 쓰는 경우도 non-blocking이 가능합니다.
- **`Selectors`**자바 NIO에는 “selectors” 개념을 포함하고 있습니다. selector는 여러개의 채널에서 이벤트(연결이 생성됨, 데이터가 도착함)를 모니터링할 수 있는 객체입니다. 그래서 하나의 스레드에서 여러 채널에 대해 모니터링이 가능합니다.

# Channels

**일반적으로 NIO의 모든 IO는 채널로 시작합니다. 채널 데이터를 버퍼로 읽을 수 있고, 버퍼에서 채널로 데이터를 쓸 수 있습니다.**

채널은 스트림(stream)과 유사하지만 몇가지 차이점이 있습니다.

- 채널을 통해서는 읽고 쓸 수 있지만, 스트림은 일반적으로 단방향(읽기 혹은 쓰기)으로만 가능합니다.
- 채널은 비동기적(asynchronously)으로 읽고 쓸 수 있습니다.
- 채널은 항상 버퍼에서 부터 읽거나 버퍼로 씁니다.

Channel Type

- `FileChannel`: 파일에 데이터를 읽고 쓴다. (selector 사용 불가능)
- `DatagramChannel`: UDP를 이용해 네트워크를 통해 데이터를 읽고 쓴다.
- **`SocketChannel`**: TCP를 이용해 네트워크를 통해 데이터를 읽고 쓴다.
- **`ServerSocketChannel`**: 들어오는 TCP 연결을 수신(listening)할 수 있다. 들어오는 연결마다 SocketChannel이 만들어진다.

# Buffers

**NIO의 버퍼는 채널과 상호작용할 때 사용됩니다. 데이터는 채널에서 버퍼로 읽혀지거나, 버퍼에서 읽혀 채널로 쓰여집니다.**

일반적으로 버퍼를 사용하여 데이터를 읽고 쓰는 것은 4단계 프로세스를 가집니다.

1. 버퍼에 데이터 쓰기
2. buffer.flip() 호출(읽기 모드 전환)
3. 버퍼에서 데이터 읽기
4. buffer.clear() 혹은 buffer.compact() 호출

# Selectors

**셀렉터를 사용하면 하나의 스레드가 여러 채널을 처리(handle)할 수 있습니다.**

셀렉터는 사용을 위해 하나 이상의 채널을 셀렉터에 등록하고 select() 메서드를 호출해 등록 된 채널 중 이벤트 준비가 완료된 하나 이상의 채널이 생길 때까지 봉쇄(block)됩니다. 메서드가 반환(return)되면 스레드는 채널에 준비 완료된 이벤트를 처리할 수 있습니다. 즉, 하나의 스레드에서 여러 채널을 관리할 수 있으므로 여러 네트워크 연결을 관리할 수 있습니다. (SocketChannel, ServerSocketChannel)

```java
selector = Selector.open(); //셀렉터를 생성
socketChannel = SocketChannel.open(new InetSocketAddress("localhost", 3000));
socketChannel.configureBlocking(false); // 블로킹 방식 false
socketChannel.register(selector, SelectionKey.OP_READ);
//생성한 셀렉터에 채널 등록(채널은 non-blocking mode 여야함)
//셀렉터를 통해 채널에서 발생하는 이벤트 중 확인(알림)하고자 하는 이벤트의 집합을 의미
```

```java
while (true) {
		ClientSimulatorMain.selector.select();
		Iterator iterator = ClientSimulatorMain.selector.selectedKeys().iterator();

		while (iterator.hasNext()) {
			SelectionKey key = (SelectionKey) iterator.next();
			
			if (key.isReadable())
			read(key);
			
			iterator.remove();
		}
}
```


출처 : [https://jongmin92.github.io/2019/03/03/Java/java-nio/](https://jongmin92.github.io/2019/03/03/Java/java-nio/)