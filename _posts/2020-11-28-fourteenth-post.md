---
title: "Altibase 이중화 구성"
date: 2020-11-28 22:10:00 -0900
categories: Altibase 이중화 구성
---
INFO

- Active / Standby 이중화 장비
- 방화벽 확인 필수(disable로 되어 있어야 합니다.)

방화벽 설정

- 방화벽 해제
  [altibase]$ systemctl stop firewalld

- 리부팅 시 방화벽 실행 하지 않게 하기
  [altibase]$ systemctl disable firewalld

- 방화벽 해제되었는지 확인하는 방법
  [root]# firewall-cmd --state
  not running

## 1. 이중화 생성

### 이중화 관련 프로퍼티 확인

- $ALTIBASE_HOME/conf/altibase.properties 의 REPLCATION_PORT_NO=[PORT] 변경
  ※ 이중화 장비 둘 다 같은 값(port number)으로 적용해야 합니다.

```bash
#=================================================================
# Replication Properties
#=================================================================
REPLICATION_PORT_NO                   = 21300 # REPLICATION_PORT_NO
REPLICATION_MAX_LOGFILE               = 0
REPLICATION_UPDATE_REPLACE            = 0
REPLICATION_INSERT_REPLACE            = 0
REPLICATION_CONNECT_TIMEOUT           = 10
REPLICATION_RECEIVE_TIMEOUT           = 7200
REPLICATION_SYNC_LOG                  = 0
REPLICATION_LOCK_TIMEOUT              = 5
REPLICATION_HBT_DETECT_HIGHWATER_MARK = 5
REPLICATION_HBT_DETECT_TIME           = 6
REPLICATION_PREFETCH_LOGFILE_COUNT    = 3
REPLICATION_SERVICE_WAIT_MAX_LIMIT    = 50000
REPLICATION_SENDER_SLEEP_TIME         = 10000
REPLICATION_KEEP_ALIVE_CNT            = 600
REPLICATION_ACK_XLOG_COUNT            = 100
REPLICATION_LOG_BUFFER_SIZE           = 0
REPLICATION_RECOVERY_MAX_TIME         = 4294967295
REPLICATION_RECOVERY_MAX_LOGFILE      = 0
REPLICATION_POOL_ELEMENT_SIZE         = 256
REPLICATION_POOL_ELEMENT_COUNT        = 10
REPLICATION_COMMIT_WRITE_WAIT_MODE    = 0
REPLICATION_SERVER_FAILBACK_MAX_TIME  = 4294967295
REPLICATION_FAILBACK_INCREMENTAL_SYNC = 1
REPLICATION_MAX_LISTEN                = 32
REPLICATION_SENDER_START_AFTER_GIVING_UP = 1
REPLICATION_BEFORE_IMAGE_LOG_ENABLE   = 0
REPLICATION_SENDER_COMPRESS_XLOG      = 0
REPLICATION_ALLOW_DUPLICATE_HOSTS     = 0
REPLICATION_SENDER_ENCRYPT_XLOG       = 0
```

- 변경한 프로퍼티 적용을 위해 DB 재구동

```sql
[altibase]$ server restart
```

### 이중화 대상 테이블 생성

- SYS 사용자만이 이중화 객체를 생성할 수 있습니다.
- 이중화 할 테이블에는 반드시 primary key가 필요합니다.
- 이중화 대상 테이블을 각각 생성합니다.

```sql
ex)
CREATE TABLE test1 ( c1 INTEGER PRIMARY KEY, c2 CHAR(10) ) ;
```

### 이중화 객체 생성

- IP와 REP_PORT는 상대 서버의 정보를 지정해야 합니다.
- Active인 서버에서 먼저 수행한다.
- 이중화 생성 서버(Active, Standby 서버) 양쪽에 반드시 같은 이름으로 이중화 생성해야 한다.
- CREATE REPLICATION [이중화 명] WITH [원격 서버IP], [원격 서버 PORT] FROM [USER NAME].[이중화할 TABLE NAME]  TO [USER NAME].[TABLE NAME], FROM [USER NAME].[이중화할 TABLE NAME] ... TO [USER NAME].[TABLE NAME];
  초기 생성시 여러개의 table을 replication할 수 있다.

```sql
# Active 서버에 대한 예시이다.
CREATE REPLICATION rep WITH '[replication 대상 서버]', 31300
FROM TEST_DB to TEST_DB;
```

```sql
# Standby 서버에 대한 예시이다.
CREATE REPLICATION rep WITH '[replication 대상 서버]', 31300
FROM TEST_DB to TEST_DB ;
```

### 이중화 객체 삭제

- 이중화가 개시(ALTER REPLICATION START)되어 있을 경우 사용할 수 없다.

```sql
DROP REPLICATION [REPLICATION NAME];

# 이중화가 개시되어 있을 경우 이중화 객체 삭제하는 법
ALTER REPLICATION [REPLICATOIN NAME] STOP;
DROP REPLICATION [REPLICATION NAME];
```

## 2. 이중화 SYNC

### 이중화 시작

- 이중화 생성 서버에 각각 이중화를 시작합니다.

```sql
# Active Server에서 수행한다.
ALTER REPLICATION [이중화 명] SYNC;

#데이터 복제가 정상적으로 수행되었는지 Standby Server에서 확인 후 Standby Server에서 수행
ALTER REPLICATION [이중화 명] START;
```

## -  장애 시 자동 복구 확인

### 종료

```sql
server stop
```

## -  이중화 대상 테이블에 DDL 실행

- STEP1: 서비스 중지(트랜잭션이 발생하지 않도록 조치)

```sql
iSQL> select count(*) from v$session;
COUNT(*)             
-----------------------
1                    
1 row selected.

iSQL> select count(*) from v$statement where execute_flag =1 ;
COUNT(*)             
-----------------------
1                    
1 row selected.
```

- STEP2: 대상 노드의 이중화 갭이 모두 "0"임을 확인

```sql
# DDL 수행 대상 테이블이 속해 있는 이중화 객체 확인
select REPLICATION_NAME,LOCAL_USER_NAME, LOCAL_TABLE_NAME from SYSTEM_.SYS_REPL_ITEMS_;

# 이중화 갭 확인
iSQL> SELECT rep_name, rep_gap FROM v$repgap;      # rep_gap 이 모두 0 임을 확인.
REP_NAME                                  REP_GAP              
------------------------------------------------------------------
REP                                       0
```

- STEP3: 대상 노드의 이중화를 중지(STEP3 이후 STEP들은 Active , Standby 장비 전부다 수행)

```sql
iSQL>  ALTER REPLICATION [REPLICATION NAME] STOP;   # 이중화 객체(REP_NAME) 은  STEP 2에서 확인
```

- STEP4: 이중화 객체에서 DDL을 수행하려는 대상 테이블을 제거

```sql
iSQL> ALTER REPLICATION [REPLICATION NAME] DROP TABLE FROM  [USER NAME].[이중화할 TABLE NAME] TO [USER NAME].[이중화할 TABLE NAME];
```

- STEP5: DDL 작업 수행
- STEP6: STEP4에서 제거한 이중화 대상 테이블을 다시 이중화 객체 리스트에 추가
      한 개 씩 table을 add 하여야 한다.

```sql
iSQL> ALTER REPLICATION [REPLICATION NAME] ADD TABLE FROM [USER NAME].[이중화할 TABLE NAME] to [USER NAME].[TABLE NAME];
```

- STEP7: 대상 노드에서 이중화 시작

```sql
iSQL>  ALTER REPLICATION [REPLICATION NAME] START;
```

## - 이중화 대상 테이블의 primary key에 대한 sequence 실행 방법

- sequence 생성

```sql
# sequence는 Active, Standby 서버에 각각 생성한다.
# sequence는 이중화 대상이 아니므로 Active는 짝수, Standby는 홀수로 시작되게 생성하며,
# 서로 key 값이 중복되면 안 되므로, 2씩 증가하는 sequence를 각각 생성해야 합니다.

create sequence [SEQUENCE NAME]
start with [시작 값] # (Active는 0, Standby는 1)
increment by 2
minvalue [최소 값]
maxvalue [최대 값];
```

이중화가 중지되어도 이중화로 맺어져 있는 테이블들이 변동이 일어나면 로그에 전부 기록되고 재시작하면 로그에 쌓인 기록을 읽어 싱크를 맞춰주니 운영중에 STOP 해도 된다.
