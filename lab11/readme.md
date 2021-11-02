# Настройка и проверка расширенных списков контроля доступа
### Задачи

-----

Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Настройка и проверка списков расширенного контроля доступа

----

### Решение 
<a name="0"><h2> </h2></a>
# DHCPv6
[Часть 1. топологиия](#1)

[Часть 1: Создание сети и настройка основных параметров устройства](#2)

[Часть 2. Настройка и проверка списков расширенного контроля доступа](#3)

 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab11/topology.JPG)

  
 [наверх](#0)

<a name="2"><h2>Настройка основных параметров устройства:</h2></a>
a.	Присвойте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Отключите все неиспользуемые порты.

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации


``` 
hostname S2
!
enable password 7 0822404F1A0A
!
!
!
no ip domain-lookup
!
username admin privilege 15 password 7 0822455D0A16
!
!
banner motd ^CCONNECTION PROHIBITED, DISCONNECT IMMEDIETLY!!!^C
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login local
!
line vty 0 4
 password 7 0822455D0A16
 login local
line vty 5 15
 login
``` 

 [наверх](#0) 

#  Настройка базовых параметров маршрутизаторов
```
hostname R2
!
enable password 7 0822404F1A0A
!
!
!
no ip domain-lookup
ipv6 unicast-routing
!
username admin privilege 15 password 7 0822455D0A16
!
!
banner motd ^CCONNECTION PROHIBITED, DISCONNECT IMMEDIATLY!!!^C
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login local
!
line vty 0 4
 password 7 0822455D0A16
 login local
line vty 5 15
 login
```

 [наверх](#0) 
# создание необходимых VLAN на коммутаторах
```
vlan 20
name MANAGEMENT
vlan 30
name OPERATIONS
vlan 40 
name SALES
vlan 999
name PARKINGLOT
vlan 1000
name DEFAULTNATIVE
```
# настройка интерфейсов управления и маршрута по умолчанию
S1
```
int vlan20 
ip add 10.20.0.2 255.255.255.0

ip default-gateway 10.20.0.1
```

S2
```
int vlan20 
ip add 10.20.0.3 255.255.255.0

ip default-gateway 10.20.0.1
```
# деактивация неиспользуемых портов и установка статического режима доступа
S1
```
int range F0/2-4, F0/7-24, G0/1-2
sw mode acce
sw acc vla 999
sw nonego
shu
```
![VLAN_1c1](https://github.com/ssvstdt/netwbas/blob/main/lab11/VLAN_1c1.JPG)

S2
```
int range F0/2-4, F0/6-17, F0/19-24, G0/1-2
sw mode acce
sw acc vla 999
sw nonego
shu
```
![VLAN_1c2](https://github.com/ssvstdt/netwbas/blob/main/lab11/VLAN_1c2.JPG)

# настройка VLAN на интерфейсах и организация магистральных интерфейсов
S1
```
int fa0/6
sw mode acc 
sw none
sw acc vla 30
int fa0/1
sw mode trun
sw tru all vla 10,20,30,1000
sw tru native vla 1000
```
![VLAN_2a1](https://github.com/ssvstdt/netwbas/blob/main/lab11/VLAN_2a1.JPG)
 
S2
```
int fa0/18
sw mode acc 
sw none
sw acc vla 40
int fa0/1
sw mode trun
sw tru all vla 10,20,30,1000
sw tru native vla 1000
```
![VLAN_2a2](https://github.com/ssvstdt/netwbas/blob/main/lab11/VLAN_2a2.JPG)

# настройка магистральных интерфейсов в сторону маршрутизаторов
```
int fa0/5
sw mode trun
sw tru all vla 10,20,30,1000
sw tru native vla 1000
```
![trunk_1d](https://github.com/ssvstdt/netwbas/blob/main/lab11/trunk_1d.JPG)

# настройка интерфейсов маршрутизатора R1 

```
int g0/0/1
no shu
int g0/0/1.20
enc dot1q 20
ip add 10.20.0.1 255.255.255.0
int g0/0/1.30
enc dot1q 30
ip add 10.30.0.1 255.255.255.0
int g0/0/1.40
enc dot1q 40
ip add 10.40.0.1 255.255.255.0
int g0/0/1.1000
enc dot1q 1000
int lo1
ip add 172.16.1.1 255.255.255.0
```
![r1_int](https://github.com/ssvstdt/netwbas/blob/main/lab11/r1_int.JPG)

# настройка R2
```
int g0/0/1
no shu
ip add 10.20.0.4 255.255.255.0
exit
ip route 0.0.0.0 0.0.0.0 10.20.0.1 
```

# настройка дотупа по SSH
```
username SSHadmin secret $cisco123!
ip domain-name ccna-lab.com
crypto key generate rsa general-keys  modulus 1024
ip  ssh ver 2

line vty 0 4 
transport input ssh 
login local
```
 проверка
![ssh_check](https://github.com/ssvstdt/netwbas/blob/main/lab11/ssh_check.JPG)

# включение защищенных веб-служб на R1
```
ip http secure-server 
ip http authentication local

```
# Проверка подключения с  PC-XXX
![ping_pc-a](https://github.com/ssvstdt/netwbas/blob/main/lab11/ping_pc-a.JPG)
![ping_pc-b](https://github.com/ssvstdt/netwbas/blob/main/lab11/ping_pc-b.JPG)
![ssh_pc-b](https://github.com/ssvstdt/netwbas/blob/main/lab11/ssh_pc-b.JPG)

 [наверх](#0)

<a name="3"><h2>Часть 2. Настройка и проверка списков расширенного контроля доступа</h2></a>

# анализ и реализация списков контроля доступа в соответстви с политиками:
# Политика1. 
Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).
```
 remark Policy1
 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
``` 
# Политика 2. 
Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).
```
 remark Policy2
 permit tcp 10.40.0.0 0.0.0.255 any eq 22 
 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq www 
 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443 
 deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq www 
 deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq www 
 deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq 443 
 deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq 443 
 permit tcp 10.40.0.0 0.0.0.255 any eq www 
 permit tcp 10.40.0.0 0.0.0.255 any eq 443 
```
# Политика3. 
Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам
```
 remark Policy3
 deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255
 deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255
 permit icmp any any
```
# Результирующий список доступа:
```
ip access-list extended POLICY_SALES
 remark Policy1
 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
 remark Policy2
 permit tcp 10.40.0.0 0.0.0.255 any eq 22 
 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq www 
 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443 
 deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq www 
 deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq www 
 deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq 443 
 deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq 443 
 permit tcp 10.40.0.0 0.0.0.255 any eq www 
 permit tcp 10.40.0.0 0.0.0.255 any eq 443 
 remark Policy3
 deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255
 deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255
 permit icmp any any
 deny ip any any
```
# применение на интерфейсе
```
interface GigabitEthernet0/0/1.40
 encapsulation dot1Q 40
 ip address 10.40.0.1 255.255.255.0
 ip access-group POLICY_SALES in
```

# проверка
![r1_pol_counters](https://github.com/ssvstdt/netwbas/blob/main/lab11/r1_pol_counters.JPG)
![pc-b_checkacl](https://github.com/ssvstdt/netwbas/blob/main/lab11/pc-b_checkacl.JPG)


# Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам.
```
ip access-list extended POLICY_OPER
 deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255
 permit icmp any any
 deny ip any any
```

# применение на интерфейсе
```
interface GigabitEthernet0/0/1.30
 encapsulation dot1Q 30
 ip address 10.30.0.1 255.255.255.0
 ip access-group POLICY_OPER in
```
# проверка
![r1_pol_counters](https://github.com/ssvstdt/netwbas/blob/main/lab11/r1_pol_counters2.JPG)
![pc-b_checkacl](https://github.com/ssvstdt/netwbas/blob/main/lab11/pc-a_checkacl.JPG)

 [наверх](#0) 

