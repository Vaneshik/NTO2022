# NTO2022 ThereIsNoInfoRoma _Axiom

## Легенда

Злоумышленник с помощью цепочки атак получил доступ к инфраструктуре предприятия, связанного с электроснабжением города. В результате большинство компьютеров было захвачено, доступ к ним потерян, а кое-где даже зашифрованы данные. Захватив промышленный сегмент, злоумышленник распространил бот-зловред, в результате которого на подстанции происходят аварийные события и блокируется работа операторов.

## Задача

Вам необходимо:
- Провести инвентаризацию хостов в 4 сетевых сегментах': сегмент DMZ (10.х.2.0/24), сегмент SERVERS (10.х.3.0/24), сегмент OFFICE (10.x.4.0/24) и сегмент АСУ ТП (10.x.239.0/24, 10.x.240.0/24)?.
- Расследовать цепочку атак злоумышленника, используя журналы безопасности и логи средств защиты.
- Вернуть контроль над захваченными компьютерами, восстановив легитимный доступ.
- Найти и расшифровать ценные файлы. - Остановить распространение бота-зловреда во всей инфраструктуре, а также понять, что он делает.

1) В каждом сегменте сети машины с адресами .1 .2 .3 .4 в четвертом октете не участвуют в задаче.
2) В вашей инфраструктуре в IP адрес вместо «Х» необходимо подставить второй октет ір адреса машины kali linux.

---

## user_names.txt 
```
#Учетка Linux серверов
admin

#Локальный админ windows
Administator (builtin)
Администратор

#Доменный админ windows
company.local\Administrator

#Учетная запись на межсетевом экране
user

#Админитративная учетка диспетчера АСУТП
oper
```

---

## Результаты скана nmap -ом:

### 10.31.239.0/24

```shell
$ nmap 10.31.239.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 08:52 MSK
Nmap scan report for 10.31.239.5
Host is up (0.0074s latency).
Not shown: 990 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
1433/tcp  open  ms-sql-s
3389/tcp  open  ms-wbt-server
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
 
Nmap scan report for 10.31.239.6
Host is up (0.0049s latency).
Not shown: 990 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49175/tcp open  unknown
49176/tcp open  unknown
```

### 10.31.240.0/24

```shell
$ nmap 10.31.240.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 08:53 MSK
Nmap scan report for 10.31.240.5
Host is up (0.010s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
 
Nmap scan report for 10.31.240.6
Host is up (0.0064s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
 
Nmap scan report for 10.31.240.9
Host is up (0.0055s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
 
Nmap scan report for 10.31.240.10
Host is up (0.013s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
 
Nmap scan report for 10.31.240.14
Host is up (0.0077s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49156/tcp open  unknown
49157/tcp open  unknown
```

### 10.31.240.0/24

```shell
$ nmap 10.31.240.0/24 -sV
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 09:08 MSK
Nmap scan report for 10.31.240.5
Host is up (0.0065s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    JBoss Enterprise Application Platform
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
Nmap scan report for 10.31.240.6
Host is up (0.0069s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    JBoss Enterprise Application Platform
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
Nmap scan report for 10.31.240.9
Host is up (0.0076s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    JBoss Enterprise Application Platform
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
Nmap scan report for 10.31.240.10
Host is up (0.0080s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    JBoss Enterprise Application Platform
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
Nmap scan report for 10.31.240.14
Host is up (0.0093s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
22/tcp    open  ssh                OpenSSH for_Windows_8.6 (protocol 2.0)
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
49157/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: A31-ENTEK; OS: Windows; CPE: cpe:/o:microsoft:windows
```

### 10.31.239.0/24

```shell
$ nmap 10.31.239.0/24 -sV
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 09:11 MSK
Nmap scan report for 10.31.239.5
Host is up (0.0064s latency).
Not shown: 990 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
22/tcp    open  ssh                OpenSSH for_Windows_8.6 (protocol 2.0)
80/tcp    open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: company)
1433/tcp  open  ms-sql-s           Microsoft SQL Server 2012 11.00.7001
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: OIK-SERVER; OS: Windows; CPE: cpe:/o:microsoft:windows

Nmap scan report for 10.31.239.6
Host is up (0.0071s latency).
Not shown: 990 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
22/tcp    open  ssh                OpenSSH for_Windows_8.6 (protocol 2.0)
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: company)
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49175/tcp open  msrpc              Microsoft Windows RPC
49176/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: OIK-CLIENT; OS: Windows; CPE: cpe:/o:microsoft:windows
```

> Из результатов убраны ip с последним октетом .1 .2 .3 .4

---
