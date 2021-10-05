Внедрение маршрутизации между виртуальными локальными сетями
Задачи
-----
Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Создание сетей VLAN и назначение портов коммутатора

Часть 3. Настройка транка 802.1Q между коммутаторами.

Часть 4. Настройка маршрутизации между сетями VLAN

Часть 5. Проверка, что маршрутизация между VLAN работает






----

### Решение 
<a name="0"><h2> </h2></a>
[Часть 1. топологиия](#1)

[Часть 1: Настройка основных параметров устройства](#2)

[Часть 2. Настройка базовых параметров каждого коммутатора.](#3)

[Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами](#4)

[Часть 4: Настройка маршрутизации между сетями VLAN](#5)

[Часть 5. Проверка работоспособности маршрутизации между VLAN](#6)


 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab6/topology.JPG)

  
 [наверх](#0)

<a name="2"><h2>Настройка основных параметров устройства:</h2></a>
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

g.	Зашифруйте открытые пароли.

h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.

```
hostname R1
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
clock timezone MSC 3
!
ip ssh version 2
no ip domain-lookup
ip domain-name testlab.com
!
banner login ^C NonAuthorised users login prohibited!!! DISCONNECT IMMEDEATLY!!! ^C
!
line con 0
 password 7 0822404F1A0A
 login
!
line aux 0
login 

!
line vty 0 4
 password 7 0822404F1A0A
 login local
!
``` 
 [наверх](#0)
 
# Настройка параметров коммутаторов:
```
hostname S1
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
clock timezone MSC 3
!
ip ssh version 2
no ip domain-lookup
ip domain-name testlab.com
!
banner login ^C NonAuthorised users login prohibited!!! DISCONNECT IMMEDEATLY!!! ^C
!
line con 0
 password 7 0822404F1A0A
 login
!
line aux 0
login 

!
line vty 0 4
 password 7 0822404F1A0A
 login local
!
```

 [наверх](#0) 
<a name="3"><h2>Часть 2. Создание сетей VLAN и назначение портов коммутатора</h2></a>
# Создание сетей VLAN на коммутаторах
#S1
```
vlan 10
name MANAGEMENT
vlan 20 
name SALES
vlan 30
name OPERATIONS
vlan 999
name PARKING_LOT
vlan 1000
name DEFAULT_NATIVE

!
interface FastEthernet0/6
 switchport access vlan 20
!
interface Vlan1
 no ip address
!
interface Vlan10
 ip address 192.168.10.11 255.255.255.0
!
ip default-gateway 192.168.10.1

```
#S2
```
vlan 10
name MANAGEMENT
vlan 20 
name SALES
vlan 30
name OPERATIONS
vlan 999
name PARKING_LOT
vlan 1000
name DEFAULT_NATIVE

interface FastEthernet0/18
 switchport access vlan 30
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 ip address 192.168.10.12 255.255.255.0
!
ip default-gateway 192.168.10.1

```

 [наверх](#0) 


<a name="4"><h2>Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами</h2></a>
# настраиваем основны параметры коммутатора
```
!
interface FastEthernet0/1
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,30,1000
 switchport mode trunk
!
```
 # Настройка порта fa0/5 в сторону маршрутизатора на S1
```
interface FastEthernet0/5
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,30,1000
 switchport mode trunk
```
 [наверх](#0) 

<a name="5"><h2>Часть 4. Настройка маршрутизации между сетями VLAN</h2></a>
```
!
interface GigabitEthernet0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
!
interface GigabitEthernet0/0/1.1000
 encapsulation dot1Q 1000
 no ip address
!
```
 [наверх](#0)
<a name="6"><h2>Часть 5. Проверка работоспособности маршрутизации между VLAN</h2></a>
# ping  from PC-A
 ![pca-pings](https://github.com/ssvstdt/netwbas/blob/main/lab6/pca-pings.JPG)
 [наверх](#0)
# tracert  from PC-B
 ![pcb-tracert](https://github.com/ssvstdt/netwbas/blob/main/lab6/pcb-tracert.JPG)
 [наверх](#0)

 [наверх](#0)
