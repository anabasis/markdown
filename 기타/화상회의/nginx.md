# 설정

/usr/sbin/nginx
/etc/nginx/

- nginx.conf

```properties
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```

- bigbluebutton

```properties
listen   80;
listen [::]:80;
server_name  172.30.1.33;
access_log  /var/log/nginx/bigbluebutton.access.log;

# Handle desktop sharing tunneling.  Forwards
# requests to Red5 on port 5080.
location /deskshare {
    proxy_pass         http://127.0.0.1:5080;
}

# BigBlueButton landing page.
location / {
    root   /var/www/bigbluebutton-default;
    index  index.html index.htm;
}

# Include specific rules for record and playback
include /etc/bigbluebutton/nginx/*.nginx;

```
