# Настройка протоколов CDP, LLDP и NTP
### Задачи

-----

Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP

Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP

Часть 4. Настройка и проверка NTP


----

### Решение 
<a name="0"><h2> </h2></a>

[Часть 1. топологиия](#1)

[Часть 1: Создание сети и настройка основных параметров устройства](#2)

[Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP](#3)

[Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP](#4)

[Часть 4. Настройка и проверка NTP](#5)

 

----
<a name="1"><h2>топология</h2></a>
![Lab Map](https://github.com/ssvstdt/netwbas/blob/main/lab13/topology.JPG)

  
 [наверх](#0)

<a name="2"><h2>Настройка основных параметров маршрутизаторов:</h2></a>
a.	Назначьте маршрутизатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Настройка интерфейсов, перечисленных в таблице выше

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации

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
banner motd ^CONLY AUTHORIZED USERS!!! ^C
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
int lo1
ip add 172.16.1.1 255.255.255.0
no shu

int gi0/0/1
ip add 10.22.0.1 255.255.255.0
no shu


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
banner motd ^CAUTHORIZED USERS ONLY!!!^C!
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
ip add 10.22.0.2 255.255.255.0
no shu
ip default-gateway 10.22.0.1
```
![PINGS](https://github.com/ssvstdt/netwbas/blob/main/lab13/1_s1.JPG)

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
banner motd ^CAUTHORIZED USERS ONLY!!!^C
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
int ran fa0/2-24,gi0/1-2
shu
int vla1
ip add 10.22.0.3 255.255.255.0
no shu
ip default-gateway 10.22.0.1
```
# проверка связности
![PINGS](https://github.com/ssvstdt/netwbas/blob/main/lab13/1_s2.JPG)

 [наверх](#0) 



<a name="3"><h2>Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP.</h2></a>

# На R1 используем  команду show cdp
![CDP0](https://github.com/ssvstdt/netwbas/blob/main/lab13/2_a.JPG)
# видим один интерфейс в рабочем состоянии


# определяем соседа
![CDP](https://github.com/ssvstdt/netwbas/blob/main/lab13/2_a1.JPG) 

# запрашиваем информацию по соседу:
![PINGS](https://github.com/ssvstdt/netwbas/blob/main/lab13/2_b.JPG)
# определяем версию ОС на S1 - 15.0(2)




 [наверх](#0) 

<a name="4"><h2>Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP.</h2></a>

# включаем протоко LLDP, по умолчанию он не включен.
```
lldp run
```

# запрашиваем информацию о соседях обнаруженных по протоколу LLDP:
```
sh lldp nei
```
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/3_a0.JPG)
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/3_a1.JPG)
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/3_a2.JPG)


<a name="5"><h2>Часть 4. Настройка NTP.</h2></a>

# текущее время на устройствах:
![4](https://github.com/ssvstdt/netwbas/blob/main/lab13/4_a.JPG)
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/4_a1.JPG)
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/4_a2.JPG)


# Устанавливаем время на R1
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/4_a3.JPG)

# устанавливаем настройки на R1
```
ntp master
```

# объявляем источник времени на S1 и S2
```
ntp server 10.22.0.1
```
# через несколько минут проверяем результат:
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/4_a4.JPG)
![3](https://github.com/ssvstdt/netwbas/blob/main/lab13/4_a5.JPG)


## Время на всех устройствах синхронизированно!