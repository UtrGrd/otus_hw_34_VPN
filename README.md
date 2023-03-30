# OTUS ДЗ №34.  Мосты, туннели и VPN. #
-----------------------------------------------------------------------
## Домашнее задание ##

1) Между двумя виртуалками поднять vpn в режимах
    - tun;
    - tap;
    - Прочуствовать разницу.
2) Поднять RAS на базе OpenVPN с клиентскими сертификатами, подключиться с локальной машины на виртуалку.
3) *Самостоятельно изучить, поднять ocserv и подключиться с хоста к виртуалке
    Формат сдачи ДЗ - vagrant + ansible


-----------------------------------------------------------------------
## Результат ##

### 1. Отличия между TUN и TAP режимами VPN ###

Для запуска стенда можно использовать использовать команду ```vagrant up``` из директории /tun-tap/
Исследуем разницу в скорости при использовании туннеля в разных режимах. 

#### Тестирование скорости в режиме TUN. ####
На сервере активируем ```iperf3``` в качестве сервера, а на клиенте в роли клиента, указывая IP-адрес сервера, продолжительность теста в секундах ```-t``` и интервал вывода информации на экран ```-i```.

##### server #####
```
[root@server ~]# ip a | grep tun0
5: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100
    inet 10.10.10.1/24 brd 10.10.10.255 scope global tun0
[root@server ~]# iperf3 -s &
[1] 4800
[root@server ~]# -----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 10.10.10.2, port 43290
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 43292
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  11.4 MBytes  96.0 Mbits/sec                  
[  5]   1.00-2.00   sec  13.3 MBytes   112 Mbits/sec                  
[  5]   2.00-3.00   sec  12.0 MBytes   100 Mbits/sec                  
[  5]   3.00-4.00   sec  13.2 MBytes   110 Mbits/sec                  
[  5]   4.00-5.00   sec  13.1 MBytes   110 Mbits/sec                  
[  5]   5.00-6.00   sec  14.6 MBytes   122 Mbits/sec                  
[  5]   6.00-7.00   sec  11.9 MBytes   100 Mbits/sec                  
[  5]   7.00-8.00   sec  10.4 MBytes  86.8 Mbits/sec                  
[  5]   8.00-9.00   sec  11.2 MBytes  93.6 Mbits/sec                  
[  5]   9.00-10.00  sec  12.3 MBytes   103 Mbits/sec                  
[  5]  10.00-11.00  sec  11.8 MBytes  99.3 Mbits/sec                  
[  5]  11.00-12.00  sec  12.3 MBytes   103 Mbits/sec                  
[  5]  12.00-13.00  sec  11.0 MBytes  92.0 Mbits/sec                  
[  5]  13.00-14.00  sec  10.7 MBytes  89.5 Mbits/sec                  
[  5]  14.00-15.00  sec  10.7 MBytes  89.7 Mbits/sec                  
[  5]  15.00-16.00  sec  11.4 MBytes  95.4 Mbits/sec                  
[  5]  16.00-17.00  sec  12.2 MBytes   102 Mbits/sec                  
[  5]  17.00-18.00  sec  10.5 MBytes  87.9 Mbits/sec                  
[  5]  18.00-19.00  sec  10.2 MBytes  85.7 Mbits/sec                  
[  5]  19.00-20.00  sec  10.9 MBytes  91.5 Mbits/sec                  
[  5]  20.00-21.00  sec  11.2 MBytes  94.2 Mbits/sec                  
[  5]  21.00-22.00  sec  10.6 MBytes  88.5 Mbits/sec                  
[  5]  22.00-23.00  sec  10.9 MBytes  91.4 Mbits/sec                  
[  5]  23.00-24.00  sec  11.2 MBytes  94.3 Mbits/sec                  
[  5]  24.00-25.00  sec  10.2 MBytes  85.7 Mbits/sec                  
[  5]  25.00-26.00  sec  9.02 MBytes  75.7 Mbits/sec                  
[  5]  26.00-27.00  sec  9.66 MBytes  81.0 Mbits/sec                  
[  5]  27.00-28.00  sec  10.8 MBytes  90.6 Mbits/sec                  
[  5]  28.00-29.00  sec  11.0 MBytes  92.1 Mbits/sec                  
[  5]  29.00-30.00  sec  11.8 MBytes  98.9 Mbits/sec                  
[  5]  30.00-31.00  sec  10.8 MBytes  90.6 Mbits/sec                  
[  5]  31.00-32.00  sec  10.2 MBytes  85.7 Mbits/sec                  
[  5]  32.00-33.00  sec  10.5 MBytes  88.4 Mbits/sec                  
[  5]  33.00-34.00  sec  8.33 MBytes  69.9 Mbits/sec                  
[  5]  34.00-35.00  sec  5.70 MBytes  47.8 Mbits/sec                  
[  5]  35.00-36.00  sec  10.1 MBytes  84.3 Mbits/sec                  
[  5]  36.00-37.00  sec  10.1 MBytes  84.8 Mbits/sec                  
[  5]  37.00-38.00  sec  10.4 MBytes  87.5 Mbits/sec                  
[  5]  38.00-39.00  sec  7.29 MBytes  61.1 Mbits/sec                  
[  5]  39.00-40.00  sec  9.22 MBytes  77.3 Mbits/sec                  
[  5]  40.00-40.04  sec   301 KBytes  60.6 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.04  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.04  sec   434 MBytes  91.0 Mbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
```
##### client #####
```
[root@client ~]# iperf3 -c 10.10.10.1 -t 40 -i 5
Connecting to host 10.10.10.1, port 5201
[  4] local 10.10.10.2 port 43292 connected to 10.10.10.1 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-5.00   sec  64.1 MBytes   107 Mbits/sec   40    159 KBytes       
[  4]   5.00-10.00  sec  60.4 MBytes   101 Mbits/sec   50    185 KBytes       
[  4]  10.00-15.00  sec  56.2 MBytes  94.3 Mbits/sec   52    135 KBytes       
[  4]  15.00-20.00  sec  55.2 MBytes  92.5 Mbits/sec   17    131 KBytes       
[  4]  20.00-25.00  sec  54.1 MBytes  90.9 Mbits/sec   37    115 KBytes       
[  4]  25.00-30.00  sec  52.2 MBytes  87.6 Mbits/sec   13    132 KBytes       
[  4]  30.00-35.00  sec  45.3 MBytes  75.9 Mbits/sec   40    122 KBytes       
[  4]  35.00-40.00  sec  47.2 MBytes  79.1 Mbits/sec   43    108 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-40.00  sec   435 MBytes  91.2 Mbits/sec  292             sender
[  4]   0.00-40.00  sec   434 MBytes  91.1 Mbits/sec                  receiver

iperf Done.
```

#### Тестирование скорости в режиме TAP. ####

Проводим тестирование TAP. В этом случае, необходимо в файле ```main.yml``` изменить значение переменной ```vpn_mode``` с ```tun``` на ```tap``` и повторно запустить playbook. Затем провести сравнительные измерения скорости.

```
[root@server ~]# ip a | grep tap0
5: tap0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100
    inet 10.10.10.1/24 brd 10.10.10.255 scope global tap0

[root@server ~]# iperf3 -s &
[1] 4847
[root@server ~]# -----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 10.10.10.2, port 44528
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 44530
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  11.4 MBytes  95.8 Mbits/sec                  
[  5]   1.00-2.00   sec  12.6 MBytes   106 Mbits/sec                  
[  5]   2.00-3.00   sec  13.7 MBytes   115 Mbits/sec                  
[  5]   3.00-4.00   sec  14.5 MBytes   122 Mbits/sec                  
[  5]   4.00-5.00   sec  14.3 MBytes   120 Mbits/sec                  
[  5]   5.00-6.00   sec  12.4 MBytes   104 Mbits/sec                  
[  5]   6.00-7.00   sec  12.6 MBytes   105 Mbits/sec                  
[  5]   7.00-8.00   sec  11.6 MBytes  97.5 Mbits/sec                  
[  5]   8.00-9.00   sec  11.9 MBytes  99.5 Mbits/sec                  
[  5]   9.00-10.00  sec  11.6 MBytes  97.2 Mbits/sec                  
[  5]  10.00-11.00  sec  10.7 MBytes  89.4 Mbits/sec                  
[  5]  11.00-12.00  sec  11.7 MBytes  97.8 Mbits/sec                  
[  5]  12.00-13.00  sec  12.7 MBytes   106 Mbits/sec                  
[  5]  13.00-14.00  sec  11.3 MBytes  94.8 Mbits/sec                  
[  5]  14.00-15.00  sec  10.6 MBytes  89.1 Mbits/sec                  
[  5]  15.00-16.00  sec  9.09 MBytes  76.3 Mbits/sec                  
[  5]  16.00-17.00  sec  10.0 MBytes  84.1 Mbits/sec                  
[  5]  17.00-18.00  sec  10.3 MBytes  86.6 Mbits/sec                  
[  5]  18.00-19.00  sec  9.86 MBytes  82.7 Mbits/sec                  
[  5]  19.00-20.00  sec  9.77 MBytes  82.0 Mbits/sec                  
[  5]  20.00-21.00  sec  8.65 MBytes  72.6 Mbits/sec                  
[  5]  21.00-22.00  sec  8.02 MBytes  67.3 Mbits/sec                  
[  5]  22.00-23.00  sec  6.07 MBytes  50.9 Mbits/sec                  
[  5]  23.00-24.00  sec  9.90 MBytes  83.1 Mbits/sec                  
[  5]  24.00-25.00  sec  8.76 MBytes  73.5 Mbits/sec                  
[  5]  25.00-26.00  sec  11.1 MBytes  92.9 Mbits/sec                  
[  5]  26.00-27.00  sec  11.0 MBytes  92.1 Mbits/sec                  
[  5]  27.00-28.00  sec  8.64 MBytes  72.5 Mbits/sec                  
[  5]  28.00-29.00  sec  8.80 MBytes  73.8 Mbits/sec                  
[  5]  29.00-30.00  sec  8.52 MBytes  71.5 Mbits/sec                  
[  5]  30.00-31.00  sec  7.82 MBytes  65.6 Mbits/sec                  
[  5]  31.00-32.00  sec  9.02 MBytes  75.7 Mbits/sec                  
[  5]  32.00-33.00  sec  8.86 MBytes  74.3 Mbits/sec                  
[  5]  33.00-34.00  sec  9.13 MBytes  76.6 Mbits/sec                  
[  5]  34.00-35.00  sec  8.43 MBytes  70.7 Mbits/sec                  
[  5]  35.00-36.00  sec  9.89 MBytes  83.0 Mbits/sec                  
[  5]  36.00-37.00  sec  8.78 MBytes  73.6 Mbits/sec                  
[  5]  37.00-38.00  sec  11.2 MBytes  93.9 Mbits/sec                  
[  5]  38.00-39.00  sec  10.2 MBytes  85.5 Mbits/sec                  
[  5]  39.00-40.00  sec  9.29 MBytes  77.9 Mbits/sec                  
[  5]  40.00-40.05  sec   391 KBytes  69.4 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.05  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.05  sec   415 MBytes  86.9 Mbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------


[root@client ~]# iperf3 -c 10.10.10.1 -t 40 -i 5
Connecting to host 10.10.10.1, port 5201
[  4] local 10.10.10.2 port 44530 connected to 10.10.10.1 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-5.00   sec  68.0 MBytes   114 Mbits/sec  107    132 KBytes       
[  4]   5.00-10.00  sec  59.9 MBytes   101 Mbits/sec   32    151 KBytes       
[  4]  10.00-15.01  sec  57.0 MBytes  95.6 Mbits/sec   49    129 KBytes       
[  4]  15.01-20.01  sec  48.8 MBytes  81.8 Mbits/sec   33    144 KBytes       
[  4]  20.01-25.00  sec  41.6 MBytes  69.8 Mbits/sec   25    147 KBytes       
[  4]  25.00-30.00  sec  47.7 MBytes  80.0 Mbits/sec   37    126 KBytes       
[  4]  30.00-35.00  sec  43.3 MBytes  72.8 Mbits/sec   28    150 KBytes       
[  4]  35.00-40.01  sec  49.2 MBytes  82.4 Mbits/sec   43    148 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-40.01  sec   416 MBytes  87.1 Mbits/sec  354             sender
[  4]   0.00-40.01  sec   415 MBytes  87.0 Mbits/sec                  receiver

iperf Done.
```

Подводя итоги замеров мы видим - в режиме TAP что за 40 секунд прокачалось 415 MBytes  со средней скоростью 87 Mbits/sec , в режиме TUN  прокачалось 434 MBytes со средней скоростью 91 Mbits/sec. Исходя из этих замеров можно сделать вывод, что разница в производительности туннеля в данных условиях минимальна, но TUN  немного производительней.

#### Прочуствовать разницу ####
Отличие между TUN и TAP режимами заключается в функционировании на разных уровнях OSI. TAP эмулирует устройство Ethernet и действует на канальном уровне OSI, обрабатывая Ethernet-кадры. В то время как TUN (сетевой туннель) работает на сетевом уровне OSI, манипулируя IP-пакетами. TAP применяется для создания сетевых мостов, а TUN – для маршрутизации. Другими словами, если требуется объединить сегмент L2 с одной адресацией (vlan - широковещательный домен), это - TAP. Если же необходимо соединить две сети с разной адресацией, это - TUN.

### 2. RAS на базе OpenVPN ###

Для запуска стенда можно использовать использовать команду ```vagrant up``` из директории ```/ras/```.
В этой части задания мы будем проверять работу RAS на базе OpenVPN. Для задания будем использовать те же два виртуальных сервера: server и client, только добавим к ним ещё по одному интерфейсу с локальными сетями: 172.16.10.0/24 и 172.16.20.0/24. Это позволит эмулировать наличие других сетей за пределами server и client, и настроить маршрутизацию между ними.
Для того, чтобы маршрутизация работала, необходимо в конфигурационном файле сервера добавить следующие строки:
```
route 172.16.20.0 255.255.255.0
push "route 172.16.10.0 255.255.255.0"
```
А также создаём файл `/etc/openvpn/client/client` со следующим содержимым.
```
iroute 172.16.20.0 255.255.255.0
```
После запуска стенда проверяем маршрутизацию.
- На server:
```
[root@server ~]# ip route
default via 10.0.2.2 dev eth0 proto dhcp metric 100 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
10.10.10.0/24 via 10.10.10.2 dev tun0 
10.10.10.2 dev tun0 proto kernel scope link src 10.10.10.1 
172.16.10.0/24 dev eth3 proto kernel scope link src 172.16.10.1 metric 103 
172.16.20.0/24 via 10.10.10.2 dev tun0 
192.168.10.0/24 dev eth1 proto kernel scope link src 192.168.10.10 metric 101 
192.168.56.0/24 dev eth2 proto kernel scope link src 192.168.56.10 metric 102 

[root@server ~]# ping -c 4 172.16.20.1
PING 172.16.20.1 (172.16.20.1) 56(84) bytes of data.
64 bytes from 172.16.20.1: icmp_seq=1 ttl=64 time=1.08 ms
64 bytes from 172.16.20.1: icmp_seq=2 ttl=64 time=0.880 ms
64 bytes from 172.16.20.1: icmp_seq=3 ttl=64 time=1.07 ms
64 bytes from 172.16.20.1: icmp_seq=4 ttl=64 time=0.994 ms

--- 172.16.20.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3324ms
rtt min/avg/max/mdev = 0.880/1.009/1.083/0.082 ms

```
- На client:
```
[root@client ~]# ip route
default via 10.0.2.2 dev eth0 proto dhcp metric 102 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 102 
10.10.10.0/24 via 10.10.10.5 dev tun0 
10.10.10.5 dev tun0 proto kernel scope link src 10.10.10.6 
172.16.10.0/24 via 10.10.10.5 dev tun0 
172.16.20.0/24 dev eth3 proto kernel scope link src 172.16.20.1 metric 103 
192.168.10.0/24 dev eth1 proto kernel scope link src 192.168.10.20 metric 100 
192.168.56.0/24 dev eth2 proto kernel scope link src 192.168.56.20 metric 101 

[root@client ~]# ping -c 4 172.16.10.1
PING 172.16.10.1 (172.16.10.1) 56(84) bytes of data.
64 bytes from 172.16.10.1: icmp_seq=1 ttl=64 time=0.913 ms
64 bytes from 172.16.10.1: icmp_seq=2 ttl=64 time=0.970 ms
64 bytes from 172.16.10.1: icmp_seq=3 ttl=64 time=1.02 ms
64 bytes from 172.16.10.1: icmp_seq=4 ttl=64 time=1.02 ms

--- 172.16.10.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3015ms
rtt min/avg/max/mdev = 0.913/0.984/1.027/0.047 ms
```
