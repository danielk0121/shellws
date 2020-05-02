### 디비 초기화
```
cd /home/users/dkkwon/app/mariadb/rpcfgdev/mariadb-10.2.10-linux-x86_64/bin
./scripts/mysql_install_db --user=dkkwon --defaults-file=/home/users/dkkwon/app/mariadb/rpcfgdev/var/my.cnf 
```

### root 비번 설정
```
/home/users/dkkwon/app/mariadb/rpcfgdev/mariadb-10.2.10-linux-x86_64//bin/mysqladmin --defaults-file=/home/users/dkkwon/app/mariadb/rpcfgdev/var/my.cnf -u root password 'root.123'
```

### mysql client 비번 없이 접속
```
/home/users/dkkwon/app/mariadb/rpcfgdev/mariadb-10.2.10-linux-x86_64/bin/mysqld_safe --defaults-file=/home/users/dkkwon/app/mariadb/rpcfgdev/var/my.cnf --skip-grant-tables &
```
