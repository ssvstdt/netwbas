Доступ к сетевым устройствам по протоколу SSH
Задачи
-----
Часть 1. Настройка основных параметров устройства

Часть 2. Настройка маршрутизатора для доступа по протоколу SSH

Часть 3. Настройка коммутатора для доступа по протоколу SSH

Часть 4. SSH через интерфейс командной строки (CLI) коммутатора




----

### Решение 
<a name="0"><h2> </h2></a>
[Часть 1. топологиия](#1)

[Часть 1: Настройка основных параметров устройства](#2)

[Часть 2. Настройка маршрутизатора для доступа по протоколу SSH](#3)

[Часть 3: Настройка коммутатора для доступа по протоколу SSH](#4)

[Часть 4: Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора](#5)


 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab5/topology.JPG)

# исправленная топология  
 ![ssh-r1](https://github.com/ssvstdt/netwbas/blob/main/lab5/topology_r.JPG)
  
 [наверх](#0)

<a name="2"><h2>нНастройка основных параметров устройства:</h2></a>
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

g.	Зашифруйте открытые пароли.

h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.

i.	Настройте и активируйте на маршрутизаторе интерфейс G0/0/1, используя информацию, приведенную в таблице адресации.:
```
!
hostname R1
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
no ip domain-lookup
!
interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 duplex auto
 speed auto
!
banner login ^C NonAuthorised users login prohibited!!! DISCONNECT IMMEDEATLY!!! ^C
!
line con 0
 password 7 0822404F1A0A
 login
!
line aux 0
!
line vty 0 4
 password 7 0822404F1A0A
 login local
!
``` 
 [наверх](#0)
 
<a name="3"><h2>Настройка маршрутизатора для доступа по протоколу SSH:</h2></a>
# Настроиваем аутентификацию устройства:
```
ip ssh version 2

ip domain-name testlab.com
!
```
# создаем ключ
```
crypto key generate rsa general-keys modulus 1024
```

# ключ создан
```
R1#sh crypto key mypubkey rsa
% Key pair was generated at: 0:35:25 UTC Март 1 1993
Key name: R1.testlab.com
 Storage Device: not specified
 Usage: General Purpose Key
 Key is not exportable.
 Key Data:
 0000787c  00001864  00002d53  000006c1  00000f3c  00005f27  00007c3d  000063a6
 000001df  00003887  00007483  000024e6  00001a29  00004cc7  00004ca9  00005902
 0000566a  00006389  0000261a  000072ca  000018ff  00006541  000058ca  7673
% Key pair was generated at: 0:35:25 UTC Март 1 1993
Key name: R1.testlab.com.server
```
# создаем пользователя 
```
username admin privilege 15 secret 5 $1$mERr$o9LE61ujQ/U6ZN.1Od2Zo.
```
# активируем SSH на VTY
```
transport input ssh
login local
```
# проверка  
 ![ssh-r1](https://github.com/ssvstdt/netwbas/blob/main/lab5/telnet_r1.JPG)

 [наверх](#0) 


<a name="4"><h2>Настройка коммутатора для доступа по протоколу SSH</h2></a>
# настраиваем основны параметры коммутатора
```
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
ip ssh version 2
ip domain-name testlab.com
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode access
interface Vlan1
 ip address 192.168.1.11 255.255.255.0
!
banner motd ^C Unathorized access PROHIBITED!!! ^C
!
!
!
line con 0
 password 7 0822455D0A16
 login
!
line vty 0 4
 password 7 0822404F1A0A
 login local
line vty 5 15
 login
!
```
 # Настраиваем коммутатор для соединения по протоколу SSH
```
ip ssh version 2

ip domain-name testlab.com
!
crypto key generate rsa general-keys modulus 1024
```
# создаем пользователя 
```
username admin privilege 15 secret 5 $1$mERr$o9LE61ujQ/U6ZN.1Od2Zo.
```
# активируем SSH на VTY
```
transport input ssh
login local
```
 [наверх](#0) 

<a name="5"><h2>Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора</h2></a>

 ![ssh_s1-r](https://github.com/ssvstdt/netwbas/blob/main/lab5/ssh_s1-r1.JPG)
 [наверх](#0)
