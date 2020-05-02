

# eth0, eth1 등의 nic mac 주소 수정
```
vi /etc/udev/rules.d/70-persistent-net.rules
```

# 수정한 mac 주소 적용
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

# 재시작
```
reboot
```
