DHCP 기능을 사용하려면 DHCP서버가 필요하다. DHCP 서버는 할당할 수 있는 IP 주소 범위를 관리하며 DHCP 요청이 들어오면 IP 주소를 할당하고, 네트워크를 사용하기 위하여 필요한 네트워크 정보를 제공하는 역할을 수행한다. 가장 흔히 볼 수 있는 DHCP 서버는 가정용 인터넷 공유기이다. 인터넷 공유기는 DHCP 기능을 수행하여 공유기에 접속하는 클라이언트에게 IP 주소를 할당하고 NAT를 사용하여 사설 네트워크 영역의 클라이언트가 공인 IP 주소를 사용하여 외부와 통신을 가능하게 하는 기능을 지원한다. 

## DHCP 동작 방식
DHCP 프로토콜을 통해 DHCP 서버가 호스트에게 전달해 주는 주요 정보는 다음과 같다.
- 할당할 IP 주소
- 서브넷 마스크
- 디폴트 라우팅 경로(게이트 웨이)
- DNS 서버 주소
- 기본 도메인 이름
- IP 주소 임대 기간
- WINS서버 등 기타 네트워크 정보
DHCP 서버가 호스트에게 이러한 정보를 전달하기 위한 과정은 4단계로 수행된다.

| 단계        | 설명                                                                                                                                                                                   |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Discovery   | 새로 연결된 호스트가 DHCP 서버에게 사용할 수 있는 IP 주소와 네트워크 정보를 전달해 줄 것을 요청한다. 현재 IP 주소를 할당받지 못한 상태이므로 DHCP 요청을 브로드 캐스트로 요청한다.     |
| Offer       | DHCP 서버가 DCHP Discovery 요청에 응답하여 IP 할당을 제안하는 단계이다. 이 단계에서 IP 주소 및 네트워크에 대한 모든 정보가 호스트에게 전달된다.                                        |
| Requset     | 복수의 DHCP 서버에게 DHCP Offer를 수신할 수 있으므로, 어떤 DHCP 서버로부터 전달받은 IP 주소를 사용할 지 알린다. 네트워크 내 모든 DHCP 서버가 수신할 수 있도록 브로드캐스트로 전송한다. |
| Acknowledge | 이 단계에서는 DHCP 서버가 IP 주소를 임대했다는 정보를 저장하고, 호스트는 전달받은 IP 주소와 네트워크 정보를 사용한다.                                                                  | 

DHCP 서버는 호스트에게 IP 주소를 임대할 때 임대 기간을 설정할 수 있다.  호스트는 DHCP를 통해 IP 주소를 임대 받은 후에 임대 기간이 만료될 때 까지 IP 주소를 사용할  수 있다. IP 주소를 지속적으로 사용하려면 만료되기 전에 임대 기간을 연장하기 위한 절차를 진행한다. 
임대 연장에 사용되는 단계는 DHCP Requset 와 DHCP Acknowledge 단계이다. 최초 임대 과정과의 차이는 브로드 캐스트가 아닌 유니캐스드로 진행한다는 점이다. 

# DHCP 서버 구성
## dhcp 패키지 설치

```
[root@server1 ~]# yum install -y dhcp
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile

...

Installed:
  dhcp.x86_64 12:4.2.5-83.el7.centos.1                                                      

Complete!
```

기존 리눅스에서 dhcp와 충돌되는 dnsmasq를 종료한다.

```
[root@server1 ~]# ps -ef | grep dnsmasq
nobody    1825     1  0 15:20 ?        00:00:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
root      1827  1825  0 15:20 ?        00:00:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
root     20157  3815  0 16:19 pts/2    00:00:00 grep --color=auto dnsmasq
[root@server1 ~]# kill -9 1825
[root@server1 ~]# ps -ef | grep dnsmasq
root     20168  3815  0 16:19 pts/2    00:00:00 grep --color=auto dnsmasq
[root@server1 ~]# systemctl disable dnsmasq
[root@server1 ~]# systemctl stop dnsmasq
```


## 고정 IP 주소 설정
DHCP 서버는 자기 자신에게 IP 주소를 임대하거나 다른 서버로부터 IP 주소를 임대받을 수 없다. 따라서 dhcp 패키지를 설치한 후에 IP 주소를 수동으로 설정해야한다.
```
[root@server1 ~]# nmcli con add con-name "DHCP static" ifname enp0s3 type ethernet ipv4.addresses 10.0.2.40/24 ipv4.dns 8.8.8.8 ipv4.gateway 10.0.2.1 ipv4.method manual
Connection 'DHCP static' (a5a183ae-c83b-4110-8e5c-2cdf542e1e07) successfully added.
[root@server1 ~]# nmcli con up "DHCP static"
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/5)
[root@server1 ~]# curl naver.com
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center> NWS </center>
</body>
</html>
```
VirtualBox의 NAT network 에서 DHCP를 비활성화한다. 

## dhcpd.conf 설정 파일 수정
```
ddns-update-style	interim;   //혹은 none  //네임 서버의 동적 업데이트 옵션
subnet	10.0.2.0	netmask	255.255.255.0	{  //dhcp서버가 존재하는 네트워크 대역대 주소
	option routers 10.0.2.1;   //클라이언트에게 알려줄 게이트웨이
	option subnet-mask	255.255.255.0;  // 클라이언트에게 알려줄 넷마스크(네트워크 범위)
	range	dynamic-bootp	10.0.2.80	10.0.2.100;   클라이언트에게 할당할 IP 주소 범위
	option	domain-name-servers	8.8.8.8;  클라이언트에게 알려줄 dns서버 주소
	default-lease-time	10000;  클라이언트에게 ip를 임대할때 기본적인 시간(단위 초)
	max-lease-time	50000; 클라이언트가 ip를 임대한후 보유할 수 있는 최대 시간(특정 컴퓨터가 ip 독점 방지)
}
```

각 서브넷 설정과 별도로 특정 호스트에게 IP 주소를 지정하여 할당할 수 있도록 IP 주소를 예약할 수 있다. 이 때 MAC 주소를 사용하여 호스트를 구별한다. 
```
host
```

## 서비스 활성화 및 방화벽 구성
```
[root@server1 ~]# systemctl restart dhcpd
[root@server1 ~]# systemctl enable dhcpd
Created symlink from /etc/systemd/system/multi-user.target.wants/dhcpd.service to /usr/lib/systemd/system/dhcpd.service.
[root@server1 ~]# firewall-cmd --add-service=dhcp --permanent
success
[root@server1 ~]# firewall-cmd --reload
success
```

이후 2번 리눅스 재부팅

## 동작 및 상태 확인
서비스가 정상적으로 시작되면 DHCP 서버에서 임대한 IP 주소의 정보는 `/var/lib/dhcp/dhcp.leases` 파일에 저장된다.