---
title: "Altibase 설정 권고 파라미터 종류"
date: 2020-12-12 21:00:00 -0900
categories: Altibase 설정 권고 파라미터 종류
---

## 설정 권고 파라미터 종류

설정 변경은 root계정에서 실행합니다.

### - CPU frequency Governor

- VM에서는 CPU governor가 필요 없다고 생각합니다. 왜냐하면 CPU governor는 호스트에서 수행해야 하기 때문입니다. 그렇기에 아래 절차는 생략하도록 하겠습니다.
- 권고값: performance

```bash
# CPU frequency Governor 설정 확인 방법
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor | sort -u performance
sort: cannot read: performance: No such file or directory
cat: /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor: No such file or directory

# CPU frequency Governor 즉시 변경1
# CPU 코어 수만큼 아래 명령어를 반복 수행한다.
echo performance > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
echo performance > /sys/devices/system/cpu/cpu1/cpufreq/scaling_governor

# CPU frequency Governor 즉시 변경2
cpupower frequency-set -g performance

# CPU frequency Governor 설정 영구 적용 방법
# /etc/rc.d/rc.local 파일에 CPU 코어 수만큼 아래 명령어를 추가한다.
echo performance > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
echo performance > /sys/devices/system/cpu/cpu1/cpufreq/scaling_governor
```

### - RemoveIPC

IPC란?
Inter-Process Communication의 약어로, 프로세스들 사이에 서로 데이터를 주고받는 행위 또는 경로를 뜻한다.

- OS 사용자가 세션을 종료할 때 System V IPC와 POSIX IPC 객체를 제거하는 리눅스
- 권고값: no
           : 기본 설정이 yes의 경우 IPC를 사용하는 Altibase 환경에서 강제적인 세마포어 할당, 반환으로 Altibase 서버 및 애플리케이션 비정상 종료 현상이 발생한 사례가 보고되고 있음.

```bash
# RemoveIPC 설정값 확인 방법
grep RemoveIPC /etc/systemd/logind.conf RomoveIPC=no
/etc/systemd/logind.conf:#RemoveIPC=no
/etc/systemd/logind.conf:RemoveIPC=no
grep: RomoveIPC=no: No such file or directory

# RemoveIPC 설정값 즉시 변경
# /etc/systemd/logind.conf 파일을 편집기로 열고 RemoveIPC=no 로 변경
# This file is part of systemd.
...
[Login]
...
RemoveIPC=no

# RemoveIPC 변경 후 적용
systemctl restart systemd-logind.service
```

### - swappiness

swapping이란?
- 물리 메모리가 부족한 상황에서 사용 빈도가 낮은 물리 메모리 영역을 스왑 영역으로 내리는 작업(스왑 아웃)을 의미한다.
- 스왑은 디스크를 메모리처럼 사용하기 위한 것으로 스왑 아웃이 발생하면 DISK I/O로 인한 시스템 성능 저하가 발생한다.

- swapping 발생 시 디스크 I/O로 인한 시스템 성능 저하가 Altibase 서버 성능에 영향을 미치는 것을 최소화 하기 위해 권장하는 커널 파라미터이다.
- 권고값: 1
            : swapiness를 비활성화하지 않고 스와핑을 최소화하기 위한 설정
- 낮은 값은 커널이 가급적 페이지 캐시의 페이지를 사용하며 높은 값은 물리 메모리에서 사용 빈도가 낮은 페이지를 스왑 아웃하는 것을 선호한다.

페이지 캐시란?
파일 I/O 성능 향상을 위해 Linux에서 관리하는 메모리 영역이다.

```bash
# swappiness 설정 확인 방법
sysctl -a | grep swappiness
sysctl: reading key "net.ipv6.conf.all.stable_secret"
sysctl: reading key "net.ipv6.conf.default.stable_secret"
sysctl: reading key "net.ipv6.conf.eth0.stable_secret"
sysctl: reading key "net.ipv6.conf.eth1.stable_secret"
sysctl: reading key "net.ipv6.conf.eth2.stable_secret"
sysctl: reading key "net.ipv6.conf.eth3.stable_secret"
sysctl: reading key "net.ipv6.conf.eth4.stable_secret"
sysctl: reading key "net.ipv6.conf.eth5.stable_secret"
sysctl: reading key "net.ipv6.conf.eth6.stable_secret"
sysctl: reading key "net.ipv6.conf.lo.stable_secret"
vm.swappiness = 30

# swappiness 즉시 변경1
echo 1 > /proc/sys/vm/swappiness

# swappiness 즉시 변경2
sysctl -w vm.swappiness=1

# swappiness 영구 적용 방법
$ vi /etc/sysctl.conf
vm.swappiness = 1 # sysctl.conf 파일에 vm.swappiness = 1 를 추가하거나 값을 변경한다.
```

### - THP(Transparent Huge Pages)

THP란?
대용량 사이즈의 페이지를 응용프로그램이 필요로 할 때, 관리자의 특정한 설정 없이 가장 활성화 되어 있는 프로레서로 부터 대용량 사이즈의 페이지를 보다 쉽게 접근하도록 허용하기 위한 기능이다.

Page란?
메모리를 일정한 크기로 나누어 관리하는 단위

- 대량의 메모리 관리를 위해 Linux에서 채택한 방법으로 4096바이트 단위의 메모리 페이지를 2MB 또는 1GB 단위로 확대하는 기능을 자동화하는 설정이다.
- 권고값: never

```bash
# THP 설정 확인 방법1
cat /sys/kernel/mm/transparent_hugepage/enabled 
[always] madvise never

# THP 설정 확인 방법2
grep -i huge /proc/meminfo 
AnonHugePages:   1126400 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB

# THP  즉시 변경1
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag

# THP   설정 영구 적용 방법 - /etc/grub.conf
vi /etc/grub.conf 
default=0
timeout=5
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu
title Red Hat Enterprise Linux 6 (2.6.32-504.el6.x86_64)
 root (hd0,0)
 # 아래 kernel 마지막 부분에 transparent_hugepage=never를 추가한다.
 kernel /vmlinuz-2.6.32-504.el6.x86_64 ro root=/dev/mapper/vg_os-lv_os rd_NO_LUKS
LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto
rd_LVM_LV=vg_os/lv_os KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet
transparent_hugepage=never
 initrd /initramfs-2.6.32-504.el6.x86_64.img
```

### - 공유메모리

- 한 서버에서 여러 응용 프로그램이 서로 정보를 주고받아야 하는 경우 필요한 커널 파라미터이다.
 - Altibase 서버와 클라이언트의 통신 방식이 IPC 또는 IPCDA 타입인 경우
 - 두 개 이상의 Altibase 응용 프로그램이 IPC로 통신하는 경우
- 권고값: shmmni: 4096 / shmmax: 217483648

```bash
# 공유 메모리 설정 확인 방법
ipcs -m -l

------ Shared Memory Limits --------
max number of segments = 4096
max seg size (kbytes) = 18014398509465599
max total shared memory (kbytes) = 18014398442373116
min seg size (bytes) = 1

# 공유 메모리 즉시 변경
echo 4096 > /proc/sys/kernel/shmmni
echo 2147483648 > /proc/sys/kernel/shmmax

# 공유 메모리 영구 적용 방법- /etc/sysctl.conf
vi /etc/sysctl.conf
kernel.shmmni = 4096
kernel.shmmax = 2147483648
```

### - 세마포어

- Altibase 서버와 클라이언트의 통신 방식이 IPC 또는 IPCDA 타입인 경우 프로세스 간 동기화 구현하기 위해 필요한 커널 파라미터이다.
- 권고값: semmsl:2000 / semmns:32000 / semopm: 512 / semmni: 5029

```bash
# 세마포어 확인 방법
ipcs -s -l

------ Semaphore Limits --------
max number of arrays = 128
max semaphores per array = 250
max semaphores system wide = 32000
max ops per semop call = 32
semaphore max value = 32767

# 세마포어 즉시 변경
echo 2000 32000 512 5029 > /proc/sys/kernel/sem

# 세마포어 영구 적용 방법- /etc/sysctl.conf
vi /etc/sysctl.conf
kernel.shmmni = 4096
kernel.sem = 20000 32000 512 5029
```

[Replication](https://www.notion.so/Replication-70a9f70abac849d78e1e8f031cd53772)

CREATE MEMORY TABLESPACE [TABLESPACE NAME]
SIZE [크기]
AUTOEXTEND ON NEXT [자동 증가 크기] MAXSIZE [최대 크기]
CHECKPOINT PATH '[저장위치]','[저장위치]','[저장위치]'
SPLIT EACH [파일분할크기];

CREATE DISK DATA TABLESPACE [TABLESPACE NAME]
DATAFILE '[저장위치]' SIZE [크기],
'[저장위치]' SIZE [크기],
'[저장위치]' SIZE [크기]
AUTOEXTEND ON NEXT [자동 증가 크기] MAXSIZE [최대 크기];
