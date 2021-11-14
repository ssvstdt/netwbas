# Настройка и проверка расширенных списков контроля доступа
### Задачи

-----

Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Настройка и проверка NAT для IPv4

Часть 3. Настройка и проверка PAT для IPv4

Часть 4. Настройка и проверка статического NAT для IPv4.

----

### Решение 
<a name="0"><h2> </h2></a>

[Часть 1. топологиия](#1)

[Часть 1: Создание сети и настройка основных параметров устройства](#2)

[Часть 2. Настройка и проверка NAT для IPv4](#3)

[Часть 3. Настройка и проверка PAT для IPv4](#4)

[Часть 4. Настройка и проверка статического NAT для IPv4.](#5)

 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab12/topology.JPG)

  
 [наверх](#0)

<a name="2"><h2>Настройка основных параметров маршрутизаторов:</h2></a>
a.	Назначьте маршрутизатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Настройте IP-адресации интерфейса, как указано в таблице выше.

i.	Настройте маршрут по умолчанию. от R2 до  R1.

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.



``` 
hostname R1
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

int gi0/0/0
ip add 209.165.200.230 255.255.255.248
no shu

int gi0/0/1
ip add 192.168.1.1 255.255.255.0
no shu

``` 
# R2
```
hostname R2

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

int gi0/0/0
ip add 209.165.200.225 255.255.255.248
no shu

int lo1
ip add 209.165.200.1 255.255.255.224
no shu
ip default-gateway 209.165.200.230

```
 [наверх](#0) 

#  Настройка базовых параметров коммутаторов
a.	Присвойте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Выключите все интерфейсы, которые не будут использоваться.

i.	Настройте IP-адресации интерфейса, как указано в таблице выше.

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

# S1
```
hostname S1
!
enable password 7 0822404F1A0A
!
!
!
no ip domain-lookup
!
username admin privilege 15 secret cisco
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
int ran fa0/2-4,fa0/7-24,gi0/1-2
shu
int vla1
ip add 192.168.1.11 255.255.255.0
no shu
```
# S2
```
hostname S2
!
enable password 7 0822404F1A0A
!
!
!
no ip domain-lookup
!
username admin privilege 15 secret cisco
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
int ran fa0/2-17,fa0/19-24,gi0/1-2
shu
int vla1
ip add 192.168.1.12 255.255.255.0
no shu
```
# проверка связности
![PINGS](https://github.com/ssvstdt/netwbas/blob/main/lab12/pings.JPG)

 [наверх](#0) 



<a name="3"><h2>Часть 2. Настройка и проверка NAT для IPv4.</h2></a>

# Настройка NAT на R1 используя пул из трех адресов 209.165.200.226-209.165.200.228.

```
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248
ip nat inside source list 1 pool PUBLIC_ACCESS
interface g0/0/1
ip nat inside
interface g0/0/0
ip nat outside
``` 
# Проверка конфигурации
![2_a_1](https://github.com/ssvstdt/netwbas/blob/main/lab12/2_a_1.JPG)

![2_a_2](https://github.com/ssvstdt/netwbas/blob/main/lab12/2_a_2.JPG)

 Во что был транслирован внутренний локальный адрес PC-B? - 209.165.200.226

 Какой тип адреса NAT является переведенным адресом? -  Dynamic NAT — при прохождении через маршрутизатор, новый адрес выбирается динамически из некоторого куска адресов.

# полностью использованный пул адресов:

![2](https://github.com/ssvstdt/netwbas/blob/main/lab12/2.JPG)

свободные адреса для трансляции отсутствуют:
![2](https://github.com/ssvstdt/netwbas/blob/main/lab12/2_2.JPG)

 [наверх](#0) 

<a name="4"><h2>Часть 3. Настройка и проверка PAT для IPv4.</h2></a>

# Настройка PAT на R1 используя созданый ранее пул из трех адресов 209.165.200.226-209.165.200.228.
```
no ip nat inside source list 1 pool PUBLIC_ACCESS
ip nat inside source list 1 pool PUBLIC_ACCESS overload 
```
![3](https://github.com/ssvstdt/netwbas/blob/main/lab12/3.JPG)


<a name="5"><h2>Часть 4. Настройка и проверка статического NAT для IPv4..</h2></a>

# настройка статического сопоставления  192.168.1.2 209.165.200.229.
```
ip nat inside source static 192.168.1.2 209.165.200.229
```

# проверка работоспособности статического сопоставления
![4](https://github.com/ssvstdt/netwbas/blob/main/lab12/4.JPG)
![4_1](https://github.com/ssvstdt/netwbas/blob/main/lab12/4_1.JPG)
