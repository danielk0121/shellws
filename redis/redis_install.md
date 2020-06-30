# centos 6 redis 설치

```
# 사전 준비
# Remi 저장소에서 redis 최신 버전을 배포하며 redis는 EPEL 저장소에 있는 jemalloc 패키지를 필요로 하므로 EPEL과 Remi 저장소를 설치해야 한다. 
rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

# redis 설치 
# epel, remi 저장소는 기본적으로  활성화되지 않으므로 
# --enablerepo 옵션을 주거나 
# /etc/yum.repos.d/{remi,epel}.repo 파일내 enabled 항목을 1로 설정해야 한다.
yum --enablerepo=epel,remi install redis

# 부팅시 자동 실행하도록 설정
chkconfig redis on

# redis 구동
service redis restart
```

# centos install redis-cli only

```
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
cp src/redis-cli /usr/local/bin/
```
