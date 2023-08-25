# Hello xv6!

xv6는 Unix의 운영체제에서의 기본 인터페이스를 제공하고 내부 디자인 또한 모방된 교육용 운영체제입니다.

## 설치 과정

### 1. 환경

VMware ubuntu 22.04 LTS 환경에서 진행하였습니다. 

qemu는 애뮬레이터로 하드웨어를 소프트웨어적으로 구현해 특정 실행 환경을 제공하는 역할을 합니다.
따라서 xv6가 동작되기 위해서는 가상의 하드웨어 환경인 qemu의 설치가 필요합니다. 

먼저 프로세서가 하드웨어 가상화를 지원하는지 아래와 같이 확인하시면 됩니다. 
지원하는 경우에 0보다 큰 숫자가 출력되게 됩니다. 그 말은 출력이 0이면 CPU가 하드웨어 가상화를 지원하지 않는다는 의미입니다. 
```
grep -Eoc '(vmx|svm)' /proc/cpuinfo
```

다음 명령을 실행하여 KVM (Kernel-based Virtual Machime)과 추가 가상화 관리 패키지를 설치합니다. 
```
sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
```

패키지가 모두 설치되면 libvirt 데몬이 자동으로 시작되며 다음과 같이 입력하여 확인할 수 있습니다.
```
sudo systemctl is-active libvirtd
```
output: active

가상 머신을 생성하고 관리하기 위해서는 사용자를 그룹에 추가해야 합니다. 다음과 같이 입력하여 추가합니다.
```
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

자 마지막 설정입니다.
```
sudo apt-get install git wget qemu
```

### 2. xv6 설치와 빌드

환경 구축이 되었으니 우리의 목표인 xv6를 가져봅시다.
```
git clone https://github.com/mit-pdos/xv6-public.git
```

xv6를 빌드해봅시다.
```
cd xv6-public
sudo make
```

### 3. xv6 실행

완료가 되었다면, 작동을 시킵니다.
실행을 시키는 방법은 qemu와 qemu-nox가 있습니다. 
두 가지의 차이는 nox로 실행시키면 새 창을 띄우지 않고 현재 터미널에서 동작할 수 있습니다.
```
sudo make qemu
sudo make qemu-nox
```

![Alt text](/os/xv6/helloworld/img/qemu.png)

종료는 ctrl + A, X를 통해 종료시킬 수 있습니다.