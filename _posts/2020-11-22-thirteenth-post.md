---
title: "Altibase 설치"
date: 2020-11-22 20:15:00 -0900
categories: Altibase 설치
---

# Altibase 설치 정보
- Altibase: 6.5.1.7.1
- Oracle Linux: 10.200.2.130 / 10.200.2.136 / 10.200.2.148 / 172.16.104.12
- glibc: 2.12~2.20
          glibc-2.12-1.166.el6_7.1이상 권고-이전 버전의 glibc는 시스템 콜(malloc/free) 함수가 
          race condition으로 인해 deadlock이 발생할수 있는 버그가 존재한다.
- license: 509586B9E3D30EB07577AA9637550C9B598DEC07BD8F710DA82D069192EC1C19

![Altibase%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8E%E1%85%B5%205b544edafb544b6b926775f28ef79ff7/Untitled.png](Altibase%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8E%E1%85%B5%205b544edafb544b6b926775f28ef79ff7/Untitled.png)

> Glibc란?
  'GNU C 라이브러리'의 약자로, GNU시스템과 리눅스에서 사용하는 대표적인 C 라이브러리이다.

> race condition이란?
  여러 개의 스레드가 공통 자원을병행적으로 읽거나 쓰는 상황

## 1. Altibase 설치하기
> license 가 없는 경우 Altibase 홈페이지에서 90일짜리 free license 신청

- OS 유저 생성

```bash
# Linux는 사용자 그리고 사용자가 속할 그룹을 관리
groupadd dba
useradd -d /home/altibase -g dba altibase
passwd altibase

# altibase 계정이 생성되어있는지 확인
cd /home

total 4
drwx------  3 altibase dba   78 Jan  7 11:28 altibase
drwx------. 3 si       si  4096 Dec  4 20:55 si
```

- 실행 권한 부여

```bash
# root계정에서 해당 실행파일을 권한을 준다.
cp altibase-HDB-server-6.5.1.7.1-LINUX-X86-64bit-release.run /home/altibase
chmod +x ./altibase-HDB-server-6.5.1.7.1-LINUX-X86-64bit-release.run

# altibase user로 로그인
su - altibase
```

### Altibase 설치 파일 실행

```bash
./altibase-HDB-server-6.5.1.7.1-LINUX-X86-64bit-release.run
```

아래는 Altibase 설치 파일 실행 과정을 나타낸 것입니다.

```
---

Welcome to the ALTIBASE HDB Server 6.5.1.7.1 setup wizard.

---

Installation Directory

Please specify the installation directory for ALTIBASE HDB Server 6.5.1.7.1

Installation directory [/home/altibase/altibase-HDB-server-6.5.1]: /home/altibase/altibase_home

Please select the installation type.

Installation type

[1] Full installation: full package install
[2] Patch: patch package install

Please choose an option [1] : 1

---

Pre-Installation Requirements for ALTIBASE HDB

It is first necessary to set your system environment to ensure that ALTIBASE HDB
will run properly. Before installing ALTIBASE HDB, the kernel parameter values
must be set using the root user account. The kernel parameter values may be
modified after installation; however, they must be set prior to starting
ALTIBASE HDB.

Please refer to the installation manual and pre_install.sh script file.
(pre_install.sh: '$Altibase_install_dir'/install/pre_install.sh)

================ LINUX ================
[ How to modify kernel parameter values ]

echo 512 32000 512 512 > /proc/sys/kernel/sem
echo 872415232 > /proc/sys/kernel/shmall

# shmall

If it is desired to use ALTIBASE HDB in shared memory mode, the value of
'shmall' must be set. This value determines the maximum size of an Altibase
database.

Press [Enter] to continue :
These values must be set in order for ALTIBASE HDB to operate properly.
They must be set such that they are suitable for the system configuration.

=====================================

Press [Enter] to continue :

---

ALTIBASE HDB Property Settings
Step 1: Basic Database Operation Properties

Database name [mydb]:

ALTIBASE HDB connection port number (1024-65535) [20300]:

Maximum size of memory database

- MIN value: 16M (K = kB, M = MB, G = GB) [2G]:

Buffer area size for caching disk-based database pages

- MIN value: 1M (K = kB, M = MB, G = GB) [128M]:

Do you want to create a database after the installation process is complete?

[1] YES
[2] NO
Please choose an option [1] : 1

---

ALTIBASE HDB Property Settings
Step 2: Database Creation Properties

Initial database size

- 4M-2G (K = kB,M = MB,G = GB) [10M]:

Database archive logging mode

[1] No archivelog
[2] Archivelog
Please choose an option [1] : 1

Database character set

[1] UTF-8
[2] MS949
[3] US7ASCII
[4] KO16KSC5601
[5] BIG5
[6] GB231280
[7] MS936
[8] SHIFT-JIS
[9] EUC-JP
[10] MS932
Please choose an option [1] : 1

National character set

[1] UTF-8
[2] UTF-16
Please choose an option [1] : 1

---

ALTIBASE HDB Property Settings
Step 3: Set Database Directories

Default disk database directory [/home/altibase/altibase_home/dbs]:

Memory database directory [/home/altibase/altibase_home/dbs]:

Archive log directory [/home/altibase/altibase_home/arch_logs]:

Transaction log directory [/home/altibase/altibase_home/logs]:

[Log Anchor file directories ]
ALTIBASE HDB maintains three sets of log anchor files. These files contain
important information
about the database. By default, they are located in the "logs" folder.
The location can be changed here or by modifying the contents of the ALTIBASE
HDB properties file,
which is named "altibase.properties".

Directory 1. [/home/altibase/altibase_home/logs]:

Directory 2. [/home/altibase/altibase_home/logs]:

Directory 3. [/home/altibase/altibase_home/logs]:

Warning: The directory
/
is not writable by the current user
Press [Enter] to continue :

---

Property Review

Please check your property settings.

To change these properties after installation is complete,
please modify the following file:
/home/altibase/altibase_home/conf/altibase.properties.

1. ALTIBASE HDB Property Settings:
Step 1: Basic Database Operation Properties

    1) Database name:
        [mydb]

    2) ALTIBASE HDB connection port number (1024-65535):
        [20300]

    3) Maximum size of memory database:
        [2G]

    4) Buffer area size for caching disk-based database pages:
        [128M]

ALTIBASE HDB Property Settings:

2. ALTIBASE HDB Property Settings:
Press [Enter] to continue :
  Step 2: Database Creation Properties

1) Initial database size
    [10M]

2) Database archive logging mode
    [noarchivelog]

3) Database character set
    [UTF8]

4) National character set
    [UTF8]

3. ALTIBASE HDB Property Settings:
Step 3: Set Database Directories

The database will not operate properly if any of these directories are
removed.

1) Disk database directory:
    [/home/altibase/altibase_home/dbs]

Press [Enter] to continue :
2) Memory database directory:
   [/home/altibase/altibase_home/dbs]

3) Archive log directory: 
     [/home/altibase/altibase_home/arch_logs] 
4) Transaction log directory: 
     [/home/altibase/altibase_home/logs] 
5) Log Anchor file directories:
     Directory 1: 
     [/home/altibase/altibase_home/logs] 

     Directory 2: 
     [/home/altibase/altibase_home/logs] 

     Directory 3: 
     [/home/altibase/altibase_home/logs]

Press [Enter] to continue :

---

Setup is now ready to install ALTIBASE HDB Server 6.5.1.7.1.

Do you want to continue? [Y/n]: Y

---

Please wait until the setup wizard finishes installing ALTIBASE HDB Server
6.5.1.7.1.

Installing
0% ______________ 50% ______________ 100%
#########################################

---

ALTIBASE HDB License

If a license has not been issued or if it has expired,
ALTIBASE HDB services will not start.

If this is the case, Please visit [http://support.altibase.com](http://support.altibase.com/)

Choose an option for ALTIBASE HDB license registration.

[1] I will input a license key.
[2] I will select a license file.
[3] I want to register an ALTIBASE HDB license later.
Please choose an option [1] : 1

---

ALTIBASE HDB License

Please enter your ALTIBASE HDB license key.

[]: 509586B9E3D30EB07577AA9637550C9B598DEC07BD8F710DA82D069192EC1C19

---

ALTIBASE HDB Quick Setting Guide

[ Installation complete ]
Please refer to the file listed below to verify the ALTIBASE HDB version.
/home/altibase/altibase_home/APatch/patchinfo

[ Quick Guide to Making Settings in ALTIBASE HDB ]

1. Set kernel variables using the root user account.
run the '/home/altibase/altibase_home/install/pre_install.sh' file

    - This script helps you make kernel parameter settings.

================ LINUX ================
[ How to modify kernel parameter values ]

echo 512 32000 512 512 > /proc/sys/kernel/sem
echo 872415232 > /proc/sys/kernel/shmall

# shmall

If it is desired to use ALTIBASE HDB in shared memory mode, the value of
'shmall' must be set. This value determines the maximum size of an Altibase
database.
Press [Enter] to continue :

These values must be set in order for ALTIBASE HDB to operate properly.
They must be set such that they are suitable for the system configuration.

=====================================

1. Provide a license.
Please rename and locate the license file as shown below.
/home/altibase/altibase_home/conf/license

    If no license file has been issued or if the license file has expired,
    ALTIBASE HDB services will not start.
    In this case, please visit [http://support.altibase.com](http://support.altibase.com/)

2. Configure user environment variables (using the user account with which
ALTIBASE HDB was installed).
Run the '/home/altibase/altibase_home/install/post_install.sh' file
under the account with which ALTIBASE HDB was installed.

Press [Enter] to continue :
This script performs necessary post-installation configuration.

1) Create the ALTIBASE HDB user environment file and apply it to the user

profile.
(/home/altibase/altibase_home/conf/altibase_user.env)
2) Create a database.

If you selected 'YES' in response to the question about whether to

create
a database after installation, at "ALTIBASE HDB Property setting step
1",
a database will be automatically created.

If you selected 'NO' in response to this question,
     you need to create a database manually.

     shell> server create [DB Character Set] [National Character Set]

4. Start up and shut down the server
    shell> server start
    shell> server stop

Press [Enter] to continue :

5. Connect to the database using iSQL
    shell> isql -s 127.0.0.1 -u SYS -p MANAGER

Press [Enter] to continue :

Would you like to launch 'post_install.sh' now ?

- This will create the ALTIBASE HDB database. [Y/n]: Y

Result

---

     Altibase Client Query utility.
     Release Version 6.5.1.7.1
     Copyright 2000, ALTIBASE Corporation or its subsidiaries.
     All Rights Reserved.

---

ISQL_CONNECTION = UNIX, SERVER = localhost
[ERR-910FB : Connected to idle instance]
Connecting to the DB server.... Connected.

TRANSITION TO PHASE : PROCESS
Command executed successfully.

DB Info (Page Size = 32768)
(Page Count = 257)
(Total DB Size = 8421376)
(DB File Size = 1073741824)

Creating MMDB FILES     [SUCCESS]
Creating Catalog Tables [SUCCESS]

Press [Enter] to continue :
Creating DRDB FILES [SUCCESS]

[SM] Rebuilding Indices [Total Count:0] [SUCCESS]

DB Writing Completed. All Done.

Create success.

Press [Enter] to continue :

---

Setup has finished installing the ALTIBASE HDB Server 6.5.1.7.1 on your host.

Info: [Linux Env.]
Target : /home/altibase/altibase_home/conf/altibase_user.env

created  ----------------------- Altibase environment setup file.
    added    ----------------------- ALTIBASE_HOME
    added    ----------------------- PATH
    added    ----------------------- LD_LIBRARY_PATH
    added    ----------------------- CLASSPATH

    export ----------------------- 'altibase_user.env'
    into '/home/altibase/.bash_profile'

==============================================
Please perform [re-login]
or [source /home/altibase/.bash_profile]
or [. /home/altibase/.bash_profile]

==============================================

Press [Enter] to continue :
```

### Altibase 설치 완료 확인

알티베이스 설치가 완료되면 $ALTIBASE_HOME 디렉토리 안에 bin, conf, lib, include, msg, dbs, logs, sample, install, audit, trc, admin, arch_logs  등의 디렉토리가 생성이된다.

### 시스템 환경변수 등록

```bash
# Altibase 서버 캐릭터셋이 UTF8인 경우 예제이다.
# /home/altibase/.bash_profile에 아래 내용을 추가하고 저장한다.
export ALTIBASE_NLS_USE=UTF8
export LANG=ko_KR.utf8
```

```bash
# 환경설정 파일(.bash_profile)을 적용한다.
# /home/altibase(altibase 계정에서의 최상위 폴더)에서 실행한다.
# bash_profile은 히든파일이므로 ls-al 명령어를 통하여 확인 가능하다.
source ./.bash_profile
​
# 알티베이스 사용을 위한 환경을 담고 있다.
source /home/altibase/altibase_home/conf/altibase_user.env
```

- 필수 환경변수가 올바르게 설정되어 있는지 확인한다.

```bash
env

XDG_SESSION_ID=1190
HOSTNAME=es01
SHELL=/bin/bash
TERM=ansi
HISTSIZE=1000
OLDPWD=/home/altibase
USER=altibase
LD_LIBRARY_PATH=/home/altibase/altibase_home/lib:/home/altibase/altibase_home/lib:/home/altibase/altibase_home/lib:
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=01;05;37;41:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.axv=01;35:*.anx=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=01;36:*.au=01;36:*.flac=01;36:*.mid=01;36:*.midi=01;36:*.mka=01;36:*.mp3=01;36:*.mpc=01;36:*.ogg=01;36:*.ra=01;36:*.wav=01;36:*.axa=01;36:*.oga=01;36:*.spx=01;36:*.xspf=01;36:
MAIL=/var/spool/mail/altibase
PATH=/home/altibase/altibase_home/bin:/home/altibase/altibase_home/bin:/home/altibase/altibase_home/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/altibase/.local/bin:/home/altibase/bin
PWD=/home/altibase/altibase_home
LANG=ko_KR.utf8
ALTIBASE_NLS_USE=UTF8
HISTCONTROL=ignoredups
SHLVL=1
HOME=/home/altibase
LOGNAME=altibase
CLASSPATH=/home/altibase/altibase_home/lib/Altibase.jar:/home/altibase/altibase_home/lib/Altibase.jar:/home/altibase/altibase_home/lib/Altibase.jar:
LESSOPEN=||/usr/bin/lesspipe.sh %s
ALTIBASE_HOME=/home/altibase/altibase_home
_=/bin/env

echo $ALTIBASE_HOME
```

### altibase server가 살아있는지 확인

```bash
[altibase@es01 ~]$ ps -ef | grep alti
root     17427 14369  0 15:29 pts/0    00:00:00 su - altibase
altibase 17428 17427  0 15:29 pts/0    00:00:00 -bash
altibase 18440 17428  0 15:57 pts/0    00:00:00 ps -ef
altibase 18441 17428  0 15:57 pts/0    00:00:00 grep --color=auto alti
altibase 19875     1  0  1월09 ?      01:12:43 /home/altibase/altibase_home/bin/altibase -p boot from admin
altibase 20232     1  6  1월09 ?      11:00:55 /home/altibase/altibase_home/bin/altibase -p boot from admin
```

### isql sysdba 권한으로 접속하여 DB 생성

isql -u sys -p manager -sysdba

Altibase Client Query utility.
 Release Version 6.5.1.3.9
 Copyright 2000, ALTIBASE Corporation or its subsidiaries.
 All Rights Reserved.

---

ISQL_CONNECTION = UNIX, SERVER = localhost
[ERR-910FB : Connected to idle instance]
iSQL(sysdba)> startup process
Connecting to the DB server.... Connected.

TRANSITION TO PHASE : PROCESS
Command executed successfully.

※아래 sql문의 mydb는 /home/altibase/altibase_home/conf의 DB_NAME의 값과 동일해야한다.
iSQL(sysdba)> create database mydb initsize=1024M noarchivelog character set utf8 national character set utf8;

create database mydb initsize=10M noarchivelog character set utf8 national character set utf8;

DB Info (Page Size = 32768)
(Page Count = 131073)
(Total DB Size = 4295000064)
(DB File Size = 1073741824)

Creating MMDB FILES     [SUCCESS]
    Creating Catalog Tables [SUCCESS]
    Creating DRDB FILES     [SUCCESS]

[SM] Rebuilding Indices [Total Count:0] [SUCCESS]

DB Writing Completed. All Done.

Create success.

### altibase.properties 에서 변경 내용

- MAX_STATEMENTS_PER_SESSION = 65536
QUERY_TIMEOUT = 3600

### Altibase DB 구동

isql -u sys -p manager -sysdba

iSQL(sysdba)> startup service
The database server is already up and running.

TRANSITION TO PHASE : CONTROL

TRANSITION TO PHASE : META
    [SM] Recovery Phase - 1 : Preparing Database
                                            : Dynamic Memory Version => Parallel Loading
    [SM] Recovery Phase - 2 : Loading Database
    [SM] Recovery Phase - 3 : Skipping Recovery & Starting Threads...
Refining Disk Table
    [SM] Refine Memory Table : ........................................................................................................................................... [SUCCESS]
    [SM] Rebuilding Indices [Total Count:137] ......................................................................................................................................... [SUCCESS]

TRANSITION TO PHASE : SERVICE
    [CM] Listener started : TCP on port 20300 [IPV4]
    [CM] Listener started : UNIX
    [CM] Listener started : IPC
    [RP] Initialization : [PASS]

--- STARTUP Process SUCCESS ---
Command executed successfully.

### Altibase server start/stop하는 방법

- isql(sysdba계정)으로 접속하여 실행하는 방법

```bash
# altibase server start
iSQL(sysdba)> startup
Connecting to the DB server.... Connected.

TRANSITION TO PHASE : PROCESS

TRANSITION TO PHASE : CONTROL

TRANSITION TO PHASE : META
  [SM] Recovery Phase - 1 : Preparing Database
                          : Dynamic Memory Version => Parallel Loading
  [SM] Recovery Phase - 2 : Loading Database 
  [SM] Recovery Phase - 3 : Skipping Recovery & Starting Threads...
                            Refining Disk Table 
  [SM] Refine Memory Table : ........................................................................................................................................... [SUCCESS]
  [SM] Rebuilding Indices [Total Count:137] ......................................................................................................................................... [SUCCESS]

TRANSITION TO PHASE : SERVICE
  [CM] Listener started : TCP on port 20300 [IPV4]
  [CM] Listener started : UNIX
  [CM] Listener started : IPC
  [RP] Initialization : [PASS]

--- STARTUP Process SUCCESS ---
Command executed successfully.

# altibase server stop
iSQL(sysdba)> alter database mydb shutdown immediate;  
Ok..Shutdown Proceeding....

TRANSITION TO PHASE : Shutdown Altibase
  [RP] Finalization : PASS
Alter success.
```

- Altibase script를 이용하여 실행하는 방법

```bash
# altibase server stop
[altibase@ktp-dbu-ext-1a ~]$ server stop
-----------------------------------------------------------------
     Altibase Client Query utility.
     Release Version 6.5.1.7.1
     Copyright 2000, ALTIBASE Corporation or its subsidiaries.
     All Rights Reserved.
-----------------------------------------------------------------
ISQL_CONNECTION = UNIX, SERVER = localhost
Ok..Shutdown Proceeding....

TRANSITION TO PHASE : Shutdown Altibase
  [RP] Finalization : PASS
shutdown immediate success.

# altibase server start
[altibase@ktp-dbu-ext-1a ~]$ server start
-----------------------------------------------------------------
     Altibase Client Query utility.
     Release Version 6.5.1.7.1
     Copyright 2000, ALTIBASE Corporation or its subsidiaries.
     All Rights Reserved.
-----------------------------------------------------------------
ISQL_CONNECTION = UNIX, SERVER = localhost
[ERR-910FB : Connected to idle instance]
Connecting to the DB server.... Connected.

TRANSITION TO PHASE : PROCESS

TRANSITION TO PHASE : CONTROL

TRANSITION TO PHASE : META
  [SM] Recovery Phase - 1 : Preparing Database
                          : Dynamic Memory Version => Parallel Loading
  [SM] Recovery Phase - 2 : Loading Database 
  [SM] Recovery Phase - 3 : Skipping Recovery & Starting Threads...
                            Refining Disk Table 
  [SM] Refine Memory Table : ........................................................................................................................................... [SUCCESS]
  [SM] Rebuilding Indices [Total Count:137] ......................................................................................................................................... [SUCCESS]

TRANSITION TO PHASE : SERVICE
  [CM] Listener started : TCP on port 20300 [IPV4]
  [CM] Listener started : UNIX
  [CM] Listener started : IPC
  [RP] Initialization : [PASS]

--- STARTUP Process SUCCESS ---
Command executed successfully.
```

### Altibase Database 삭제

```bash
DROP DATABASE [DATABASE NAME];
```

### Altibase Database 확인

```bash
SELECT * FROM V$DATABASE;
```

### Altibase User 생성

```bash
CREATE USER [USER NAME] IDENTIFIED BY [PASSWORD];
```

### Altibase User 삭제

```bash
DROP USER [USER NAME] CASCADE;
```

### Altibase User 확인

```bash
SELECT * FROM SYSTEM_.SYS_USERS_;
```
