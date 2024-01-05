# CentOS Oracle 12c 설치 가이드

## 패키지 설치

```
yum update -y               # (설치 시 YES로 응답)
yum install -y binutils compat-libcap1 gcc gcc-c++ glibc glibc glibc-devel
glibc-devel ksh compat-libstdc++-33 libaio libaio libaio-devel libaio-devel
libgcc libgcc libstdc++ libstdc++ libstdc++-devel libstdc++-devel libXi libXi
libXtst libXtst make sysstat xorg-x11-apps
```
## Group과 User 생성

```
groupadd oinstall(그룹 생성)
groupadd dba(그룹 생성)
useradd -g oinstall -G dba oracle(oracle 사용자를 그룹에 귀속)
passwd oracle
```


## 디렉토리 생성 후 권한 부여

```
cd /home/oracle
mkdir db(폴더 생성)
chown -R oracle:oinstall db(-R 하위 디렉터리 모두 적용, 사용자:그룹 )
chmod -R 775 db (권한 부여 all all read,Execute)
chmod g+s db(부모 디렉터리의 그룹의 소유권을 상속)
```


## 환경 변수 설정

```
vi /home/oracle/.bash_profile
 
 
export TMP=/tmp
export TMPDIR=/tmp
export ORACLE_BASE=/home/oracle/db
export ORACLE_SID=orcl
export ORACLE_HOME=$ORACLE_BASE/product/12.1.0/dbhome_1
export ORACLE_HOME_LISTNER=$ORACLE_HOME/bin/lsnrctl
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
PATH=$PATH:$HOME/.local/bin:$HOME/bin
export PATH=$ORACLE_HOME/bin:$PATH
```


## 커널 설정 수정

```
vi /etc/sysctl.conf
 
 
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 1987162112
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
```


## 커널 수정한 내용 적용 및 확인

```
sysctl -p
```


## shell limit를 설정

```
vi /etc/security/limits.conf
 
 
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
```


## Oracle Install
1.	오라클 회원가입
   
2.	https://edelivery.oracle.com/osdc/faces/SoftwareDelivery 접속 후 12c 설치
   
Oracle site에서 다운받은 Oracle 설치파일을 unzip으로 해제 후 Oracle 계정으로 접속 한다.

3. unzip linuxx64_12201_database.zip
-	관리자 계정으로 가서 yum install unzip
  
-	unzip aaa.zip -d ./target
  
=> 폴더 이동시 사용 현재 오라클 폴더는 접근이 불가하기 때문에 다른 디렉에 압축파일을 놓았고 그것을 오라클에 해제하였다.

```
su - oracle
cd /home/oracle/database
```


## Oracle 실행

```
./runinstaller
```

## 화면에 GUI 화면이 안 나타난다면 oracle 서버(X11 클라이언트)에서 아래와 같이 실행 한다.  

1.	xorg-x11-server-utils
2.	yum groupinstall "Server with GUI"
3.	xming 설치 후 재실행 -

아래 링크를 참조하여 작성

https://confluence.curvc.com/pages/viewpage.action?pageId=11927710 
