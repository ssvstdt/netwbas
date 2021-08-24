Задачи
-----
Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
* настройка маршрутизатора
* настройка коммутатора
Часть 2. Ручная настройка IPv6-адресов
* назначение IPv6 адресов интерфейсам на R1
* активизация IPv6  маршрутизации на R1
* назначение статического адреса на S1
Часть 3. Проверка сквозного соединения



----

### Решение 
<a name="0"><h2> </h2></a>
[Часть 1. топологиия](#1)

[Часть 1: настройка маршрутизатора](#2)

[Часть 1: Mac Адреса](#3)

[Часть 2: Назначьте IPv6-адреса интерфейсам Ethernet на R1](#4)

[Часть 2: Активируйте IPv6-маршрутизацию на R1](#5)

[Часть 2: Назначьте IPv6-адреса интерфейсу управления (SVI) на S1](#6)

[Часть 3: Проверка сквозного подключения](#7)

 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab4/topology.JPG)
  
 [наверх](#0)

<a name="2"><h2>настраиваем запрошенные параметры маршрутизатора:</h2></a>
 # Назначьте имя хоста и настройте основные параметры устройства:
```
!
hostname R1
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
line con 0
 password 7 0822404F1A0A
 login
!
line aux 0
!
line vty 0 4
 password 7 0822404F1A0A
 login
!
``` 
 [наверх](#0)
 
<a name="3"><h2>настройка коммутатора:</h2></a>
# Назначьте имя хоста и настройте основные параметры устройства:
```
!
hostname S1
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
line con 0
 password 7 0822404F1A0A
 login
!
line aux 0
!
line vty 0 4
 password 7 0822404F1A0A
 login
!
```
 [наверх](#0) 

<a name="4"><h2>Назначаем IPv6-адреса интерфейсам Ethernet на R1</h2></a>
```
!
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:A::1/64
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
!
```
 # проверка назначения адресов на интерфейсах
  ![ipv6-r1](https://github.com/ssvstdt/netwbas/blob/main/lab4/ipv6-r1.JPG)

 # проверка назначения групп мнооадресной рассылки на интерфейсах
  ![ipv6-r1](https://github.com/ssvstdt/netwbas/blob/main/lab4/gr_r1-0.JPG)
  ![ipv6-r1](https://github.com/ssvstdt/netwbas/blob/main/lab4/gr_r1-1.JPG)
 [наверх](#0) 

<a name="5"><h2>Активируем IPv6-маршрутизацию на R1</h2></a>
```
!
ipv6 unicast-routing
!
```
 # проверяем ipconfig на PC-B
 ![ipconfig_pc-b](https://github.com/ssvstdt/netwbas/blob/main/lab4/ipconfig_pc-b.JPG)
 [наверх](#0)

<a name="6"><h2>Назначаем IPv6-адреса интерфейсу управления (SVI) на S1</h2></a>
```
!
interface Vlan1
 no ip address
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:1::B/64
!
```
 # проверяем назначение IPv6 на vlan1
 ![vlan-s1](https://github.com/ssvstdt/netwbas/blob/main/lab4/vlan1-s1.JPG)
 [наверх](#0)

<a name="7"><h2>Проверка сквозного подключения</h2></a>
 # проверяем ping PC-A на FE80::1
 ![ping_pca-r1](https://github.com/ssvstdt/netwbas/blob/main/lab4/ping_pca-r1.JPG)
 [наверх](#0)

 # проверяем ping PC-A на S1
 ![ping_pca-s1](https://github.com/ssvstdt/netwbas/blob/main/lab4/ping_pca-s1.JPG)
 [наверх](#0)

 # проверяем trace-pca-pcb
 ![trace-pca-pcb](https://github.com/ssvstdt/netwbas/blob/main/lab4/trace-pca-pcb.JPG)
 [наверх](#0)

 # проверяем ping PC-B на PC-A
 ![ping_pcb-pca](https://github.com/ssvstdt/netwbas/blob/main/lab4/ping_pcb-pca.JPG)
 [наверх](#0)

 # проверяем ping PC-B на R1
 ![ping_pcb-r1](https://github.com/ssvstdt/netwbas/blob/main/lab4/ping_pcb-r1.JPG)
 [наверх](#0)





