# CH3. 네트워크의 이해


## 1. 네트워크의 기초

### **1.1. OSI 7계층**
- 컴퓨터 네트워크 프로토콜 디자인과 통신을 7계층으로 나누어 정의한 것으로 ISO에서 개발
- 각 계층은 독립적이며 하위계층의 기능을 이용하고 상위계층에게 기능을 제공한다.
- 계층별 특징
    - **L7: Application Layer**
        - 응용프로그램과 연계하여 사용자와 직접 상호작용
        - 인터넷상 전송단위: data
        - Chrome, Firefox, .... / SMTP, HTTP, FTP, ...
    - **L6: Presentation Layer**
        - 서로 다른 표현방식을 사용하는 송수신자가 서로의 데이터를 이해할 수 있도록 번역하는 서비스
        - 인터넷상 전송단위: data 
        - SMB, AFP, ...
    - **L5: Seesion Layer**
        - 응용프로그램 간의 접속 설정 및 유지, 동기화 유지 서비스를 통해 오류발생시 대처가능하게 해줌
        - 인터넷상 전송단위: data
        - SSL, TLS, ...
    - **L4: Transport Layer**
        - 데이터 전송 관련 서비스. 송수신측 사이의 연결설정, 오류 복구, 흐름제어 등의 서비스
        - 인터넷상 전송단위: segment
        - TCP, UDP, ...
    - **L3: Network Layer**
        - 데이터가 목적지까지 최적의 경로를 통해 전송될 수 있도록 데이터 전송과 경로 선택에 관한 서비스 제공
        - 인터넷상 전송단위: packet
        - 라우터 / IP, ICMP, ...
    - **L2: Data Link Layer**
        - L3로부터 받은 데이터에 헤더에 전송, 순서, 에러, 흐름을 제어하기 위한 정보를 담아 L1으로 전달
        - 인터넷상 전송단위: frame
        - 브릿지, 스위치 / Ethernet, Token Ring, ...
    - **L1: Physical Layer**
        - 실제 장치들을 연결. 케이블,연결장치
        - 인터넷상 전송단위: bit(signal)
        - 허브, 리피터 / RS-232,10BASE-T, ...

### **1.2. 네트워크 장비**
- LAN 구성 장비
    - **Network Card** 
        - = Network Interface Card = Ethernet Card = LAN Card = Network Adaptor
        - 네트워크안에서 컴퓨터끼리 통신하는데 쓰이는 하드웨어로 MAC 주소 할당시스템을 이용하여 physical layer, data link layer를 사용한다
    - **Cable**
        - 신호를 전송하는 물리적인 매체로 BNC, UTP, 광섬유 등이 있다.
        - BNC(동축 케이블)
            - noise에 강함 
            - 데역폭이 넓어 여러채널을 동시 수용가능
            - 굵음, 빠름, 비쌈
            - 연결되는 컴퓨터는 하나의 선으로 계속 연결 가능
            - 터미네이터 설치 필요 / 허브 불필요
            - 컴터 하나의 문제가 전체에 영향줄 수 있음 -> 안전하지 X
        - Twisted Pair
            - 꼬인 형태: 전자 신호 변형 방지
            - noise 취약, 저렴
            - 단독연결시 100m까지
            - UTP(Unshielded)
                - 차폐물 없음. 싸서 많이 사용. 8개 회선 있음
                - 종류는 대역폭과 전송속도에 따라 3 - 5 - 5 - 5e - 6 - 6e - 7
            - STP(Shielded): 차폐물 있음
            - FTP(Foil Screened): 은박 차폐물
            - 허브 사용 -> 컴퓨터 하나의 문제와 다른 컴퓨터는 독립
        - 광섬유
            -  가는 유리, 플라스틱 섬유
            - noise에 강하고 오류 발생이 적음
            - 작고 가벼움, 넓은 대역폭, 빠름
            - 비쌈
    - **Hub**
        - Ethernet network에서 여러 대의 컴퓨터 및 네트워크 장비를 연결하는 장치
        - 같은 허브에 연결된 장치는 모두 상호 간에 통신 가능
- Internetworking 장비
    - 네트워크와 네트워크의 연결을 internetworking이라 하며 이를 수행하는 장치를 일반적으로 Gateway라 부른다. 
    - Gateway는 서로 다른 통신망과 프로토콜을 사용하는 네트워크간에 통신을 가능하도록 하며 대표적으로 repeater, bridge, router 등이 있다. 
    - **Repeater**
        - L1에서 동작하며 먼 거리를 전송하면서 감쇄되는 신호를 증폭시켜 보완해준다.
    - **Bridge**
        - L2에서 리피터나 허브와 같은 기능을 하며 특정 네트워크로부터의 통신량 조절
    - **Router**
        - L1-L3의 기능을 지원. 브리지의 기능에 추가하여 L3에 대한 경로 선택 기능 제공


### **1.3. Ethernet/LAN의 기본 이해**
- 통신망의 종류는 규모에 따라 LAN, MAN, WAN 정도로 나뉜다
- **LAN (Local Area Network)**
	- 근거리. 집, 사무실, PC방 정도
	- 주로 Ethernet, WLAN 방식 사용.
	- 구성방식(topology)
		- Star형: 중앙 컴퓨터가 각 컴퓨터와 연결되어 통신한다. 중앙 컴퓨터 고장시 전체 중단
		- Bus형: 하나의 통신회선(bus)에 여러 컴퓨터를 연결하며 한번에 한 컴퓨터만 전송 가능 
		- Ring형: 원형의 통신회선에 컴퓨터와 단말기를 연결. 토큰 패싱 사용. 전송 충돌 없음
		- Mesh형: Star + Ring 형태. 라우터 이용하여 LAN들을 연결하거나 백본망 구성시 사용
	- 전송방식
		- Ethernet, CSMA/CD
			- CSMA: 회선을 체크하여 사용중이지 않으면 data 전송
			- CD: Collision Detection
			- MAC 주소를 기반으로 상호간 data 교환
			- 전송 매체:BNC, UTP, STP
			- 기기 간의 상호 연결: Hub, Switch, Repeater
		- Token Ring
			- Ring형 구성방식. 
			- Token이라는 일종의 사용권을 획득해야 data를 전송가능
		- FDDI
			- 광섬유 케이블을 사용하여 설계된 Ring 구조의 통신망
			- 이중 링 구조를 사용하여 한꺼번에 단절되는 경우 방지
- **MAN (Metropolitan Area Network)**
	- 도시 하나 정도의 규모로 도시 내의 여러 LAN을 묶어 놓은 형태
	- LAN보다 장거리이며 고속이고 음성과 데이터 모두 전송이 가능하다 
- **WAN (Wide Area Network)**
	- 국가, 대륙 등과 같은 넓은 지역을 연결하는 네트워크 
	- 거리제약 없음
	- LAN보다 느림, 전송 에러율 높음,
	- 구성방식
		- 전용회선 계약한 송수신 사용자끼리만 전용 통신 선로로 연결하기 떄문에 안전하지만 비쌈
		- 교환회선: PSTN, PSDN과 같은 공중망을 이용해 전송하는 방식으로 싸지만 느림. Circuit Switching, Packet Switching, ATM, 등이 있다.
	- Circuit Switching
		- 송수신자간의 물리적인 경로, 할당받을 대역폭을 미리 결정, 고정
		- 안정적이지만 효율적이지는 않다. 
	- Packet Switching
		- 전송 단위가 packet임
		- 전송 대역폭을 동적인 방식으로 공유
		- Datagram 방식: 연결 설정 과정이 없기 때문에 패킷들이 각기 다른 경로로 독립적으로 전송. 순서고 제각각
		- Virtual Circuit 방식: 송수신자간 가상의 파이프를 통해 모든 패킷이 동일한 경로로 전송. 순서도 그대로

#### 컴퓨터 통신망 발전
> 전화망 -> Telnet, Tymnet: 고품질의 데이터 서비스 제공 하기 위해 설치된 패킷 교환망 -> 장비들간 호환성 문제 -> 전화회사들을 중심으로 표준 프로토콜 제정 -> X.25

- **X.25**
	- 데이터 -> 패킷단위로 분할 -> 전송 -> 패킷 composition -> 수신자
	- 에러에 취약해서 에러 검출, 복구 기능을 탑재해야 했고 빠른 전송속도를 기대할 수는 없었다.
	- Packet Switching 기반
	- 1, 2, 3 계층에서의 처리
	- 고정된 대역폭을 가짐
- **Frame Relay**
	- X.25을 계승하고 단순화하여 만들어짐
	- 이제 망의 신뢰도가 높아졌으니 에러제어와 흐름제어 기능은 단말에서 담당하는 것으로 하고 우리는 전송(relay)기능만 하자 하여 나온 것
	- Virtual Circuit 기반
	- Frame 단위로 전송하며 Frame 은 길이가 변할 수 있는 단위이다.
	- Statistical Multiplexing 사용하여 대역폭을 동적으로 공유
	- 대역폭 절약, X.25 대비 낮아진 전송지연
- **Cell Relay**
	- ATM(Asynchronous Transfer Mode)라고도 한다.
	- Frame Realy에서 발전된 형태이다.
	- Asynchronous Time Multiplexing, 고속 패킷 교환 지원
	- 전송단위는 고정 길이를 갖는 패킷인 Cell이다
	- 고정 길이의 패킷을 사용함으로 가변적인 Frame을 사용할 때보다 오버헤드를 줄일 수 있있어 전송률이 빨라진다.

### **1.4. TCP/IP 및 네트워크 프로토콜의 이해**
- **Protocol의 개요**
	- 컴퓨터들이 서로 통신을 하기 위해 미리 정해놓은 통신 규칙.
	- 기본 구성 요소
		- Syntax: 전송할 data의 구조, 형태에 대한 내용
		- Semantic: 오류제어 및 data를 형태에 따라 어덯게 해석할지에 대한 내용
		- Timing: data를 언제 어떤 속도로 보낼 것인지에 대한 내용
- **Protocol의 기능**
	- Addressing: 주소지정 방식
	- Sequencing: data 전송순서 명시. flow control, error control에 사용
	- Fragmentation&Reassembly: 블록을 분할 전송 -> 재조합하여 수신
	- Flow Control: "(보내는 양) <= (처리가능한 양)"이 되도록 control
	- Error Control: data 교환시 발생하는 오류 검출
	- Connection Control: 연결 설정에 있어 syntax, semantic, timing을 제어하는 것
	- Synchronization: data 교환시 타이머, 윈도우의 인자값을 일치시키는 것
	- Multiplexing: 하나의 통신 선로에서 다중 시스템이 동시에 통신
	- Transmission: 우선순위 결정, 서비스 등급, 보안 요구 등의 제어 서비스
	- Encapsulation: Data가 하위 layer로 갈때 현재 layer의 정보를 헤더에 덧붙임
	- Decapsulation: 하위 layer에서는 상위 layer에서 온 정보를 data 취급
- **Protocol 제정 기관**
	- ISO(International Organization for Standardization): 국제적인 표준화 기구
	- IEEE(Institute of Electrical and Electronics Engineers): 전기전자공학 주요 표준 및 연구정책 발전
	- ANSI(American National Standards Institute): 미국의 산업 표준을 제정
	- EIA:(Electronic Industries Alliance): 미 전자 산업 협회
	- ITU-T(International Telecommunication Union-T): ITU의 산하기구 중 전기통신 표준화 부문을 담당
- **TCP/IP의 개요**
	- 컴퓨터 기종에 관계없이 정보 교환이 가능하도록 해준다.
	- TCP는 packet의 흐름을 제어하고 IP는 packet을 목적지까지 전달한다.
- **TCP/IP의 구조**
	- L4(Application): 프로세스간의 통신. L3의 프로토콜을 사용하여 호스트간의 연결의 확립한다. 
	- L3(Transport): data를 segment 형태로 수신자에게 전달. 연결, 신뢰성, 흐름에저, 다중화 서비스 제공
	- L2(Internet): L3에서 받은 data를 packet 형태로 목적지까지 효율적으로 전달
	- L1(Network Interface): 물리적인 네트워크로 프레임을 전달
- **TCP & UDP**
	- TCP
		- src-dst를 1:1로 연결하여 직접 제어하기 때문에 속도가 느리다
		- 전송과 오류 수정에 있어 안전성과 신뢰성이 높다. 
	- UDP
		- 연결 과정, 전송 성공 여부 체크, 오류 수정 없음
		- 빠르지만 안정성은 떨어짐
- **Protocol number**
	- 서로 다른 프로토콜의 system에서 전송되는 데이터를 처리하기 위해 datagram header에 첨부
	- 리눅스 시스템에서의 protocol#들은 ``/etc/protocols``에서 확인 가능
- **Port number**
	-  
	- 
	- 

---


## 2. 네트워크 설정

#### **2.1. 환경 설정**
#### **2.2. 관련 명령어**


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgyMDQwMTQwMl19
-->

---

[Ref]: 정성재, 배유미. 리눅스 마스터 1급 정복하기 (1차,2차 시험대비). n.p.: 북스홀릭퍼블리싱, 2018.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5NDA5Nzg2MiwxNTU0NDg2MjUsNzQ2MD
cyOTM1LC0xNTc0MDcyOTYxXX0=
-->