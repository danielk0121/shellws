centos maria db install on virtual box
====

----


### 방화벽 끄기
```
service iptables stop
chkconfig --level 345 iptables off
```


### ssh dns 끄기
> putty 등으로 ssh 사용시 빠르게 하기 위해서
```
echo 'UseDNS no' >> /etc/ssh/sshd_config
service sshd restart
```


### yum repo 변경
> [제타위키원문](https://zetawiki.com/wiki/Yum_Daum_%EC%A0%80%EC%9E%A5%EC%86%8C_%EC%84%A4%EC%A0%95)
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


### 마리아 디비 설치
```
yum update -y
yum install MariaDB-server MariaDB-client -y
```


### 마리아 디비 실행
```
service mysql start
chkconfig --level 35 mysql on
service mysql status
```



