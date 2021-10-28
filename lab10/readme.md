Конфигурация безопасности коммутатора
Задачи
-----
Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области

Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области


----

### Решение 
<a name="0"><h2> </h2></a>
[Часть 1. топологиия](#1)

[Часть 1. Создание сети и настройка основных параметров устройства](#2)

[Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области](#3)

[Часть 3: Настройки безопасности коммутатора](#4)


 

----
<a name="1"><h2>топология</h2></a>

![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab10/topology10.JPG)

![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab10/topology_add10.JPG)
  
 [наверх](#0)

<a name="2"><h2>Часть 1. Создание сети и настройка основных параметров устройства</h2></a>

# производим базовые настройки R1
```
service password-encryption
!
hostname R1
!
!
!
enable password 7 0822404F1A0A
no ip domain-lookup
banner motd ^C AUTHORIZED PERSONS ONLY !!! DICONNECT NOW !!!!
^C
!
!
!
!
!
line con 0
 password 7 0822455D0A16
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login

```

# производим базовые настройки R2
```
service password-encryption
!
hostname R2
!
!
!
enable password 7 0822404F1A0A
no ip domain-lookup
banner motd ^C AUTHORIZED PERSONS ONLY !!! DICONNECT NOW !!!!
^C
!
!
!
!
!
line con 0
 password 7 0822455D0A16
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login

```

# производим базовые настройки S1
```
service password-encryption
!
hostname S1
!
!
!
enable password 7 0822404F1A0A
no ip domain-lookup
banner motd ^C AUTHORIZED PERSONS ONLY !!! DICONNECT NOW !!!!
^C
!
!
!
!
!
line con 0
 password 7 0822455D0A16
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login

```

# производим базовые настройки S2
```
service password-encryption
!
hostname S2
!
!
!
enable password 7 0822404F1A0A
no ip domain-lookup
banner motd ^C AUTHORIZED PERSONS ONLY !!! DICONNECT NOW !!!!
^C
!
!
!
!
!
line con 0
 password 7 0822455D0A16
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login

```

 [наверх](#0) 


<a name="3"><h2>Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области</h2></a>

# настройка интерфейсов R1:
```
interface GigabitEthernet0/0/1
 ip address 10.53.0.1 255.255.255.0
 duplex auto
 speed auto
interface Loopback1
 ip address 172.16.1.1 255.255.255.0
```

настройка интерфейсов R2:
```
interface GigabitEthernet0/0/1
 ip address 10.53.0.2 255.255.255.0
 duplex auto
 speed auto
interface Loopback1
 ip address 192.168.1.1 255.255.255.0
```

# создание конфигурации OSPF на R1
```
router ospf 56
 router-id 1.1.1.1
 log-adjacency-changes
 passive-interface default
 no passive-interface GigabitEthernet0/0/1
 network 10.53.0.0 0.0.0.255 area 0
```
# создание конфигурации OSPF на R2
```
router ospf 56
 router-id 2.2.2.2
 log-adjacency-changes
 passive-interface default
 no passive-interface GigabitEthernet0/0/1
 network 10.53.0.0 0.0.0.255 area 0

interface Loopback1
 ip ospf 56 area 0
```

# проверка установки смежности между R1 и R2

![2_f](https://github.com/ssvstdt/netwbas/blob/main/lab10/2_f.JPG)

![2_f2](https://github.com/ssvstdt/netwbas/blob/main/lab10/2_f2.JPG)
т.к. определение DR|BDR основывается на наивысшем router-id R2 выбран RD, R1 - BDR


#  выполняем команду show ip route ospf на R1
![2_g](https://github.com/ssvstdt/netwbas/blob/main/lab10/2_g.JPG)

#  проверяем доступность R2 от R1
![2_h](https://github.com/ssvstdt/netwbas/blob/main/lab10/2_h.JPG)

 [наверх](#0) 


<a name="4"><h2>Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области</h2></a>

# Шаг 1. настройка приоритеа на G0/0/1 R1.
```
interface GigabitEthernet0/0/1
  ip ospf priority 50
```
# результат R1 стал DR:
![3_1a](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_1a.JPG) 
 

# настройка таймеров на OSPF
```
interface GigabitEthernet0/0/1
 ip ospf hello-interval 30
```
 
# установка маршрута по умолчанию на R1 через Lo1
```
ip route 0.0.0.0 0.0.0.0 loopback 1
```
# получаем предупреждение, о возможном влиянии на производительность
![3_1c](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_1c.JPG) 


# распространение маршрута по умолчанию через OSPF
```
router ospf 56
default-information originate  
```
![3_1c_r2](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_1c_r2.JPG)

# объявление Lo1 на R2 как сеть точка-точка
```
interface Loopback1
ip ospf  network point-to-point
```
результат:
![3_1d](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_1d.JPG)

# изменяем базовую пропускую спсобность для маршрутизаторов
```
router ospf 56
auto-cost reference-bandwidth 10
```
важно производить настройку на всех устройствах в домене OSPF
![3_1f](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_1f.JPG)

# Подтверждение оптимизация OSPFv2 
обзор настроек интерфейса:
![3_2a](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_2a.JPG)

# повторение вывода ip route ospf
![3_2b](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_2b.JPG)

т.к. теперь анонсируется весь интерфейс маска анонсируемой сети берется из настроек интерфейса

# вывод команды show ip route ospf на маршрутизаторе R2
![3_2с](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_2c.JPG)

# проверка доступности Lo1 на R1 с R2
![3_6d](https://github.com/ssvstdt/netwbas/blob/main/lab10/3_2d.JPG)
 [наверх](#0) 

