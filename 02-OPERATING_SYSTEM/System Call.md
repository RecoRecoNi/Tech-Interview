## 시스템 콜이 무엇인지 설명해주세요.

- 사용자 모드로 실행되는 응용 프로그램이 하드웨어 자원에 접근하거나 운영체제가 제공하는 서비스를 이용하기 위한 요청을 `시스템 콜`이라고 합니다. 운영체제는 **커널 모드**와 **사용자 모드**로 나뉘어 구동되며, 커널 모드에서만 메모리 등의 자원을 직접 조작하고, 하드웨어를 제어할 수 있기 때문에 커널모드로의 전환이 필요합니다. 즉, 시스템 콜은 커널이 제공하는 서비스를 이용하기 위한 **인터페이스**이며, 사용자가 자발적으로 커널 영역에 진입할 수 있는 유일한 수단이라고 볼 수 있습니다.
<br>

## 꼬리질문

### 1. 우리가 사용하는 시스템 콜의 예시를 들어주세요.
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/5998498d-53b3-4dc4-84fb-1e5a627eb35a" height="400" width="500px"></p>

- file I/O의 open, read, write, close 등이 있습니다. 예를 들어, read 같은 경우 하드웨어 메모리에 존재하는 특정 파일에 접근해서 파일을 읽을 준비를 하는 일련의 과정이 커널 모드에서 수행되게 됩니다. 이외에도 Python의 import sys, time 등의 함수 역시 시스템 콜의 예시로 들 수 있습니다.
<br>

### 2. 시스템 콜의 유형에 대해 설명해주세요.

> 💡 **프로세스 제어**, **파일 조작**, **장치 조작**, **정보 유지보수**, **통신과 보호**
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/de4a2cdd-4aad-42c4-9840-5ce15362009a" height="150" width="400"></p>

- **프로세스 제어(Process Control)**: 프로세스 실행, 생성, 대기 등
  - `fork()`: 새로운 프로세스 생성
  - `exec()`: 새로운 프로그램 실행
  - `exit()`: 현재 프로세스 종료
  - `wait()`: 자식 프로세스가 종료될 때까지 대기

- **파일 조작(File Manipulation)**: 파일 열기, 읽기, 쓰기 등
  - `open()`: 파일 열기
  - `read()`: 파일 읽기
  - `write()`: 파일 쓰기
  - `close()`: 파일 닫기

- **장치 관리(Devide Management)**: 디바이스 부착, 분리, 읽기, 쓰기 등
  - `read()`: 장치 읽기
  - `write()`: 장치 쓰기
  - `ioctl()`: 장치 제어

- **정보 유지(Information Maintenance)**: 시간, 날짜 설정 등
  - `getpid()`: 현재 프로세스의 ID 가져오기
  - `alarm()`: 지정한 시간 후에 알람 시그널을 보내는 타이머를 설정
  - `sleep()`: 현재 프로세스를 지정한 시간 동안 멈춤

- **통신(Communication)**: 통신 연결 생성, 제거, 상태 정보 전달 등
  - `pie()` : 두 프로세스 간에 단방향 통신 파이프를 생성
  - `shm_open()` : 공유 메모리 객체를 생성하거나 열기
  - `mmap()` : 파일의 내용을 메모리에 매핑하여 읽거나 쓸 수 있도록 변경

- **보호 (Protection)**: 파일 및 디렉터리의 접근 권한과 소유자 정보를 관리 등
  - `chmod()`: 파일 또는 디렉터리의 권한 변경
  - `umask()`: 새로운 파일 생성 시의 기본 권한 제한 설정
  - `chown()`: 파일 또는 디렉터리의 소유자와 그룹 소유자 변경
<br>

### 3. 시스템 콜이 운영체제에서 어떤 과정으로 실행되는지 설명해주세요.
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/9f651309-bf21-4bf2-b947-55c101830a40" height="350" width="450"></p>

1. **사용자 프로그램 실행**: 먼저, 사용자가 작성한 프로그램이 사용자 모드에서 실행됩니다. 
2. **시스템 콜 호출**: 프로그램이 운영체제의 특정 기능을 사용해야 할 때, 그 기능을 호출하는데 사용되는 함수 또는 명령을 호출합니다. 이것이 바로 시스템 콜입니다. (예를 들어, 파일을 열고 읽는 작업을 해야 할 때, 사용자 프로그램은 파일 관련 시스템 콜을 호출합니다.)
3. **사용자 모드에서 커널 모드로 전환**: 시스템 콜 호출이 발생하면, 프로그램은 현재 실행 중인 사용자 모드에서 커널 모드로 전환됩니다. 커널 모드에서만 운영체제의 핵심 기능과 하드웨어 자원에 접근할 수 있기 때문입니다.
4. **요청 분석 및 처리**: 커널은 내부적으로 시스템 콜 각각의 서비스 루틴에 대응되는 인덱스 테이블을 지니고 있고, 시스템 콜이 호출되면 이에 대응되는 인덱스를 참조하여 서비스 루틴을 수행합니다.(예를 들어, 파일 열기 요청의 경우 해당 파일을 찾아서 열기 작업을 수행합니다.)
5. **작업 수행 및 결과 반환**: 운영체제는 요청된 작업을 수행하고, 결과를 메모리에 저장하거나 레지스터에 반환합니다. (예를 들어, 파일을 읽는 작업의 경우 파일 내용을 메모리에 읽어옵니다.)
6. **커널 모드에서 사용자 모드로 전환**: 요청된 작업이 완료되면, 운영체제는 다시 사용자 모드로 전환합니다. (이제 프로그램은 작업 결과를 받아 사용자 모드에서 필요한 작업을 계속할 수 있습니다.)
7. **프로그램 실행 재개**: 프로그램은 이제 시스템 콜 이후부터 다음 작업을 계속 수행합니다. 필요한 결과를 받아 사용자 모드에서 다양한 작업을 진행할 수 있습니다.
<br>

### 4. 운영체제의 Dual Mode에 대해 설명해주세요.
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/b632580d-1d31-408b-a320-2cacbb2aec45" height="350" width="450"></p>

**`Dual Mode(이중 모드)`**

프로세스는 크게 커널 프로세스와 사용자 프로세스로 나뉩니다. 사용자 프로세스가 커널의 기능을 사용하기 위해서는 시스템 콜을 요청해야 합니다. 사용자 프로세스는 시스템 콜을 요청한 후 대기 상태로 전환되고, 하드웨어 리소스에 접근할 수 있는 커널 프로세스는 요청받은 작업을 처리합니다. 이 때 **사용자 모드에서 커널모드로 전환**되는데, 이와 같이 **운영체제가 두 모드를 전환하며 일 처리를 하는 것**을 **이중 모드**라고 합니다.

- **커널 모드(Kernel Mode)**
    
    → 커널 모드는 운영체제의 핵심인 커널 코드가 실행되는 상태입니다. 커널은 운영체제의 핵심 기능을 수행하며 하드웨어 리소스에 접근하고 관리할 수 있는 권한을 가집니다. 커널 모드에서 실행되는 코드는 시스템의 모든 자원에 접근하여 제어할 수 있으므로, 시스템 콜 및 운영체제의 서비스를 제공한다.커널 모드에서는 특권 명령(Privileged Instructions)을 실행할 수 있습니다. `mode bit = 0`으로 관리됩니다.
    
- **사용자 모드(User Mode)**
    
    → 사용자 모드는 일반 응용 프로그램이 실행되는 상태입니다. 이 모드에서 실행되는 프로그램은 자신의 주소 공간 내에서만 작동하며, 하드웨어 리소스에 직접적인 접근이 허용되지 않습니다. 즉, 사용자 모드에서는 응용 프로그램이 제한된 권한을 가지며, 시스템 리소스에 대한 직접적인 제어는 불가능합니다. 유저모드에서 강제로 Privileged Instructions 을 실행하면 Exception이 발생합니다. `mode bit = 1`로 관리됩니다.
<br> 

### 5. 왜 유저모드와 커널 모드를 구분해야 하나요?

- 사용자 프로세스가 항상 (시스템 자원에 직접 접근 가능한)특권을 가진 커널 모드에 있다는 것은 전체 시스템에 장애를 일으킬 수 있는 위험을 수반합니다. 그러므로 상대적으로 안전한(Safety) 모드인 사용자 모드에서 프로그램을 실행하고, 하드웨어 자원에 접근이 필요한 경우 시스템 콜(System Call)을 요청하여 커널 모드로 운영체제에게 모드의 전환을 요청하는 것입니다.
<br>

### 6. 서로 다른 시스템 콜을 어떻게 구분할 수 있을까요?
- 통상적으로 시스템 콜은 여러 종류의 기능으로 나뉘어져 있으며, 각 시스템 콜에는 번호가 할당됩니다. System Call Interface는 이러한 번호에 따라 인덱스 테이블을 관리하고 서로 다른 시스템 콜을 구분할 수 있습니다. 일부 운영체제에서는 지정된 함수의 이름이 운영체제 내부에서 시스템 콜 번호로 변환되어 호출하는 과정에서 시스템 콜을 구분하여 실행합니다. 위 두 방법 중 하나 또는 둘을 조합하여 시스템 콜을 구분하고 처리합니다.
<br>

## 꼬꼬무

### 1. 시스템 콜이 필요한 이유는 무엇인가요?
1. **유효성 검증 & 하드웨어 관리**: 커널 모드와 사용자 모드로 나뉘는 것은 운영체제와 사용자 프로세스 간의 권한을 분리하여 유효성을 검증하는 역할을 합니다. 운영체제는 컴퓨터 시스템의 중요한 자원과 기능을 제어하는데, 사용자 프로세스가 이 자원에 직접 접근하거나 제어할 경우, 시스템의 안정성이 약화될 수 있습니다. 이를 방지하기 위해 시스템 콜을 통해 커널 모드에서만 특권 명령을 실행하고 하드웨어 접근 등을 제한함으로써 시스템을 안전하게 유지합니다.
2. **효율성**: 운영체제는 여러 작업을 동시에 관리하며, 이를 위해 작업을 스케줄링하고 자원을 할당해야 합니다. 시스템 콜을 사용하여 작업을 요청하면, 운영체제는 이를 효율적으로 관리하고 조정할 수 있습니다.
<br>

### 2. 유저모드에서 커널모드로, 커널모드에서 유저모드로 전환될 때 Context Switching이 발생하나요?
- **Context Switching**은 현재 실행 중인 프로세스의 상태를 저장하고, 다음 실행할 프로세스의 상태를 복원하는 작업을 의미합니다. 따라서, 커널모드와 유저모드 간 전환이나 다른 프로세스로의 전환 시에도 Context Switching이 발생합니다.

  - **유저모드 → 커널모드**: 사용자 프로세스가 시스템 콜을 호출하거나 중요한 작업을 수행해야 할 때, 커널 모드로 전환됩니다. 이때 현재 사용자 프로세스의 상태는 저장되고, 시스템 콜이나 작업 처리가 완료되면 다시 해당 프로세스의 상태가 복원됩니다.
  - **커널모드 → 유저모드**: 커널 모드에서 사용자 프로세스로 다시 전환될 때에도 현재 커널 모드에서 실행 중인 프로세스의 상태가 저장되며, 이전에 실행되던 사용자 프로세스의 상태가 복원됩니다.
 
## Reference
- [유튜브 - System Calls](https://www.youtube.com/watch?v=lhToWeuWWfw)
- [나무위키 - 시스템 호출](https://ko.wikipedia.org/wiki/%EC%8B%9C%EC%8A%A4%ED%85%9C_%ED%98%B8%EC%B6%9C)
- [유튜브 - 인터럽트와 시스템 콜을 설명합니다! 당연히 유저 모드, 커널 모드도 설명해야겠죠? 그런데 이 모든게 프로그래밍 언어와 무슨 상관이냐구요?? 상관있죠! 왜 상관있냐면요..!](https://www.youtube.com/watch?v=v30ilCpITnY)
- [티스토리 - [Operating System] (iOS) System Call (시스템콜, 시스템 호출이란?)](https://didu-story.tistory.com/311)
- [벨로그 - [OS] 시스템 콜, System Call](https://velog.io/@nnnyeong/OS-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%BD%9C-System-Call)
- [티스토리 - [운영체제] 시스템 콜(System Call)](https://c4u-rdav.tistory.com/85)
- [깃허브 - 리눅스 시스템 호출 정리](https://kangtegong.github.io/self-learning-cs/system_calls/syscalls.html)
- [벨로그 - [운영체제] 인터럽트/시스템 콜](https://velog.io/@klm03025/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%BD%9C#%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%EC%9D%98-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95---dual-mode)