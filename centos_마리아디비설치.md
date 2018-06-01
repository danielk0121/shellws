# 방화벽 끄기
```
service iptables stop
chkconfig --level 345 iptables off
```


# ssh dns 끄기
```
echo 'UseDNS no' >> /etc/ssh/sshd_config
service sshd restart
```


# yum repo 변경
```
bzip2 /etc/yum.repos.d/CentOS-*.repo

echo '[base]
name=CentOS-$releasever - Base
baseurl=http://ftp.daumkakao.com/centos/$releasever/os/$basearch/
gpgcheck=0 
[updates]
name=CentOS-$releasever - Updates
baseurl=http://ftp.daumkakao.com/centos/$releasever/updates/$basearch/
gpgcheck=0
[extras]
name=CentOS-$releasever - Extras
baseurl=http://ftp.daumkakao.com/centos/$releasever/extras/$basearch/
gpgcheck=0' > /etc/yum.repos.d/Daum.repo
yum repolist

echo '[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.2/centos6-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1' > /etc/yum.repos.d/MariaDB.repo
```


# 마리아 디비 설치
```
yum update -y
yum install MariaDB-server MariaDB-client -y
```


# 마리아 디비 실행 및 기본 설정 실행
```
service mysql start
chkconfig --level 35 mysql on
service mysql status
mysql_secure_installation
```

# root 유저 외부에서 로그인 가능하도록 수정
```
mysql -u root -p
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' ;
flush privileges ;
```

