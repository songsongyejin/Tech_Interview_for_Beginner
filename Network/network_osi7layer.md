# OSI 7 Layer
## 개요
- Open Systems Interconnection Reference Model
- 국제표준화기구(ISO)에서 개발한 모델
- 네트워크 프로토콜이 통신하는 구조를 7개의 계층으로 분리하여 각 계층간 상호 작동하는 방식을 정해 놓은 것
- 프로토콜을 `기능`별로 나눈 것 
    - 각 계층은 하위 계층의 기능만을 이용하고, 상위 계층에게 기능을 제공한다. 
    - '프로토콜 스택' 혹은 '스택'은 이러한 계층들로 구성되는 프로토콜 시스템이 구현된 시스템을 가리키는데, 프로토콜 스택은 하드웨어나 소프트웨어 혹은 둘의 혼합으로 구현될 수 있다. 
    - 일반적으로 하위 계층들은 하드웨어로, 상위 계층들은 소프트웨어로 구현된다.
- 계층을 나눈 이유 : 
1. 표준화를 통하여 장비별로 이질적인 포트, 프로토콜의 호환 문제를 해결
2. 통신이 일어나는 과정을 단계별로 파악하기 위함.<br>
( 7단계 중 특정한 곳에 이상이 생기면 다른 단계의 장비 및 소프트웨어를 건들이지 않고도 이상이 생긴 단계만 고칠 수 있기 때문이다.)
## 특징
- 데이터 캡슐화
<img src="img\network_osi7layer_data_encapsulation.jpg"/>
<br><br>
    - 사용자 데이터가 각 계층을 지나면서 하위 계층은 상위 계층으로부터 온 정보를 데이터로 취급하며, 자신의 계층 특성을 담은 제어정보(주소, 에러 제어 등)를 헤더화 시켜 붙이는 일련의 과정
    - 데이터를 보낼 때는 응용 계층에서 시작되어 OSI 계층을 차례로 내려오며 물리 계층으로 간다. 이 과정에서 캡슐화를 하게 되는데 각 계층은 다른 계층과 통신할 때 데이터에 특정 정보가 들어 있는 머리말(헤더)과 꼬리말(푸터)을 추가한 후 다른 계층으로 전달
    - `PDU(Protocol Data Unit)`은 프로토콜 데이터 단위이며 OSI 모델의 정보 처리 단위
    - 아래 계층으로 내려갈수록 PDU에는 다양한 프로토콜에 의해 헤더와 푸터가 더해진다

## 기능

### 1 계층 : 물리 계층 (Physical Layer)
- 데이터는 0과 1의 비트열로 ON, OFF의 전기적 신호 상태
-  전압, 허브, 네트워크 어댑터, 중계기 및 케이블 사양을 비롯해 사용된 모든 하드웨어의 물리적 및 전기적 특성을 정의
- 디지털에서 아날로그로 또는 그 반대로 신호를 변환하는 역할
- 단위(PDU) : Bit
- 예시 : 통신케이블, 리피터, 허브

<img src="img\network_osi7layer_1layer.jpeg"/>

### 2 계층 : 데이터 링크 계층(DataLink Layer)
- `프레임에 주소부여(MAC - 물리적주소)`
-  맥 주소를 가지고 통신
- 네트워크 위의 개체들 간 데이터를 전달하고, 물리 계층에서 발생할 수 있는 오류를 찾아 내고, 수정하는 데 필요한 기능적, 절차적 수단을 제공
- 1홉 통신을 담당한다고도 말한다. 홉(hop)은 컴퓨터 네트워크에서 노드에서 다음 노드로 가는 경로
- 1홉 통신이면 한 라우터에서 그다음 라우터까지의 경로
- 계층에서 전송되는 단위를 `프레임`이라고 한다.
- 포인트 투 포인트(Point to Point) 간 신뢰성있는 전송을 보장하기 위한 계층으로 CRC 기반의 오류 제어와 흐름 제어
> **CRC (Cyclic Redundancy Check)** 는 데이터 전송 오류를 감지하는 데 널리 사용되는 방식입니다. 데이터를 전송하기 전에 송신기는 데이터 블록(패킷이나 프레임의 본문)에 대한 CRC 체크섬 값을 계산하고, 이 값을 데이터 블록 끝에 추가합니다. 체크섬은 데이터에 대한 간단한 해시 값이나 요약이라고 생각할 수 있으며, 특정 다항식을 사용하여 계산됩니다.<br><br>
수신기에서는 동일한 CRC 다항식을 사용하여 수신된 데이터 블록의 체크섬을 다시 계산하고, 송신기가 전송한 체크섬과 비교합니다. 만약 체크섬이 일치하지 않으면, 데이터 내에 오류가 발생했다고 판단하고, 오류가 발생했다는 신호를 송신기에 보내거나 데이터를 버립니다. CRC는 일반적으로 물리 계층이나 데이터 링크 계층에서 사용됩니다.
- 단위(PDU) : Frame
- 예시 :  브릿지 및 스위치 , 이더넷 등

> 라우터는 IP 주소를 통해 경로를 찾는 장비이고, 스위치는 MAC 주소를 통해 data를 해당 장비에 전송하는 장비이다.<br><br>
라우터는 네트워크 주소가 서로 다른 장비들을 연결할 때 사용하고, 원격지에 위치한 네트워크들을 연결하는 경우가 많다.<br>
스위치는 MAC 주소와 포트 번호가 기록된 MAC 주소 테이블을 가지고 이 테이블을 통해 data를 전송할 기기를 찾는다.

<img src = "img\network_osi7layer_2layer.jpeg">

### 3 계층 : 네트워크 계층 (Network Layer)
- 2홉 이상의 통신(멀티 홉 통신)을 담당
- 실제 네트워크 간에 데이터 `라우팅`을 담당
> 라우팅이란 어떤 네트워크 안에서 통신 데이터를 짜여진 알고리즘에 의해 최대한 빠르게 보낼 최적의 경로를 선택하는 과정. <br> 목적지의 주소= IP
- 여러 개의 노드를 거칠 때마다 경로를 찾아주는 역할을 하는 계층으로서 다양한 길이의 데이터를 네트워크들을 통해 전달하고 그 과정에서 전송 계층이 요구하는 서비스 품질을 제공하기 위한 기능적, 절차적 수단을 제공
- 데이터를 연결하는 다른 네트워크를 통해 전달함으로써 인터넷이 가능하게 만드는 계층
- 네트워크 계층은 `라우팅`, `주소 부여 (IP)`,  흐름 제어, 세그멘테이션, 오류제어, 인터네트워킹 등을 수행
- 단위(PDU) : Packet
- 예시 : 라우터, 3계층에서 동작하는 스위치

### 4계층  : 전송 계층 (Transport Layer)
- 신뢰성있는 데이터를 주고 받을 수 있도록 해 주어, 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다
- 시퀀스 넘버 기반의 오류 제어 방식
> 각 데이터 패킷에 고유한 번호를 할당하고, 이를 통해 수신자 측에서 패킷의 순서를 추적하고 분실이나 중복 등의 오류를 감지하는 데 사용<br>시퀀스 넘버를 사용하는 오류 제어 방식에는 주로 두 가지:<br> 
> 1. 자동 반복 요청 (Automatic Repeat reQuest, ARQ):
정지-대기 (Stop-and-Wait) ARQ: 송신자가 한 번에 하나의 패킷만을 전송하고, 해당 패킷에 대한 수신자의 확인응답(ACK) 또는 부정응답(NACK)을 기다립니다. ACK가 도착하면 다음 패킷을 전송하고, NACK가 도착하거나 응답이 없으면 같은 패킷을 재전송합니다.<br>
연속 ARQ (Go-Back-N): 송신자가 여러 개의 패킷을 연속적으로 전송할 수 있지만, 오류가 감지되면 오류가 발생한 패킷과 그 이후에 전송된 모든 패킷들을 재전송합니다.
선택적 반복 (Selective Repeat) ARQ: 송신자가 여러 개의 패킷을 연속적으로 전송할 수 있고, 오류가 감지된 특정 패킷만을 재전송합니다.<br> 
> 2. 슬라이딩 윈도우 프로토콜 (Sliding Window Protocol):<br> 이 방식에서는 송신자와 수신자 양쪽에서 '윈도우'라고 하는 패킷의 프레임을 유지합니다. 송신 윈도우는 전송할 수 있는 패킷의 범위를 나타내며, 수신 윈도우는 수신을 허용할 패킷의 범위를 나타냅니다.<br>
각 패킷에는 시퀀스 넘버가 할당되어 있고, 수신자는 이 넘버를 기반으로 패킷을 재조립하고 순서가 뒤바뀌거나 분실된 패킷을 감지합니다.<br>
수신자는 수신한 패킷의 시퀀스 넘버를 기반으로 ACK를 전송하며, 송신자는 이를 통해 어떤 패킷들이 성공적으로 수신되었는지 파악할 수 있습니다.<br>
- 포트 번호를 통해 도착지 컴퓨터의 최종 도착지인 프로세스까지 데이터가 도달하게 한다.
- 가장 잘 알려진 전송 계층의 예는 TCP / UDP
- 단위(PDU) : TCP-Segment, UDP-datagram
- 예시 : 특정 방화벽이나 프록시 서버

### 5계층  : 세션 계층 (Session Layer)
- 두 컴퓨터 간의 대화나 세션을 관리하며, `포트(Port)연결`이라고도 한다
-  TCP/IP 세션을 만들고 없애고 통신하는 사용자들을 동기화하고 오류 복구 명령들을 일괄적으로 다루며 통신을 하기 위한 `세션을 확립, 유지, 중단하는 작업`을 수행
- 모든 통신 장치 간에 연결을 설정하고 관리 및 종료하고 또한 연결이 전이중(Full duplex / 양방향)인지 반이중(half duplex / 단방향)인지 여부를 확인하고 체크 포인팅과 유휴, 재시작 과정 등을 수행하며 호스트가 갑자기 중지되지 않고 정상적으로 호스트를 연결하는 데 책임.

> 전이중(Full Duplex):통신 채널에서 양방향 통신이 동시에 발생할 수 있음을 의미합니다. 즉, 전송과 수신이 동시에 이루어질 수 있습니다. 예를 들어, 현대의 전화 통화는 전이중 통신입니다. 한 사람이 말하는 동안 다른 사람도 동시에 말할 수 있습니다.
반이중(Half Duplex): 이는 통신 채널에서 양방향 통신이 가능하지만, 한 번에 한 방향으로만 데이터를 전송할 수 있음을 의미합니다. 즉, 한 장치가 전송할 때 다른 장치는 수신만 할 수 있고, 전송이 끝난 후에 방향을 바꿔 다른 장치가 전송을 시작할 수 있습니다. 무전기가 이에 해당합니다.

- 단위(PDU) : Data


### 6계층  : 표현 계층 (Presentation Layer)
- 사용자의 명령어를 완성 및 결과 표현
- 전달받은 데이터를 읽을 수 있는 형식으로 변환 
- 응용 계층으로부터 전송받거나 응용 계층으로 전달해야 할 데이터의 인코딩과 디코딩
- 데이터를 안전하게 사용하기 위해서 암호화와 복호화를 하는데 이 작업도 표현 계층에서 이루어진다
- 단위(PDU) : Data

### 7계층  : 응용 계층 (Application Layer)
- 네트워크 소프트웨어 UI 부분
- 사용자의 입출력(I/O)부분
- 예시 :  예를 들면 가상 터미널인 텔넷(telnet), 구글의 크롬(chrome), 이메일(전자우편), 데이터베이스 관리 등의 서비스
- 단위(PDU) : Data

## 비교
<img src="img\network_osi7layer_comparison.jpg">

출처: 
https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95 <br>
https://wiki.hash.kr/index.php/OSI_7_%EA%B3%84%EC%B8%B5#n<br>
https://shlee0882.tistory.com/110<br>
https://onecoin-life.com/19