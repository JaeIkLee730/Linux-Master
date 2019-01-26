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
- 통신망의 종류
	- 규모에 따라 나뉜다
	- LAN (Local Area Network): 근거리. 집, 사무실, PC방 정도
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
				- 
	- MAN (Metropolitan Area Network): 도시 하나 정도
	- WAN (Wide Area Network): 원거리

### **1.4. TCP/IP 및 네트워크 프로토콜의 이해**

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
eyJoaXN0b3J5IjpbMjAyNzE5NTk0OSw3NDYwNzI5MzUsLTE1Nz
QwNzI5NjFdfQ==
-->