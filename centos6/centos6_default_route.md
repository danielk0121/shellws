# NIC 구성 및 default route 설정

```
[root@test01 ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
HWADDR=08:00:27:46:23:0E
TYPE=Ethernet
UUID=d00a91fa-db26-4e50-99a9-77954ce4c161
ONBOOT=yes
BOOTPROTO=static
IPV6INIT=no
NM_CONTROLLED=no
IPADDR=10.0.3.101
NETWORK=10.0.3.0
NETMASK=255.255.255.0
GATEWAY=10.0.3.2
METRIC=0
DNS1=8.8.8.8
```

```
[root@test01 ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth1
DEVICE=eth1
HWADDR=08:00:27:43:14:3A
TYPE=Ethernet
UUID=d00a91fa-db26-4e50-99a9-77954ce4c161
ONBOOT=yes
BOOTPROTO=static
IPV6INIT=no
NM_CONTROLLED=no
IPADDR=192.168.56.101
NETWORK=192.168.56.0
NETMASK=255.255.255.0
GATEWAY=192.168.56.1
METRIC=10
```

``` 
[root@test01 ~]# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.0.3.0        *               255.255.255.0   U     0      0        0 eth0
192.168.56.0    *               255.255.255.0   U     0      0        0 eth1
link-local      *               255.255.0.0     U     1002   0        0 eth0
link-local      *               255.255.0.0     U     1003   0        0 eth1
172.17.0.0      *               255.255.0.0     U     0      0        0 docker0
default         10.0.3.2        0.0.0.0         UG    0      0        0 eth0
default         192.168.56.1    0.0.0.0         UG    10     0        0 eth1
```

```
[root@test01 ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=test01
```

```
[root@test01 ~]# ping google.com
PING google.com (216.58.197.206) 56(84) bytes of data.
64 bytes from nrt13s48-in-f206.1e100.net (216.58.197.206): icmp_seq=1 ttl=53 time=32.8 ms
64 bytes from nrt13s48-in-f206.1e100.net (216.58.197.206): icmp_seq=2 ttl=53 time=32.6 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1651ms
rtt min/avg/max/mdev = 32.673/32.758/32.843/0.085 ms
```
