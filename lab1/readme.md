Задачи
-----
Часть 1. Проверка конфигурации коммутатора по умолчанию 

Часть 2. Создание сети и настройка основных параметров устройства 
* Настройте базовые параметры коммутатора.
* Настройте IP-адрес для ПК.

Часть 3. Проверка сетевых подключений 
* Отобразите конфигурацию устройства.
* Протестируйте сквозное соединение, отправив эхо-запрос.
* Протестируйте возможности удаленного управления с помощью Telnet.


----

### Решение 
<a name="0"><h2> </h2></a>
[Конфигурация по умолчанию:](#1)

[Создание сети и настройка основных параметров устройства](#2)

[Проверка сетевых подключений](#3)

----
<a name="1"><h2>проверяем базовую конфигурацию после старта:</h2></a>

  

    Switch Ports Model              SW Version            SW Image
    ------ ----- -----              ----------            ----------
    *    1 26    WS-C2960-24TT-L    15.0(2)SE4            C2960-LANBASEK9-M
    
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2013 by Cisco Systems, Inc.
    Compiled Wed 26-Jun-13 02:49 by mnguyen
    
    
    
    Press RETURN to get started!
    
    
    
    Switch>en
    Switch#sh run
    Building configuration...
    
    Current configuration : 1080 bytes
    !
    version 15.0
    no service timestamps log datetime msec
    no service timestamps debug datetime msec
    no service password-encryption
    !
    hostname Switch
    !
    !
    !
    !
    !
    !
    spanning-tree mode pvst
    spanning-tree extend system-id
    !
    interface FastEthernet0/1
    !
    interface FastEthernet0/2
    !
    interface FastEthernet0/3
    !
    interface FastEthernet0/4
    !
    interface FastEthernet0/5
    !
    interface FastEthernet0/6
    !
    interface FastEthernet0/7
    !
    interface FastEthernet0/8
    !
    interface FastEthernet0/9
    !
    interface FastEthernet0/10
    !
    interface FastEthernet0/11
    !
    interface FastEthernet0/12
    !
    interface FastEthernet0/13
    ! 
    interface FastEthernet0/14
    !
    interface FastEthernet0/15
    !
    interface FastEthernet0/16
    !
    interface FastEthernet0/17 
    !
    interface FastEthernet0/18
    !
    interface FastEthernet0/19
    !
    interface FastEthernet0/20
    !
    interface FastEthernet0/21
    !
    interface FastEthernet0/22
    !
    interface FastEthernet0/23
    !
    interface FastEthernet0/24
    !
    interface GigabitEthernet0/1
    !
    interface GigabitEthernet0/2
    !
    interface Vlan1
    no ip address
    shutdown
    !
    !
    !
    !
    line con 0
    !
    line vty 0 4
    login
    line vty 5 15
    login
    !
    !
    !
    !
    end

<a name="2"><h2>настраиваем базовые параметры коммутатора и IP-адрес для ПК:</h2></a>
 ![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab1/net-lab1.JPG)
 
 [наверх](#0)
 
<a name="3"><h2>конфигурация устройства и проверка коннективити:</h2></a>
# проверка на коммутаторе
 ![ping-pc](https://github.com/ssvstdt/netwbas/blob/main/lab1/net-lab1-ping-tern.JPG) 
 # проверка на PC
 ![ping term](https://github.com/ssvstdt/netwbas/blob/main/lab1/net-lab1-ping-pc.JPG)
 [наверх](#0) 

# конфигурация коммутатора
```
S1#sh run
Building configuration...

Current configuration : 1261 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
!
!
line con 0
 logging synchronous
!
line vty 0 4
 password 7 0822404F1A0A
 login
line vty 5 15
 login
!
!
!
!
end
```
 [наверх](#0)
 
 # проверка коннективити
 ![ping term](https://github.com/ssvstdt/netwbas/blob/main/lab1/net-lab1-telnet-pc.JPG)
 [наверх](#0)
