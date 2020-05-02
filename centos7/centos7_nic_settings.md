### MAC 설정
- centos 7 에서 70-persistent-net.rules 파일이 없으면 만들어준다
- ATTR{address} 부분을 정확히 써줘야됨
- NAME="enp0s3" 부분에 앞으로 사용할 device 이름을 적어줌
- reboot 해야 적용됨
```
[root@localhost ~]# cat /etc/udev/rules.d/70-persistent-net.rules 
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="08:00:27:90:EA:01", ATTR{type}=="1", KERNEL=="eth*", NAME="enp0s3"

SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="08:00:27:90:EA:02", ATTR{type}=="1", KERNEL=="eth*", NAME="enp0s8"
```

### ifcfg 파일 설정 - 1
``` 
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-enp0s3 
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"

DEFROUTE="yes"
NM_CONTROLLED=yes

IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"

NAME="enp0s3"
UUID="e2e9d627-e73c-4e73-bec5-4b53f0f6e83e"
DEVICE="enp0s3"
ONBOOT="yes"
HWADDR="08:00:27:90:EA:01"
IPV6_PRIVACY="no"

GATEWAY=10.0.3.2
IPADDR=10.0.3.108
NETWORK=10.0.3.0
NETMASK=255.255.255.0
METRIC=0
```

### ifcfg 파일 설정 - 2
``` 
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no

BOOTPROTO=static
DEFROUTE=no
NM_CONTROLLED=no

IPV4_FAILURE_FATAL=yes
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8
UUID=c51212eb-180b-420c-8d4d-929a6e60c5bd
DEVICE=enp0s8
ONBOOT=yes
HWADDR=08:00:27:90:EA:02
IPADDR=192.168.56.108
PREFIX=24
GATEWAY=192.168.56.1
NETMASK=255.255.255.0
IPV6_PRIVACY=no
```

# ifconfig 확인
```
[root@localhost ~]# ifconfig -a
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.3.108  netmask 255.255.255.0  broadcast 10.0.3.255
        inet6 fe80::a00:27ff:fe90:ea01  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:90:ea:01  txqueuelen 1000  (Ethernet)
        RX packets 50  bytes 7772 (7.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 55  bytes 4548 (4.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.108  netmask 255.255.255.0  broadcast 192.168.56.255
        inet6 fe80::a00:27ff:fe90:ea02  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:90:ea:02  txqueuelen 1000  (Ethernet)
        RX packets 529  bytes 47729 (46.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 445  bytes 45805 (44.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

# route 설정 확인
```
[root@localhost ~]# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         gateway         0.0.0.0         UG    0      0        0 enp0s3
10.0.3.0        0.0.0.0         255.255.255.0   U     0      0        0 enp0s3
link-local      0.0.0.0         255.255.0.0     U     1002   0        0 enp0s3
link-local      0.0.0.0         255.255.0.0     U     1003   0        0 enp0s8
192.168.56.0    0.0.0.0         255.255.255.0   U     0      0        0 enp0s8
```
