# 인스톨

hostnamectl set-hostname rabbitmq1.localdomain

1. EPEL 저장소 설치

    ```bash
    yum install -y epel-release
    ```

2. erlang 설치

    ```bash
    yum install -y erlang
    ```

3. RabbitMQ 설치

    ```bash
    yum install -y rabbitmq-server
    sudo yum -y update
    curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
    sudo yum makecache -y --disablerepo='*' --enablerepo='rabbitmq_rabbitmq-server'
    sudo yum -y install rabbitmq-server
    ```

4. Web Console 활성화

    ```bash
    rabbitmq-plugins enable rabbitmq_management
    ```

5. RabbitMQ 서비스 등록

    ```bash
    systemctl list-unit-files | grep rabbitmq-server
    rabbitmq-server.service disabled

    systemctl enable rabbitmq-server
    ```

6. RabbitMQ 서비스 기동

    ```bash
    systemctl start rabbitmq-server
    ```

7. RabbitMQ의 새로운 계정 추가

    ```bash
    ## RabbitMQ 사용자 리스트 확인
    rabbitmqctl list_users

    ## RabbitMQ 사용자 추가(<사용자> <비번>)
    rabbitmqctl add_user test test
    rabbitmqctl add_user admin admin

    ##RabbitMQ 사용자에게 태그설정(<사용자> <태그>)
    rabbitmqctl set_user_tags test administrator
    rabbitmqctl set_user_tags test administrator
    rabbitmqctl set_user_tags admin administrator

    ## RabbitMQ 사용자 접속 퍼미션 설정(<사용자> <접속퍼미션> ...)
    rabbitmqctl list_permissions

    # rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
    rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"

    ## 사용자 제거
    rabbitmqctl delete_user <사용자>

    ## 사용자의 비번 변경
    rabbitmqctl change_password <사용자> <신규비번>

    ## 방화벽
    ## 6.5 이전
    vi /etc/sysconfig/iptables
    -A INPUT -m state —state NEW -m tcp -p tcp —dport 5672 -j ACCEPT
    -A INPUT -m state —state NEW -m tcp -p tcp —dport 4369 -j ACCEPT
    -A INPUT -m state —state NEW -m tcp -p tcp —dport 35197 -j ACCEPT
    -A INPUT -m state —state NEW -m tcp -p tcp —dport 15672 -j ACCEPT
    ## 6.5 이후
    firewall-cmd --permanent --zone=public --add-port=5672/tcp
    firewall-cmd --permanent --zone=public --add-port=4369/tcp
    firewall-cmd --permanent --zone=public --add-port=35197/tcp
    firewall-cmd --permanent --zone=public --add-port=15672/tcp
    firewall-cmd --reload
    ```

8. Web Console 접속 https://serverip:15672

[원본 URL]
http://bangcfactory.tistory.com/entry/rabbitmq-%EC%82%AC%EC%9A%A9%EC%9E%90-%EA%B4%80%EB%A6%AC
http://linux.systemv.pe.kr/centos-7-rabbitmq-%EC%84%A4%EC%B9%98/

[참고] ‘메시지 큐’ 란 무엇인가? http://zzong.net/post/3

출처: https://sangchul.kr/647 [변군이글루]