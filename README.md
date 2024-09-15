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

LXC는 사용자가 직접 컨트롤하기 어렵다.
`Docker`는 이러한 컨테이너 격리 기술을 활용할 수 있게 도와준다.

---

## Docker

`Container Platform` : Docker와 같은 컨테이너 가상화 도구 

<p align="center">
  <img src="./imgs/container-platform.png" width="100%">
</p>

- `컨테이너 엔진`과 `컨테이너 런타임`으로 구성됨
  - 컨테이너 엔진 : 사용자 요청 받아 컨테이너 관리
  - 컨테이너 런타임 : 직접 커널과 통신하며 **실제 격리 공간 생성**
    - Docker는 `RUNC`라는 컨테이너 런타임을 사용하며, OCI 표준에 맞는 다른 런타임도 가능

### 동작원리

<p align="center">
  <img src="./imgs/docker-architecture.png" width="100%">
</p>

도커는 client-server 모델로 실행된다.
Docker Daemon이 서버처럼 동작한다.
- Daemon : 서버에서 지속적으로 실행되는 소프트웨어
- API : 데이터를 주고 받는 규약

### Basic Commands

```bash
docker version
docker info

docker --help
docker ([management-commend]) [command]
```

```bash
# 컨테이너 실행
docker run {image-name}

# 컨테이너 삭제
docker rm {container-name / id} 
```

```bash
docker run -p 80:80 --name nginx-test nginx
```

---

## Image

Image : 파일 시스템의 특정 시점을 저장해 놓은 압축 파일

- 프로그램 : Disk에 저장된 실행 가능한 SW
- 프로세스 : 프로그램이 매모라에 올라가 실행 상태임

- 이미지 : Disk에 저장된 실행 가능한 SW
- 컨테이너 : 이미지가 메모리에 올라가 실행 상태임
  - 이미지 1개로 여러 컨테이너 실행 가능
  - 컨테이너 실행 시 프로세스도 함께 실행

### Image Command

```bash
# 백그라운드(-d)에서 컨테이너 실행
docker run -d --name {container-name} {image-name}

# 실행 중인 컨테이너 리스트 조회
docker ps 

# 실행 중인 컨테이너 삭제
docker rm -f {container-name}
```

### Image-metadata