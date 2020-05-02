## 계정 생성, 폴더 변경, 그룹 지정, 비밀번호 설정
- centos 6 기준

```
# root 계정으로 실행
groupadd develop
useradd rpapp -G develop
mkdir -p /home/users/rpapp
usermod -d /home/users/rpapp/ rpapp
cp /root/.bash_profile /home/users/rpapp/
cp /root/.bashrc /home/users/rpapp/
passwd rpapp
```
