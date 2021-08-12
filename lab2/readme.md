Задачи
-----
Часть 1. Создание и настройка сети
* Настройте имена устройств в соответствии с топологией.
* Настройте IP-адреса, как указано в таблице адресации.
* Назначьте cisco в качестве паролей консоли и VTY.
* Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.

Часть 2. Изучение таблицы МАС-адресов коммутатора
* МАС-адреса сетевых устройств.
* МАС-адресов коммутатора.
* Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов
* С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора


----

### Решение 
<a name="0"><h2> </h2></a>
[Часть 1: имена устройств в соответствии с топологией.](#1)

[Часть 1: Настройте IP-адресов, паролей и т.д.](#2)

[Часть 2: Mac Адреса](#3)

[Часть 2: очистка таблицы MAC адресов](#4)
----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab2/topology.JPG)
  
 [наверх](#0)

<a name="2"><h2>настраиваем запршенные параметры коммутаторов и  ПК:</h2></a>
 ![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab2/pc1-ipconfig.JPG)
 ![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab2/pc2-ipconfig.JPG)
# Коммутатор S1:
```
hostname S1
!
enable password 7 0822455D0A16

interface FastEthernet0/6
 switchport mode access

interface Vlan1
 ip address 192.168.1.11 255.255.255.0
!
!
!
!
line con 0
 password 7 0822404F1A0A
 login
!
line vty 0 4
 password 7 0822404F1A0A
 login
line vty 5 15
 login
```
# Коммутатор S2:

```
hostname S2
!
enable password 7 0822455D0A16

interface FastEthernet0/6
 switchport mode access

interface Vlan1
 ip address 192.168.1.12 255.255.255.0
!
!
!
!
line con 0
 password 7 0822404F1A0A
 login
!
line vty 0 4
 password 7 0822404F1A0A
 login
line vty 5 15
 login
```
 
 [наверх](#0)
 
<a name="3"><h2>MAC Адреса:</h2></a>
 # S1:
 ![s1-mac](https://github.com/ssvstdt/netwbas/blob/main/lab2/s1-mac-tab.JPG) 
 # S2
 ![s2-mac](https://github.com/ssvstdt/netwbas/blob/main/lab2/s2-mac-tab.JPG)
 [наверх](#0) 

<a name="4"><h2>очистка таблицы :</h2></a>
  ![s2-mac](https://github.com/ssvstdt/netwbas/blob/main/lab2/s2-mac-clear.JPG)

 [наверх](#0) 
 # ARP на PC2
 ![pc2-arp](https://github.com/ssvstdt/netwbas/blob/main/lab2/pc2-arp.JPG)
 [наверх](#0)
