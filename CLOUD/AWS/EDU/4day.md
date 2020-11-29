# 4 DAY

MYIP = 121.142.29.68/32

BASTION - 54.180.9.164  , 172.31.14.180
WEB -  15.164.228.47  ,  172.31.40.201
WORKPRES - 3.34.152.141 , 10.0.0.57
DATABASE - 10.0.1.50

탄력적 IP - 3.34.106.144

시작템플릿 작성

사용자 데이터

```bash
#!/bin/bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo echo "WEB01" > /var/www/html/index.html
```

ns-1660.awsdns-15.co.uk
ns-1284.awsdns-32.org
ns-625.awsdns-14.net
ns-306.awsdns-38.com

![NAT Gateway](./images/4day_NATGATEWAY.png)

![NAT Gateway](./images/4day_NATGATEWAY2.png)

## NAT Gateway

프라이빗/퍼블릭 연동

가용영역이 다른 경우

- 라우팅테이블
- 피어링연결

```bash
ssh -i .ssh/20201129-4DAY.pem ubuntu@10.0.1.50

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/Users/username/.ssh/your-key.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.

chmod 600 .ssh/20201129-4DAY.pem

 ssh -i .ssh/20201129-4DAY.pem ubuntu@10.0.1.50
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.4.0-1117-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

### WordPress / MySQL

```bash
#// MariaDB 설치
sudo apt-get -y update
sudo apt-get -y install mariadb-server
sudo mysql_secure_installation
sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf
#bind-address            = 127.0.0.1

sudo systemctl restart mysql
sudo mysql -u root -p
```

```sql
CREATE USER 'wpuser'@'%' IDENTIFIED BY 'welcome!1';
CREATE DATABASE IF NOT EXISTS wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'%';
quit
```

### 워드프레스 설치

```bash
#!/bin/bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo echo "WEB01" > /var/www/html/index.html
```

```bash
yum install -y php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysql -y
wget https://ko.wordpress.org/wordpress-4.8.2-ko_KR.zip
yum install -y httpd php php mysql php-gd php-mbstring wget unzip
cd /var/www/html
unzip /root/wordpress-4.8.2-ko_KR.zip
chown -R apache:apache wordpress
```

웹브라우저 http://본인 ip/wordpress

인스턴스 삭제 > NAT 게이트웨이 삭제 > VPC 삭제


종료

EC2 -> VPC (NAT Gateway) -> VPC 

## EFS

```bash
sudo yum install -y amazon-efs-utils
```

```bash
sudo mount -t efs -o tls fs-44869c25:/ efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-44869c25.efs.ap-northeast-2.amazonaws.com:/ efs


sudo umount /home/ec2-user/efs
```

## S3

https://s3.ap-northeast-2.amazonaws.com/s3.anabasis.shop/CF_TEST/image.jpg

정적 웹사이트 호스팅

```html
<html xmlns="http://www.w3.org/1999/xhtml" >
<head>
    <title>My Website Home Page</title>
</head>
<body>
  <h1>Welcome to my website</h1>
  <p>Now hosted on Amazon S3!</p>
<p><img src="https://s3.ap-northeast-2.amazonaws.com/s3.anabasis.shop/CF_TEST/image.jpg" alt="my test image"></p>
</body>
</html>
```

https://s3.ap-northeast-2.amazonaws.com/s3.anabasis.shop/index.html

Rout53 설정

## CloudFront

CDN

![CloudFront](./images/4day_CDN.png)

Domain Name : d3ioz0j5fcdpgj.cloudfront.net