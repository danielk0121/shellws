## virtual-box 설정
- 각 vm NIC 설정을 'NAT' 가 아니라 'NAT 네트워크' 로 설정해야 한다
- VM 설정 > 네트워크 > 어댑터2 > 다음에 연결됨 > NAT 네트워크

## route 명령어로 default route 설정이 되어 있는지 확인해봐야 한다
```
[root@localhost ~]# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.0.2.0        *               255.255.255.0   U     0      0        0 eth0
192.168.56.0    *               255.255.255.0   U     0      0        0 eth1
link-local      *               255.255.0.0     U     1002   0        0 eth0
link-local      *               255.255.0.0     U     1003   0        0 eth1
172.17.0.0      *               255.255.0.0     U     0      0        0 docker0
default         10.0.2.2        0.0.0.0         UG    0      0        0 eth0
```

## 10.0.2.x eth0 은 NAT 사용하여 호스트 외부와 통신, 192.168.56.x eth1 은 호스트only 사용
```
[root@localhost ~]#
[root@localhost ~]# ifconfig -a
docker0   Link encap:Ethernet  HWaddr 96:61:63:82:22:52
          inet addr:172.17.42.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::14df:dbff:fe8a:8d27/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:600 (600.0 b)  TX bytes:468 (468.0 b)

eth0      Link encap:Ethernet  HWaddr 08:00:27:3F:44:07
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe3f:4407/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:20 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:1772 (1.7 KiB)  TX bytes:1862 (1.8 KiB)

eth1      Link encap:Ethernet  HWaddr 08:00:27:AF:A6:00
          inet addr:192.168.56.110  Bcast:192.168.56.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:feaf:a600/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:108 errors:0 dropped:0 overruns:0 frame:0
          TX packets:128 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:15122 (14.7 KiB)  TX bytes:38296 (37.3 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)

vethe9aa064 Link encap:Ethernet  HWaddr 96:61:63:82:22:52
          inet6 addr: fe80::9461:63ff:fe82:2252/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:7 errors:0 dropped:0 overruns:0 frame:0
          TX packets:13 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:558 (558.0 b)  TX bytes:1014 (1014.0 b)
```

## default gw 설정 !! 
> ifcfg-eh0,1 에 GATEWAY 옵션을 지우고 
> /etc/sysconfig/network 에 GATEWAY 를 넣어서 route 의 default gw 를 잡아준다
```
[root@localhost ~]#
[root@localhost ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=localhost.localdomain
GATEWAY=10.0.2.2
[root@localhost ~]#
[root@localhost ~]# cat /etc/sysconfig/network
network          network-scripts/ networking/
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
HWADDR=08:00:27:3f:44:07
TYPE=Ethernet
UUID=d00a91fa-db26-4e50-99a9-77954ce4c161
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=dhcp
[root@localhost ~]#
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth1
DEVICE=eth1
HWADDR=08:00:27:af:a6:00
TYPE=Ethernet
UUID=d00a91fa-db26-4e50-99a9-77954ce4c161
ONBOOT=yes
#NM_CONTROLLED=yes
BOOTPROTO=static
IPV6INIT=no
IPADDR=192.168.56.110
NETMASK=255.255.255.0
#GATEWAY=192.168.56.1
#BOOTPROTO=dhcp
```
