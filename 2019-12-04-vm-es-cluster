centos7 기준, ElasticSearch 7.4 기준
엘라스틱서치를 실행하기 위해 필요한 os 설정

# 임시 적용
sysctl -w vm.max_map_count=262144

vi /etc/sysctl.conf
```
# for permanent
fs.file-max = 512000
vm.max_map_count=262144
```

vi /etc/security/limits.conf
```
...중략...
# for any account
*                soft    memlock         unlimited
*                hard    memlock         unlimited
*                soft    nproc           65535
*                hard    nproc           65535
*                soft    nofile          65535
*                hard    nofile          65535

# End of file
```
