# X.25 / Frame Relay / Cell Realay

### 컴퓨터 통신망 발전
> 전화망 -> Telnet, Tymnet: 고품질의 데이터 서비스 제공 하기 위해 설치된 패킷 교환망 -> 장비들간 호환성 문제 -> 전화회사들을 중심으로 표준 프로토콜 제정 -> X.25

<br>

### X.25
- 데이터 -> 패킷단위로 분할 -> 전송 -> 패킷 composition -> 수신자
- 에러에 취약해서 에러 검출, 복구 기능을 탑재해야 했고 빠른 전송속도를 기대할 수는 없었다.
- Packet Switching 기반
- 1, 2, 3 계층에서의 처리
- 고정된 대역폭을 가짐

<br>

### Frame Relay
- X.25을 계승하고 단순화하여 만들어짐
- 이제 망의 신뢰도가 높아졌으니 에러제어와 흐름제어 기능은 단말에서 담당하는 것으로 하고 우리는 전송(relay)기능만 하자 하여 나온 것
- Virtual Circuit 기반
- Frame 단위로 전송하며 Frame 은 길이가 변할 수 있는 단위이다.
- Statistical Multiplexing 사용하여 대역폭을 동적으로 공유
- 대역폭 절약, X.25 대비 낮아진 전송지연

<br>

### Cell Relay
- ATM(Asynchronous Transfer Mode)라고도 한다.
- Frame Realy에서 발전된 형태이다.
- Asynchronous Time Multiplexing, 고속 패킷 교환 지원
- 전송단위는 고정 길이를 갖는 패킷인 Cell이다
- 고정 길이의 패킷을 사용함으로 가변적인 Frame을 사용할 때보다 오버헤드를 줄일 수 있있어 전송률이 빨라진다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIyMzE2ODE3MV19
-->
