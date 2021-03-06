### Задачи

Настройка DHCPv6
-----
Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Проверка назначения адреса SLAAC от R1

Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1

Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1

Часть 5. Настройка и проверка DHCPv6 Relay на R2

Реализация DHCPv4 
-----
Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Настройка и проверка двух серверов DHCPv4 на R1

Часть 3. Настройка и проверка DHCP-ретрансляции на R2


----

### Решение 
<a name="0"><h2> </h2></a>
# DHCPv6
[Часть 1. топологиия](#1)

[Часть 1: Настройка основных параметров устройства](#2)

[Часть 2. Проверка назначения адреса SLAAC от R1](#3)

[Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов](#4)

[Часть 4: Настройка сервера DHCPv6 с сохранением состояния на R1](#5)

[Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.](#6)

----
# DHCPv4
[Часть 1. Таблица адресации](#7)

[Часть 1. Создание сети и настройка основных параметров устройства](#8)

[Часть 2. Настройка и проверка двух серверов DHCPv4 на R1](#9)

[Часть 3. Настройка и проверка DHCP-ретрансляции на R2](#10)


 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab8/topology.JPG)

  
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
# настройка интерфейсов маршрутизаторов 

```
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:2::2/64
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:3::1/64
!
!
ipv6 route ::/0 2001:DB8:ACAD:2::1
```

# и проверка
![1_4-ping](https://github.com/ssvstdt/netwbas/blob/main/lab8/1_4-ping.JPG)

 [наверх](#0)

<a name="3"><h2>Часть 2. проверка назначения SLAAC от R1</h2></a>

# ipconfig на PC-A
![pca-slaac](https://github.com/ssvstdt/netwbas/blob/main/lab8/2_1-pca-slaac.JPG)

 [наверх](#0) 


<a name="4"><h2>Часть 3. Настройка и проверка сервера DHCPv6 на R1</h2></a>
# текущая конфигурация PC-A
![pc-a_ipconfig](https://github.com/ssvstdt/netwbas/blob/main/lab8/3_1-pca-ipconfig.JPG) 

# конфигурация PC-A после включения DHCP на R1

![pca-ipconfig](https://github.com/ssvstdt/netwbas/blob/main/lab8/3_2-pca-ipconfig.JPG)

 [наверх](#0) 
# проверка 
![pca_ping_r2](https://github.com/ssvstdt/netwbas/blob/main/lab8/4_3_pca_ping_r2.JPG)

 [наверх](#0) 

<a name="5"><h2>Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1</h2></a>

# создание дополнительного пула
![r1_dhcp](https://github.com/ssvstdt/netwbas/blob/main/lab8/4_4_r1_dhcp.JPG)

 [наверх](#0)

<a name="6"><h2>Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.</h2></a>

# начальная конфигурация PC-B
![pcb-initial](https://github.com/ssvstdt/netwbas/blob/main/lab8/5_1_pcb_initial.JPG)


# включаем ретрансляцию адресов на R2
```
R2 (конфигурация) # интерфейс g0/0/1
R2(config-if)# ipv6 nd managed-config-flag
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
```

 [наверх](#0)


[Часть 1. Создание сети и настройка основных параметров устройства](#8)

[Часть 2. Настройка и проверка двух серверов DHCPv4 на R1](#9)

[Часть 3. Настройка и проверка DHCP-ретрансляции на R2](#10)


 

----
<a name="7"><h2>Таблица адресации</h2></a>
![Lab IPs](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_addresses.JPG)

 [наверх](#0)
<a name="8"><h2>Часть 1. Создание сети и настройка основных параметров устройства</h2></a>

# Базовая настройка маршрутизатора (дополнение часы на маршрутизаторе на сегодняшнее время и дату)
![r1_clock](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_1_3_i.JPG)

# Настройка маршрутизации между сетями VLAN на маршрутизаторе R1
![r1_rou](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_1_4_c.JPG)

# Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов
![r1_r2](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_1_5.JPG)

# VLAN на коммутаторе S1
![s1_vlan](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_1_7.JPG)

# интерфейс S1 F0/5 в качестве транка 802.1Q.
![s1_f05](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_1_9.JPG)

 [наверх](#0)

<a name="9"><h2>Настройка и проверка двух серверов DHCPv4 на R1</h2></a>

# R1 с пулами DHCPv4 для двух поддерживаемых подсетей.
![Lab IPs](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_2_1.JPG)

# IP-адрес от DHCP на PC-A
![IP_dhcp](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_2_4_c.JPG)

 [наверх](#0)

<a name="10"><h2>Настройка и проверка DHCP-ретрансляции на R2</h2></a>

# IP-адрес от DHCP на PC-B
![pcB_IP_dhcp](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_3_2_c.JPG)

# проверка назначений адресов в DHCP

![pcB_IP_dhcp](https://github.com/ssvstdt/netwbas/blob/main/lab8/II_2_3_2_d.JPG)

 [наверх](#0)
