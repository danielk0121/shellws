## bitnami stack 으로 설치 (아래 수동 설치 방법으로 설치하니, 레드마인에서 500에러가 자주 발생함)
```
# bitnami 공식 홈페이지에서 다운받으면 redmine 최신 버전 3버전 이상으로 다운받는데, 
# centos 6은 redmine 2.5 버전으로 설치해야한다
# bitnami older version link : 
# http://downloads.bitnami.com/files/download/redmine/bitnami-redmine-2.5.1-1-linux-x64-installer.run

wget http://downloads.bitnami.com/files/download/redmine/bitnami-redmine-2.5.1-1-linux-x64-installer.run
chmod +x bitnami-redmine-2.5.1-1-linux-x64-installer.run
./bitnami-redmine-2.5.1-1-linux-x64-installer.run

# 수동 설치시 httpd, mysql 을 기본으로 사용하므로 기존 시스템에서 사용하는 것과 겹치는 이슈가 있다
# => 해결 필요 !
```


## 수동설치 주의사항
```
vm 으로하면 2코어 이상을 권장, ruby 컴파일 처리 할때 오래걸림
설치 가이드 : http://www.redmine.org/projects/redmine/wiki/install_Redmine_25x_on_Centos_65_complete
```

## 필요 패키지 설치
```
yum -y install wget zip unzip libyaml-devel zlib-devel curl-devel openssl-devel httpd-devel apr-devel apr-util-devel mysql-devel gcc ruby-devel gcc-c++ make postgresql-devel ImageMagick-devel sqlite-devel perl-LDAP mod_perl perl-Digest-SHA httpd mysql mysql-server
```

## mysql, httpd 등록
```
chkconfig httpd on
chkconfig mysqld on
service httpd start
service mysqld start
```

## mysql security 설정
```
/usr/bin/mysql_secure_installation
```

## 루비 설치
```
\curl -L https://get.rvm.io | bash
source /etc/profile.d/rvm.sh
rvm list known
# rvm install 1.9.3
rvm install 2.2.2
ruby -v
yum -y install rubygems
```

## 루비 모듈 설치
```
gem install passenger
passenger-install-apache2-module
```

## passenger install 이후 아파치 모듈 설정 복사
```
echo 'LoadModule passenger_module /usr/local/rvm/gems/ruby-2.2.2/gems/passenger-5.3.4/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
	PassengerRoot /usr/local/rvm/gems/ruby-2.2.2/gems/passenger-5.3.4
	PassengerDefaultRuby /usr/local/rvm/gems/ruby-2.2.2/wrappers/ruby
</IfModule>' > /etc/httpd/conf.d/passenger.conf
```

## 아파치 재시작
```
service httpd restart
```

## 레드마인을 위한 디비 생성
```
mysql -u root -p

create database redmine_db character set utf8;
create user 'redmine_admin'@'localhost' identified by 'root.123';
grant all privileges on redmine_db.* to 'redmine_admin'@'localhost';
quit;
```

## 레드마인 다운로드 및 사용 디비 설정
```
cd /var/www
wget http://www.redmine.org/releases/redmine-2.5.0.tar.gz
tar xvfz redmine-2.5.0.tar.gz
mv redmine-2.5.0 redmine
rm -rf redmine-2.5.0.tar.gz

# 레드마인에서 사용할 mysql db 패스워드 설정
cp /var/www/redmine/config/database.yml.example /var/www/redmine/config/database.yml
vi /var/www/redmine/config/database.yml

production:
  adapter: mysql2
  database: <db_name>
  host: localhost
  username: root
  password: "password"
  encoding: utf8

```

## 루비로 레드마인 컴파일
```
cd /var/www/redmine
gem install bundler
bundle install
```

## 루비로 레드마인 최종 설정
```
rake generate_secret_token
RAILS_ENV=production rake db:migrate
RAILS_ENV=production rake redmine:load_default_data
```

## 아파치 설정
```
# 192.168.56.101 대신에 운영하는 도메인을 사용
echo '
<VirtualHost *:80>
		ServerName		192.168.56.101
		ServerAdmin		192.168.56.101@example.com
		DocumentRoot	/var/www/redmine/public/
		ErrorLog 		logs/redmine_error_log
		<Directory "/var/www/redmine/public/">
			Options Indexes ExecCGI FollowSymLinks
			Order allow,deny
			Allow from all
			AllowOverride all
		</Directory>
</VirtualHost>' >  /etc/httpd/conf.d/redmine.conf

# 아파치 재시작
chown -R root:root /var/www/redmine
chmod -R 755 root
service httpd restart
```

## 브라우저로 접속
```
http://192.168.56.101/
최초 로그인 계정 : admin / admin
```

## SCM 연동 필요 !!


