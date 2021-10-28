Конфигурация безопасности коммутатора
Задачи
-----
Часть 1. Настройка основного сетевого устройства

Часть 2. Настройка сетей VLAN

Часть 3: Настройки безопасности коммутатора



----

### Решение 
<a name="0"><h2> </h2></a>
[Часть 1. топологиия](#1)

[Часть 1. Настройка основного сетевого устройства](#2)

[Часть 2. Настройка сетей VLAN](#3)

[Часть 3: Настройки безопасности коммутатора](#4)


 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab9/topology.JPG)

![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab9/topology_add.JPG)
  
 [наверх](#0)

<a name="2"><h2>Настройка основного сетевого устройства</h2></a>

# Конфигурация R1
```
enable
configure terminal
hostname R1
no ip domain lookup
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.201 192.168.10.202
!
ip dhcp pool Students
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 domain-name CCNA2.Lab-11.6.1
!
interface Loopback0
 ip address 10.10.1.1 255.255.255.0
!
interface GigabitEthernet0/0/1
 description Link to S1
 ip dhcp relay information trusted
 ip address 192.168.10.1 255.255.255.0
 no shutdown
!
line con 0
 logging synchronous
 exec-timeout 0 0

```
![1_2](https://github.com/ssvstdt/netwbas/blob/main/lab9/1_2_b.JPG)

 [наверх](#0) 


<a name="3"><h2>Часть 2. Настройка сетей VLAN</h2></a>

# S1
![2_S1](https://github.com/ssvstdt/netwbas/blob/main/lab9/2_1all.JPG)

# S1
![2_S2](https://github.com/ssvstdt/netwbas/blob/main/lab9/2_2all.JPG)

 [наверх](#0) 


<a name="4"><h2>Часть 3. Настройки безопасности коммутатора</h2></a>
# Шаг 1. Релизация магистральных соединений 802.1Q.
![3_1a1](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_1_a.JPG) 

![3_1a2](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_1_a_2.JPG) 

# проверка
![3_1d1](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_1_d.JPG)
![3_1d2](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_1_d_2.JPG)

 
# Безопасность неиспользуемых портов коммутатора
![3_3b](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_3_b.JPG)
![3_3b2](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_3_b_2.JPG)

# Документирование и реализация функций безопасности порта
![3_4a](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_4_a.JPG)
![3_4c1](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_4_c.JPG)
![3_4c2](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_4_c_2.JPG)

# Реализация DHCP snooping
![3_5d](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_5_d.JPG)
![3_5e](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_5_e.JPG)
![3_5f](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_5_f.JPG)

# Реализация PortFast и BPDU Guard
![3_6c1](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_6_c1.JPG)
![3_6c2](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_6_c2.JPG)

# Проверка наличия сквозного ⁪подключения
![3_7](https://github.com/ssvstdt/netwbas/blob/main/lab9/3_7.JPG)

 [наверх](#0) 

