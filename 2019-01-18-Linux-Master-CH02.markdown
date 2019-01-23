# CH2. 리눅스 시스템의 이해


## 1. 리눅스와 하드웨어

**1.1. 하드웨어의 이해**
* 리눅스 탄생: 유닉스와 호환되는 PC용 OS를 만들자
* 리눅스 설치: CPU,  메모리, 하드 정보 숙지하고 알맞은 리눅스를 설치할 것
    * CPU: 모델, bit수, core수, 가상화지원여부
    * memory: 대부분의 종류 OK, 대용량 메모리는 64bit 필요
    * 하드 디스크: cPU, memory에 비해 변경이 많은편 -> 종류 숙지
    * 모니터와 비디오 어댑터: 정확한 드라이버 -> 최적의 성능
    * 네트워크 인터페이스: 제조사홈페이지에 모듈 제공, IP, 넷마스크, 게이트웨이 주소, DNS IP 알아여 네트워크 설정 가능
    * CD-ROM: 하드디스크와 마찬가지로 파일로 관리된다.
**1.2. 하드웨어의 선택**

* RAID
    * Redundant Array of Independent Disks
    * 여러개의 하드디스크가 있을 때, 동일한 때 동일한 데이터를 다른위치에 중복해서 저장하거나 하나의 데이터를 다른위치에 나누어 저장
    * RAID-0 (striping): data를 여러 드라이브에 나누어 넣고 각각의 드라이브에 동시 접근 -> 빨라
    * RAID-1 (mirroring): 디스크에 에러가 발생시 데이터의 손실을 막기 위해 추가적으로 하나 이상의 장치에 중복 저장
    * RAID 2~: striping, mirroring, parity, Error check 등을 결합한 방식
* LVM
    * 파티션 변경과 디스크 설정이 자유롭고, 유동적
    * PV: LVM에서 블록장치를 사용하려면 PV로 초기화 필요. 물리적으로 분할한 파티션
    * VG:PV가 모인 것
    * LV: 논리적 분할 파티션
    * PE: PV에서 사용하는 블록
    * LE: LV에서 사용하는 블록. PE에 1:1 mapping


## 2. 리눅스의 구조

**2.1. 부트 매니저**
* == BootLoader
* 부팅을 도와주는 프로그램
* 하드디스크의 맨 앞쪽 영역인 MBR에 설치
* LILO, GRUB
* GRUB
    * /boot/grub/grub.conf
    * grub booting mode
        * [a]: 매개변수 추가 가능
        * [e]: grub.conf의 모든 항목 직접 편집. 실제 파일 변경 X
        * [c]: 명령어를 통한 대화식

**2.2. 디렉터리 구조 및 역할**
* /:
    * 
    * /boot: 부팅시 필요한 파일이 들어있는 디렉토리
    * /dev: 하드디스크, CD-ROM 등 실제로 존재하는 물리적인 장치 등을 파일화하여 관리하는 디렉터리
    * /etc: 시스템 환경 설정 파일, 부팅 관련 스크립트 파일
    * /home: 사용자들의 홈 디렉토리가 위치하게 되는 디렉토리
    * /lib: 각종 라이브러리 저장
    * /lost+found: fsck 명령어를 이용하여 파일 시스템을 복구할때 작업하는 디렉토리
    * /mnt: 장치들을 마운트할때 포인터가 되는 디렉토리
    * /misc: 자동 마운트 프로그램 autofs에 의해 사용되는 디렉토리
    * /opt: 응용 프로그램들의 설치를 위해 사용되는 디렉토리
    * /proc: 가상파일 시스템. 시스템에서 운영되고 있는 다양한 프로세스의 상태 정봏, 하드웨어 정보, 기타 시스템정보 등을 담고 있다.
    * /root: root 유저의 홈 디렉토리
    * /bin: binary의 약자. 실행파일, 명령어. /usr같은 큰 파티션이 마운트 되기 전 '/'에 위치해야할 작은 program들이 들어있다.
    * /sbin: /usr같은 큰 파티션이 마운트 되기 전 '/'에 위치해야할 작은 program들이 들어있다. 단 시스템 관리에 대한 명령어들
    * /usr/bin: 배포판에서 관리하는 일반적인 유저 프로그램들
    * /usr/sbin: 배포판에서 관리하는 시스템 관리 프로그램들
    * /usr/local/bin(sbin): 배포판 패키지 관리자가 관리하지 않는 프로그램
    * /tmp: 각종 작업에서 임시로 생성되는 파일을 저장
    * /usr: 시스템 운영에 필요한 명령, 응용 프로그램들이 위치
    * /var: 시스템 운영 로그 파일과 스풀링과 같은 가변적인 데이터를 보관 

**2.3. 부팅과 셧다운**
* 부팅
    * Power ON -> BIOS가 하드웨어 점검 -> CMOS에 설정된 첫번째 부팅 하드디스크 확인 -> 첫번째 하드의 MBR 영역에 있는 부트매니저 실행 -> 환경설정파일을 열람 + 유저가 매개변수, 설정 변경 가능 -> OS 부팅
    * 하드웨어 인식
        * 하드웨어 사용하려면 kernel에 포함되어 있어야 함 -> 포함되어있지 않을경우 -> 환경설정파일에 모듈을 등록시켜주면 -> 부팅시 설정파일을 읽고 -> 모듈을 kernel에 적재 -> 하드웨어 인식 완료
    * SW 인식
        * kernel load -> mount root file system by Read-only -> 이상무 -> RW로 다시 mount -> kernel이 init process 발생 -> /etc/init 안에 있는 설정파일 읽어서 동작 -> Run Level에 맞는 파일을 찾아 실행
    * RunLevel
        * 0: halt (종료)
        * 1: single mode (윈도우의 안전모드 같은거)
        * 3: 일반적인 TUI
        * 5: 일반적인 GUI (X윈도)
        * 8(또는 s 또는 S): 긴급복구 mode
* 로그인
    * 리눅스는 다수가 사용하는 OS기 때문에 로그인 절차 필수
    * /etc/issue, /etc/issue.net, /etc/motd
* 로그아웃
    * 로그인만 해두고 작업을 하지 않으면 자원이 낭비, 보안 위험
    * 작업끝나고 로그아웃 필수
    * 일정시간 지나면 자동 로그아웃 -> /etc/profile TMOUT=초 로 설정
* 종료
    * shutdown [option] 시간 [경고 메세지]
        * root 사용자만 가능
        * 안전한 종료 방법
    * reboot [option]
        * 모든 사용자
        * 재부팅
    * halt
        * 모든 사용자
        * 종료
    * poweroff
        * 모든 사용자
        * 종료 
    * init, telinit
        * 모든 프로세스의 조상인 init 프로세스에게 직접 ㅇ쇼청하여 실행 레벨 변경
        * 실행중인 프로세스를 무조건적으로 종료하므로 권장하지는 않는다

**2.4. 파일 시스템의 이해**
* OS가 파티션이나 디스크에 데이터를 RW 하기 위해 구성하는 일련의 체계
* OS 설치 -> 파티션 분할 -> 포맷 -> file system이 구성됨 -> OS가 data RW 가능
* file system에 따른 규칙:
    * 파일명 길이, 파티션 크기, 파일 크기, 디렉토리 수,...
* 리눅스 파일 시스템
    * Minix File System -> ext -> ext2 -> ext3 -> ext4 ->xfs
    * proc: 리눅스에서 사용하는 가상 파일 시스템
    * ext3 부터 저널링 도입
    * 저널링: 파일 시스템에 대한 변경사항을 반영하기 전에 로그에 변경사항을 저장하여 추적이 가능하게 만든 것. 시스템 오류 발생시 복구 확률을 높여준다. 
* 리눅스 파일 시스템 구조
    * 디스크 드라이브 파티션 분할 -> 파티션 포맷 -> 파일시스템 생성 완료 -> 파일/디렉토리 생성 -> i-node 생성 -> i-list에 i-node 등록
    * i-node
        * 파일이나 디렉토리의 모든 정보를 갖고 있는 자료구조
        * i-list는 i-node의 list
        * i-number는 i-node가 i-list에 등록되는 entry number
    * block: 시스템에서 기본적으로 데이터를 저장하는 단위
    * data block: 파일이 보관해야 할 정보가 들어있다
    * directory block: i-number와 그에 해당하는 filename의 쌍들이 들어있다. 
    * ext2 Disk
        * MBR
        * Reserved
        * Partition 1
        * ...
        * Partition N -> format -> file system
            * Boot block: 
            * Block Group 0
            * ...
            * Block Group N
                * Supre block: 파일 시스템에 대한 전체적인 정보
                * Group Description: 각각의 블록 그룹을 기술하는 자료구조
                * Blcok bitmap: 블록의 사용 현황을 비트로 표현
                * i-node bitmap: i-node 할당 상태를 비트로 표현
                * inode table: i-node에 대한 정보
                * data blocks: 파일이 보관해야 하는 정보를 저장
                * 간접 블록: 아이노드 수가 한정되어 있기 때문에 추가적인 데이터 블록을 위해 동적으로 할당될 수 있는 공간 필요


## 3. X-윈도

**3.1. X-윈도의 개념 및 특징**
* X-윈도: platform(디스플레이 장치)-independent 네트워크 프로토콜 기반 GUI 시스템
* 오픈소스이다. XFree86은 중단되었고, 현재는 X.org에서 관리된다.
* X-client ---- X-protocol ---- X-server
* Xlib: X 서버와 대화하는 역할을 하는 작고 단순한 라이브러리

**3.2. X-윈도 설정과 실행**
* 설정: 그래픽카드 설정이 제일 중요. 정확한 드라이버로 설치할 것
* 실행: RunLevel 5로 실행 / startx 명령어
* 디스플레이 매니저
    * X 윈도 시스템에서 작동하는 프로그램 중의 하나로  X-server의 접속과 세션 시작을 담당
    * XDM, GDM, KDM

**3.3.데스크톱 환경**
* GUI를 이용하기 위해 사용자에게 제공되는 인터페이스 스타일
* KDE, GNOME, 

**3.4. 윈도 매니저**
* X 윈도 환경에서 윈도의 배치와 표현을 담당하는 시스템 소프트웨어
* Mutter(GNOME3), Metacity(GNOME2), KWin(KDE)

**3.5. X-윈도 활용**
* X-server와 X-client가 독립적으로 동작하는 네트워크 지향 시스템이기 때문에 원격지의 X-client를 다른 시스템의 X-server에서 실행시킬 수 있다.
* xhost 명령: X-server에 접근할 수 있는 클라이언트를 지정하거나 해제하는 명령
* 환경변수 DISPLAY 값이 0.0인 경우 : 첫번째 X-윈도의 첫번째 모니터로 X-client 프로그램을 전송한다는 의미
* xauth 명령: IP, 사용자의 key를 통해 인증. host원치않는 불필요한 클라이언트의 접속을 막을 수 있다.
* X-윈도 응용 프로그램
* GIMP, Totem, KMid, ImageMagick, eog, kdergraphics, Rhythmbox, evince, LibreOffice 


## 4. Shell

**4.1. Shell의 이해**
* 개념
    * Kernel과는 분리되어 kernel과 사용자간의 다리 역할
    * User가 로그인 -> shell 부여 -> 비로소 명령 수행 가능
    * Multics -> Bourne Shell -> C Shell, csh -> sh, bash(Bourne Again Shell - 표준), ksh, tcsh, zsh, 
* Shell 종류 확인, 변경
    * $ echo $SHELL  // 확인
    * $ chsh -l  또는   $ cat /etc/shells     // 사용가능한 shell 확인
    * $ chsh -s [shell_절대경로]      // shell 변경
* SHELL 환경 설정
    * shell 변수: 특정한 SHELL에만 적용, 지역변수같은 느낌, prompt 상에서 지정하셔 사용가능, 일반적으로 소문자
    * 환경변수: 모든 SHELL에 적용,  전역변수같은 느낌, 미리예약됨, 일반적으로 대문자, $ env 명령으로 현재 지정된 환경변수들을 확인할 수 있음
* Bash shell의 주요 기능
    * 자동완성: tab키로 자동완성
    * History: ~/.bash_history에 실행했던 명령들 저장, history명령으로 이전 명령들 열람
    * alias: 어떤 명령을 지정해 놓으면 사용자가 그 명령을 실행시 alias로 지정된 명령이 대신 실행
    * 명령행 편집기능: 명령행에서 커서 이동, 삭제를 빠르게 할 수 있는 단축키들: ESC/ctrl + b/f/a/e/d/k/u/y/....
    * 명령 대체 (치환 기능) : `backquote`또는 $()를 사용하여 특정 명령의 결과를 다른 명령어의 인자값으로 사용할 수 있도록 해준다
    * 그룹 명령: ";", "&&", "||"를 사용하여 하나의 명령행에 여거래그이 명령어를 동시에 넣을 수 있다. 
    * Standard I/O 제어 기능: 
        * stdin: 0 / stdout: 1 / stderr:2
    * Redirection
        * >, >> : 출력 변경
        * <, << : 입력 변경
    * Pipe:
        * 어떤 process의 표준 출력이 또다른 어떤 프로세스의 표준 입력으로 쓰이게 하는것
        * tee : pipe 연결 출력을 두 갈래로 나눌 때 사용하는 명령
    * Process control: bg process, fg process
    * expr을 사용한 산술 연산 기능
    * Prompt를 원하는 대로 변경 지정가능. 환경변수 PS1, PS2(2차 프롬프트) 이ㅛㅇ행서 변경가능
    * 확장된 bash 내부 명령어: bash 자체적으로 해석하는 set, export 등의 내부 명령어들이 점점 많아지는중
* bash shell 관련 파일 및 디렉터리
    * /etc/profile: 시스템 전체에 적용되는 환경변수와 시작 관련 프로그램 설정
    * /etc/bashrc: 시스템 전체에 적용되는 alias와 함수를 설정
    * ~/에 있는 .* 파일들 : 개인 사용자에게 적용되는 내용
    * process 1
        * linux  ---call--->  /etc/profile  ---call--->  /etc/profile.d/test.sh (test.sh는 임의의 이름임)
    * process 2
        * linux  ---call--->  ~/.bash_profile  ---call--->  ~/.bashrc ---call---> /etc/bashrc
* shell에서 사용되는 특수문자
    * 디렉터리 계층구조: ~(home), .(현재 디렉토리), ..(부모 디렉토리)
    * 'single quote' : 모두 문자 취급
    * "double quote": $,`,\,! 를 제외하고 모두 문자 취급
    * `backquote` : 명령 치환
    * #: comment. 환경설정 파일에서 사용
    * $: 뒤에 오는 문자열을 변수로 취급
    * &: 프로세스를 bg로 실행
    * *: 아무것도 없는 경우를 포함한 모든 문자
    * ?: 한문자를 대체 / return 변수로 사용
    * (): subshell. 하나의 shell 단위로 묶어준다 
    * \: escape. 특수문자나 alias의 기능을 없앤다. 명령행 입력이 길어질때 행 연장에도 사용
    * [ 문자1, 문자2, ...]: 안에 오는 문자들 중 하나  라는 의미
    * {문자열1, 문자열2, 문자열3}: 안에 오는 문자열 중 하나   라는 의미
    * >,<: 입출력 컨트롤
    * /: 경로명
    * !: 명령문 history

**4.2. Shell Programming**
* 개념
    * shell에서 사용되는 여러 명령어들을 모아 하나의 파일(shell script)로 만드는 과정
* 작성
    * 파일 생성
    * #!/bin/bash : 첫줄에 사용할 셸 명시
    * body 작성 후 저장
    * chmod 755 [파일명] : 실행할 수 있는 권한을 부여한다.
* 실행
    * 실행권한 부여 후 ./[파일명]
    * source [파일명]
    * sh [파일명]
    * . [파일명]
* 문법
    * #을 사용하여 comment
    * 변수
        * 변수형은 문자열 밖에 없으므로 따로 변수형을 지정하지 않음
        * 변수명: -를 제외한 특수문자와 숫자로 시작하면 안됨
        * 대입연산 : 변수명=값    // 공백이 없도록
        * 변수의 값 사용 : $변수명
        * :=, :+, :-, ?, …. 등을 사용한 여러가지 변수 대응법이 있다.
        * $0: 실행된 shell script 명
        * $1, $2, $3, …. 명령행 인자
        * $$, $*, $@, $?, $-
        * $ set : 현재 정의되어 있는 모든 변수와 그 값을 출력
        * $ env : export된 변수의 값만 출력
        * $ export : 특정 변수(local)의 범위를 환경 데이터 (global) 공간으로 전송하여 자식 프로세스도 사용가능하도록
        * $ unset: 선언된 변수를 제거
    * echo, escape
        * 특수 문자도 모두 문자 그래도 출력한다.  
        * -e option이 있으면 escape문자가 있는 부분은 특수문자로 번역한다.
    * 간단한 조건문
        * test expression 또는
        * [ expression ]     // 반드시 공백을 포함해야 한다. 
        * 1 또는 0 반환 : True/False
    * 조건문
        * if[ condition ] - then - elif[ condition ] - then - else - fi
        * case - 문자열 - in - 정규식1) 명령1 - 정규식2) - 명령2 - …… - *) default 명령 - esac
        * select - 변수 - in 값1, 값2, …. - do - body - done      // 자동으로 메뉴를 생성해주고 입력도 받는다
    * 반복
        * for - 변수 - in 값1, 값2,….. - do - body - done
        * while - [ condition ] - do - body - done    // condition이 참인동안 수행
        * until - [ condition ] - do - body - done     // condition이 거짓인동안 수행   
    * 함수 
        * def
            * function_name( ) { body }
            * function function_name { body }
        * call
            * function_name
    * 패턴을
        * 처음부터(#, ##) / 끝부터(%, %%) 
        * 비교하여 match하는 부분 중 
        * 작은(최소) 부분(#, %) / 큰(최대) 부분(##, %%)  
        * 제거 후 나머지 return


## 5. Process

**5.1. Process의 개념 및 종류**
* 하드디스크에 저장되어 있는 program이 실행되어 주기억장치(RAM)에 올라가게 되면 process라고 한다.
* PID / bg or fg / Process Control Block / Program Counter
* Process의 생성
    * 리눅스를 부팅하면 kernel이 init 프로세스라는 최초의 프로세스를 발생시킴 (PID==1) 데몬을 비롯한 시스템 운영에 필요한 여러 프로세스들은 최초의 프로세스인 init으로부터 복사되어 실행된다.
    * fork: 새로운 프로세스를 새로운 메모리를 할당받아 실행한다. 일반적으로 fork방식이며 사용자가 로그인하면 bash라는 프로세스를 할당받고 명령을 내리면 bash의 child 프로세스로 fork된다. 
    * exec: 원래의 프로세스를 새로 호출된 프로세스로 대체한다. (메모리에 덮어씀)
* Process의 종류
    * Foreground process
        * 내가 실행시키고 종료되기를 기다리는 프로세스
        * 이 프로세스가 종료되기 전에는 그 터미널로는 다른 작업을 할수가 없음
        * 현재의 진행상태를 알려주는 프로세스도 있고 
        * Task를 마치면 그냥 멈춰버리는 놈도 있고
        * 기본적으로 모드 process는 foreground로 실행된다
        * Shell에서는 prompt 에 명령을 입력함으로 실행한다
            * Ex) ls, pwd, .....
    * Background Process
        * 메모리 사용이 여유가 있는 상황이라면 process를 실행후 process가 끝나기 전에 다른 process를 실행시킬 수 있다.
        * $ command_a &.    // “&”를 붙임으로 process를 background로 실행시킬 수 있다.
        * 키보드와(입력과?) 연결되지 않은 상태로 실행된다
    * BG <—> FG 전환
        * suspend
        * bg / fg 명시하여 다시 가동
    * Suspend: 메모리에 올라와서 작업중인 프로세스를 일시적으로 중지 -  ctrl + Z 사용
    * $ bg %[PID]: 중지된 process를 background mode로 다시 가동
    * $ fg %[PID]: 중지된 process를 foreground mode로 다시 가동 
    * $ jobs: 작업상태 확인하기
    * + : 현재 최우선순위인 프로세스
    * - : 현재 차우선순위인 프로세스
* signal
    * process간 메세지를 보내는 것
    * 시그널이 발생하는 경우 다양함 - 인터럽트 키가 발생, 프로세스가 발생, 하드웨어가 발생
    * 시그널마다 이름, 번호

**5.2. Process 관리의 이해**
* 데몬(daemon)
    * 개념
        * 주기적, 지속적인 서비스 요청을 처리하기 위해 계속 실행되는 프로세스를 칭함. 서버 프로그램 등....
        * 백그라운드로 실행된다. 이름뒤에 ~d가 붙음
        * inetd: superdaemon의 하나. 데몬의 종류가 늘어남에 따라 이들을 관리하기 위해 등장 
        * standalone 방식: 부팅 —> 실행 —> 메모리에 상주 —> 계속 실행
        * inet 방식: client 요청 —> inetd가 요청된 process 실행 —> 작업완료 —> inetd가 process 종료
    * 실행
        * 부팅시 런레벨에 따라 실행
        * 부팅후에는 서비스에 따라 시작, 중지할 수 잇는 스크립트를 사용
            * /etc/rc.d/init.d  또는 
            * /etc/rc.d/rc0.d ~ /etc/rc.d/rc6.d (symbolic-link 들만 들어있음 - init.d 내의 파일에 대한 link들)
            * service [SCRIPT_FILE_NAME] start/stop/restart/reload   또는
            * [SCRIPT_FILE_ABSOLUTE_PATH] start/stop/restart/reload
    * 관련 유틸리티
        * 부팅 중 런레벨에 따라 자동으로 실행되는 서비스를 설정할 수 있는 유틸
        * ntsysv, chkconfig, system-config-services
