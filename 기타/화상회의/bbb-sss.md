# BBB-SSL

<https://cerulean85.tistory.com/287>

## 우분투 버전 확인

grep . /etc/*-release

## 종속성 제거 설치

```bash
apt-get purge bbb-apps bbb-apps-akka bbb-apps-screenshare bbb-apps-sip bbb-apps-video bbb-client bbb-config bbb-demo bbb-freeswitch-core bbb-freeswitch-sounds bbb-fsesl-akka bbb-libreoffice bbb-mkclean bbb-playback-presentation bbb-record-core bbb-red5 bbb-swftools bbb-web bigbluebutton

# apt-get purge nginx nginx-common nginx-core
# apt-get purge libtomcat7-java tomcat7 tomcat7-common

sudo vim /etc/apt/sources.list
deb http://archive.ubuntu.com/ubuntu xenial main restricted universe multiverse

sudo apt-get install haveged
sudo apt-get update
sudo apt-get dist-upgrade

wget http://ubuntu.bigbluebutton.org/repo/bigbluebutton.asc -O- | sudo apt-key add - 

echo "deb http://ubuntu.bigbluebutton.org/xenial-110/ bigbluebutton-xenial main" | sudo tee /etc/apt/sources.list.d/bigbluebutton.list

sudo apt-get update
sudo apt-get install bigbluebutton
sudo bbb-conf --restart
bbb-conf --check
sudo bbb-conf --setip live2.ggomgi.com
sudo apt-get install bbb-demo

# 2.0 업그레이드 : HTML까지 설치해야함

sudo vim /etc/apt/sources.list.d/bigbluebutton.list
deb http://ubuntu.bigbluebutton.org/xenial-200/ bigbluebutton-xenial main

sudo apt-get update
sudo apt-get dist-upgrade

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

sudo apt-get update
sudo apt-get install -y mongodb-org curl
sudo service mongod start

curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

sudo apt-get install -y bbb-html5

sudo bbb-conf --restart
sudo apt-get update
sudo apt-get dist-upgrade

sudo systemctl restart bbb-html5.service
```

## 인증서

### 인증서 생성

```bash
apt-get install letsencrypt

mkdir /etc/nginx/ssl
openssl dhparam -out /etc/nginx/ssl/dhp-2048.pem 2048
bbb-conf --setip bigbluebutton.example.com

#letsencrypt --webroot -w /var/www/bigbluebutton-default/ -d bigbluebutton.example.com certonly
letsencrypt --webroot --webroot-path=/var/www/bigbluebutton-default/ -d webrtc.oss-lab.kr certonly

ls /etc/letsencrypt/live/bigbluebutton.example.com/
```

### 인증서 적용

```bash
vim /etc/nginx/sites-available/bigbluebutton
server {
  server_name live2.ggomgi.com;
  listen 80;
  listen [::]:80;
  listen 443 ssl;
  listen [::]:443 ssl;
  ssl_certificate /etc/letsencrypt/live/live2.ggomgi.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/live2.ggomgi.com/privkey.pem;
  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout 10m;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers "ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS:!AES256";
  ssl_prefer_server_ciphers on;
  ssl_dhparam /etc/nginx/ssl/dhp-2048.pem;
}  
```

```bash
sudo crontab -e
30 2 * * 1 /usr/bin/letsencrypt renew >> /var/log/le-renew.log
35 2 * * 1 /bin/systemctl reload nginx
```

### dhp-2048.pem 생성

```bash
sudo mkdir /etc/nginx/ssl
sudo openssl dhparam -out /etc/nginx/ssl/dhp-2048.pem 2048
```

### http to https

```bash
sudo vim /opt/freeswitch/conf/sip_profiles/external.xml
<param name="wss-binding" value=":7443"/>
sudo vim /etc/bigbluebutton/nginx/sip.nginx
location /ws {
  proxy_pass https://203.0.113.1:7443;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
  proxy_read_timeout 6h;
  proxy_send_timeout 6h;
  client_body_timeout 6h;
  send_timeout 6h;
}

sudo vim /var/lib/tomcat7/webapps/bigbluebutton/WEB-INF/classes/bigbluebutton.properties
bigbluebutton.web.serverURL=https://bigbluebutton.example.com

sudo vim /usr/share/red5/webapps/screenshare/WEB-INF/screenshare.properties
jnlpUrl=https://live2.ggomgi.com/screenshare
jnlpFile=https://live2.ggomgi.com/screenshare/screenshare.jnlp

sudo sed -e 's|http://|https://|g' -i /var/www/bigbluebutton/client/conf/config.xml

sudo vim /usr/local/bigbluebutton/core/scripts/bigbluebutton.yml
playback_protocol: https

sudo vim /var/lib/tomcat7/webapps/demo/bbb_api_conf.jsp
String BigBlueButtonURL = "https://live2.ggomgi.com/bigbluebutton/";

sudo bbb-conf --restart
```

```bash
sudo vim /opt/freeswitch/conf/vars.xml
<X-PRE-PROCESS cmd="set" data="external_rtp_ip=35.200.75.77"/>
<X-PRE-PROCESS cmd="set" data="external_sip_ip=35.200.75.77"/>

sudo vim /opt/freeswitch/conf/sip_profiles/external.xml
<param name="ext-rtp-ip" value="$${external_rtp_ip}"/>
<param name="ext-sip-ip" value="$${external_sip_ip}"/>

sudo vim /etc/bigbluebutton/nginx/sip.nginx
ocation /ws {
        proxy_pass https://35.200.75.77:7443;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_read_timeout 6h;
        proxy_send_timeout 6h;
        client_body_timeout 6h;
        send_timeout 6h;
}
```

```bash
$ sudo bbb-conf --restart
```
