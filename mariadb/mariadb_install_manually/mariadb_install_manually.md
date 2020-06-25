# 설치 폴더 구성
```
# 일부 폴더, 파일 내용 생략
├── bin  <-- maria db 시작, 중지 등
│   ├── mysql_client
│   ├── mysql_start
│   ├── mysql_status
│   └── mysql_stop
├── data  <-- db 데이터 설치 장소
├── log   <-- 로그 저장 폴더
│   └── mysqld.log
├── mariadb-10.5.4-linux-x86_64  <-- maria db 실행 파일
│   ├── bin
└── var  <-- 설정파일, sock, pid 파일
    ├── my.cnf
    ├── mysqld.pid
    └── mysqld.sock
```

# mariad db 실행 종료 스크립트
```
[mysql@localvm ~/mariadb/bin] cat mysql_client
/home/mysql/mariadb/mariadb-10.5.4-linux-x86_64/bin/mysql --defaults-file=/home/mysql/mariadb/var/my.cnf

[mysql@localvm ~/mariadb/bin] cat mysql_start
/home/mysql/mariadb/mariadb-10.5.4-linux-x86_64/bin/mysqld_safe --defaults-file=/home/mysql/mariadb/var/my.cnf &

[mysql@localvm ~/mariadb/bin] cat mysql_status
while true; do sleep 2; clear; date; ps -ef | grep -v grep | grep -v ssh | grep -v bash | grep mysql; netstat -anopt | grep 3306; done

[mysql@localvm ~/mariadb/bin] cat mysql_stop
kill `cat /home/mysql/mariadb/var/mysqld.pid`
```

# my.cnf 예제
```
[mysqld]
skip-grant-tables
max_connections=1000
wait_timeout=200
interactive_timeout=200
user			=mysql
port			=3306
basedir			=/home/mysql/mariadb/mariadb-10.5.4-linux-x86_64
datadir			=/home/mysql/mariadb/data
socket			=/home/mysql/mariadb/var/mysqld.sock

[mysqld_safe]
log-error		=/home/mysql/mariadb/log/mysqld.log
pid-file		=/home/mysql/mariadb/var/mysqld.pid

[client]
user			=root
password		=123123
port			=3306
socket			=/home/mysql/mariadb/var/mysqld.sock
```

# 디비 data 폴더 초기화
```
./scripts/mysql_install_db --user=mysql --defaults-file=/home/mysql/mariadb/var/my.cnf
```

# 시작 - [mysqld] 영역에 skip-grant-tables 추가해야됨
```
./mysqld_safe --defaults-file=/home/mysql/mariadb/var/my.cnf &
```

# mysql client - 초기 비밀번호는 공백으로 엔터 입력
```
./mysql --defaults-file=/home/mysql/mariadb/var/my.cnf -u root -p
```

# root 비밀번호 변경
```
Run mysql> flush privileges;
Set new password by ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';
Go back to /etc/my.cnf and remove/comment skip-grant-tables
Restart Mysql
```

# 종료
```
kill `cat /home/mysql/mariadb/var/mysqld.pid`
```

# 계정추가 및 권한 부여
```
# any host 에서 root 접근 하도록 추가
SELECT HOST, user, PASSWORD FROM mysql.user ;
create user 'root'@'%' identified BY '123123'; 
grant all privileges on *.* to 'root'@'%' identified BY '123123' with grant OPTION;
flush PRIVILEGES;

# 계정 추가 및 권한 부여
create user 'splash'@'%' identified BY '123123'; 
grant all privileges on *.* to 'splash'@'%' identified BY '123123' with grant option;
flush privileges;
show grants for 'splash'@'%';
```
