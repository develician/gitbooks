# ORACLE DB 설치 및 환경 설정 절차

먼저 오라클 홈페이지에서 Oracle Database 응용프로그램 설치 프로그램을 자신의 컴퓨터 사양에 맞게 다운로드 한다.
File 1, 2 를 모두 다운로드 해야한다.

다운로드 하려면 로그인이 필요하니 없다면 계정을 생성해서 로그인을 하자.

그다음 File2 에서 나온 win64_11gR2_database_2of2\database\stage\Components 에 있는 폴더들을 winx64_12102_database_1of2\database\stage\Components 로 모두 복사시킨다.

File1 의 setup.exe 를 실행하여 설치를 시작한다.

데이터베이스 생성 및 구성에 체크한다.
데스크톱 클래스로 생성한다.
DB File 들을 담을 디렉터리와 루트 비밀번호를 설정한다.

이제 개발할 목적에 맞게 계정들을 설정해주어야 한다. 먼저 아까 만든 루트 계정으로 접속을 한다음, DB 를 만들고, 새로운 계정을 부분적으로 Admin 권한을 부여하여 만든후, 목적에 맞는 테이블들을 생성한다.

### 설치 후 계정 설정 및 Table 생성

- System 계정으로 접속.
- 계정 생성

```sql
CREATE USER ats IDENTIFIED BY 1234;
CREATE USER ats ACCOUNT UNLOCK;
```

- 권한 부여 (grant resource, DBA, connect to ats)

```sql
GRANT RESOURCE, DBA CONNECT TO ats;
```

- Table Space 및 임시 Table Space 생성.

```sql
CREATE TABLESPACE ats_data
DATAFILE 'C:\oraclexe\app\oracle\oradata\XE\ats_data.dbf' SIZE 1G
AUTOEXTEND ON NEXT 512M MAXSIZE UNLIMITED
LOGGING
ONLINE
PERMANENT
EXTENT MANAGEMENT LOCAL AUTOALLOCATE
BLOCKSIZE 8K
SEGMENT SPACE MANAGEMENT AUTO
FLASHBACK ON

CREATE TEMPORARY TABLESPACE ats_tmp
TEMPFILE 'C:\oraclexe\app\oracle\oradata\XE\ats_tmp.dbf' SIZE 512M
AUTOEXTEND ON NEXT 128M MAXSIZE UNLIMITED
```

- Table Space 에 계정 연결

```sql
ALTER USER ats DEFAULT TABLESPACE ats_data;
ALTER USER ats TEMPORARY TABLESPACE ats_tmp;
```

- 예시 테이블 생성

```sql
CREATE TABLE TB_TRD_JONGMOK (
    USER_ID VARCHAR2(8) NOT NULL,
    JONGMOK_CD CHAR(6) NOT NULL,
    JONGMOK_NM VARCHAR2(150) NOT NULL,
    PRIORITY NUMBER(9) NOT NULL,
    BUY_AMT NUMBER(15) NOT NULL,
    BUY_PRICE NUMBER(9) NOT NULL,
    TARGET_PRICE NUMBER(9) NOT NULL,
    CUT_LOSS_PRICE NUMBER(9) NOT NULL,
    BUY_TRD_YN CHAR(1) NOT NULL,
    SELL_TRD_YN CHAR(1) NOT NULL,
    INST_ID VARCHAR2(50) NOT NULL,
    INST_DTM DATE NOT NULL,
    UPDT_ID VARCHAR2(50) NOT NULL,
    UPDT_DTM DATE NOT NULL,
    CONSTRAINT TB_TRD_JONGMOK_PK PRIMARY KEY(USER_ID, JONGMOK_CD)
);
```
