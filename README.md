# docker-study

## 가상화 기술
서버는 웹서버(정적), WAS(동적 어플리케이션) 등으로 구분됨

### 왜 가상화?
한 OS에서 여러 소프트웨어를 실행하면 소프트웨어 간의 간섭이 발생한다.
1. 하나의 프로그램에 문제가 생기면 다른 프로그램에도 문제 가능성
2. 하나의 프로그램 트래픽이 급증해서 리소스를 모두 소모하면 나머지 프로그램 문제

- 사용자가 원하는 만큼 리소스 분배할 수 있다.
- 논리적으로 격리되어 있어 앞서 말한 문제들을 예방할 수 있다.

하드웨어 성능은 증가하고 소프트웨어 요구사항은 감소하는 추세.
높은 성능 서버 한 대를 운용하는 것이 효율적이다.

## 가상화 방식
### 1. Hypervisor(Virtual Machine)

<p align="center">
  <img src="./imgs/hypervisor.png" width="100%">
</p>
Host OS의 자원을 격리해서 새로운 OS(Guest)를 실행한다.
- 이미지를 스토리지에 저장하고 있다가, 실행하면 메모리와 CPU를 사용한다.

<p align="center">
  <img src="./imgs/process.png" width="100%">
</p>

프로세스는 시스템콜로 OS의 커널을 통해서만 리소스를 사용할 수 있다.

<p align="center">
  <img src="./imgs/hypervisor-vm.png" width="100%">
</p>

hypervisor 소프트웨어가 시스템 콜을 통역해주는 역할

### 2. Container

> 가볍고 빠르다.

<p align="center">
  <img src="./imgs/container.png" width="100%">
</p>

`LXC(Linux Containers)`를 사용하면 **Kernel 자체 기능**으로 격리 공간을 만들 수 있다.
- 커널의 `name space`와 `CGroups(control groups)`를 활용한다.
- name space : 프로세스, 하드드라이브, 네트워크, 사용자 등 리소스를 나누는 기준
- CGroups : 프로세스가 사용하는 메모리, CPU 등 리소스를 배분

> Host OS Kernel을 공유한다.

오버헤드가 작다 -> HW 리소스 사용 요청이 효율적이다.
