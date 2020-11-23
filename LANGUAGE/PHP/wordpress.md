# WORDPRESS

## 설치

## 업그레이드


1. 시스템 업데이트
[root@localhost ~]# sudo yum update


2. PHP Apache MariaDB 버전 확인
[root@localhost ~]# php -v
PHP 5.4.16 (cli) (built: Nov 15 2017 16:33:54) 
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies

[root@localhost ~]# httpd -v
Server version: Apache/2.4.6 (CentOS)
Server built:   Apr 12 2017 21:03:28

[root@localhost ~]# mysql --version
mysql  Ver 15.1 Distrib 10.1.25-MariaDB, for Linux (x86_64) using readline 5.1

[root@localhost ~]# php -i | grep 'Client API'
PHP Warning:  Unknown: It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected the timezone 'UTC' for now, but please set date.timezone to select your timezone. in Unknown on line 0
Client API version => 10.1.25-MariaDB
Client API library version => 10.1.25-MariaDB
Client API header version => 5.5.50-MariaDB
Client API version => 10.1.25-MariaDB

3. 기존 PHP 삭제
[root@localhost ~]# yum remove php-*
[root@localhost ~]# yum remove php-common mod_php php-cli

4. YUM 설치를 위한 저장소 추가
CentOS 7에서 기본적으로 제공하는 PHP버전은 5.X 버전입니다.
최신버전인 PHP7을 설치하기 위해서 Webtatic EL 저장소를 추가합니다.

[ CentOS/RHEL 7.x ]
[root@localhost ~]# rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
[root@localhost ~]# rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

[ CentOS/RHEL 6.x ]
[root@localhost ~]# rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
[root@localhost ~]# rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

[root@localhost ~]# yum --enablerepo=remi update remi-release


5-1. PHP 7.0 설치
먼저 PHP7 본 패키지를 설치합니다.
[root@localhost ~]# yum install php70w

기타 필요한 모듈을 설치합니다. yum search php70w  명령으로 설치할 수 있는 모듈을 찾아볼 수 있습니다.
[root@localhost ~]# yum install php70w-cli php70w-common php70w-dba php70w-devel php70w-fpm php70w-gd php70w-imap
[root@localhost ~]# yum install php70w-ldap php70w-mbstring php70w-mcrypt php70w-mysqlnd php70w-odbc php70w-opcache
[root@localhost ~]# yum install php70w-pdo php70w-pdo_dblib php70w-pear php70w-pecl-imagick php70w-pecl-imagick-devel
[root@localhost ~]# yum install php70w-pgsql php70w-phpdbg php70w-process php70w-snmp php70w-soap php70w-tidy php70w-xml php70w-xmlrpc

[root@localhost ~]# php -v
PHP 7.0.21 (cli) (built: Jul  6 2017 11:19:16) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.0.21, Copyright (c) 1999-2017, by Zend Technologies


5-2. PHP 7.1 설치
[root@localhost ~]# yum --enablerepo=ius install mod_php71u php71u-cli php71u-devel php71u-json php71u-xml php71u-process php71u-mbstring php71u-mcrypt php71u-pdo php71u-mysqlnd php71u-opcache

[root@localhost ~]# php -v
PHP 7.1.12 (cli) (built: Nov 27 2017 11:01:12) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.1.12, Copyright (c) 1999-2017, by Zend Technologies


6. Apache 재실행 및 상태확인
[root@localhost ~]# systemctl restart httpd
[root@localhost ~]# systemctl status httpd


7. PHP 버전 확인
[root@localhost ~]# vi /var/www/html/info.php
<?php
phpinfo();
?>

브라우저에서 확인
http://IP_Address/info.php


8. php.ini 수정
[root@localhost ~]# vi /etc/php.ini
https://happyjung.com/lecture/2480


9. mysql api 버전 제대로 맞는지 검사
php -i | grep 'Client API'
Client API library version => mysqlnd 5.0.12-dev - 20150407 - $Id: b5c5906d452ec590732a93b051f3827e02749b83 $
Client API version => mysqlnd 5.0.12-dev - 20150407 - $Id: b5c5906d452ec590732a93b051f3827e02749b83 $


10. 이상 없으니 둘다 시작
[root@localhost ~]# systemctl restart httpd
[root@localhost ~]# systemctl restart mysql