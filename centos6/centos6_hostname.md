# centos 6 호스트네임 변경
- hostname 영구 변경
```
#CentOS 6
[root@localhost ~]# vi /etc/sysconfig/network
HOSTNAME=myhost

#CentOS 7
[root@localhost ~]# hostnamectl set-hostname myhost
```