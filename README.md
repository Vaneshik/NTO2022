# NTO2022 ThereIsNoInfoRoma _Axiom

## Легенда

Злоумышленник с помощью цепочки атак получил доступ к инфраструктуре предприятия, связанного с электроснабжением города. В результате большинство компьютеров было захвачено, доступ к ним потерян, а кое-где даже зашифрованы данные. Захватив промышленный сегмент, злоумышленник распространил бот-зловред, в результате которого на подстанции происходят аварийные события и блокируется работа операторов.

## Задача

Вам необходимо:
- Провести инвентаризацию хостов в 4 сетевых сегментах: сегмент DMZ (10.х.2.0/24), сегмент SERVERS (10.х.3.0/24), сегмент OFFICE (10.x.4.0/24) и сегмент АСУ ТП (10.x.239.0/24, 10.x.240.0/24).
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

### 10.31.2.0/24 ( Сегмент DMZ )

```shell
$ nmap 10.31.2.0/24 -sV

Nmap scan report for 10.31.2.10
Host is up (0.0043s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
80/tcp   open  http    nginx 1.14.2
3306/tcp open  mysql   MySQL (unauthorized)
8080/tcp open  http    nginx 1.14.2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
Nmap scan report for 10.31.2.11
Host is up (0.0049s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 6.7p1 Debian 5 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.10 ((Debian))
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: CLEAN-DRUPAL; OS: Linux; CPE: cpe:/o:linux:linux_kernel
 
Nmap scan report for 10.31.2.12
Host is up (0.0048s latency).
Not shown: 985 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
22/tcp    open  ssh                OpenSSH for_Windows_8.6 (protocol 2.0)
25/tcp    open  smtp               SLmail smtpd 5.5.0.4433
79/tcp    open  finger             SLMail fingerd
106/tcp   open  pop3pw             SLMail pop3pw
110/tcp   open  pop3               BVRP Software SLMAIL pop3d
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
Service Info: Host: a31-slmail; OS: Windows; CPE: cpe:/o:microsoft:windows
 
Nmap scan report for 10.31.2.53
Host is up (0.0046s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
53/tcp open  domain  ISC BIND
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### 10.31.3.0/24 ( Сегмент SERVERS )

```shell
$ nmap 10.31.3.0/24 -sV

Nmap scan report for 10.31.3.10
Host is up (0.0049s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-03-10 06:31:55Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: company.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: company.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: Host: NS2; OS: Windows; CPE: cpe:/o:microsoft:windows
 
Nmap scan report for 10.31.3.20
Host is up (0.0039s latency).
Not shown: 969 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
25/tcp   open  smtp          Microsoft Exchange smtpd
80/tcp   open  http          Microsoft IIS httpd 10.0
81/tcp   open  http          Microsoft IIS httpd 10.0
110/tcp  open  pop3          Microsoft Exchange 2007-2010 pop3d
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
143/tcp  open  imap          Microsoft Exchange 2007-2010 imapd
443/tcp  open  ssl/http      Microsoft IIS httpd 10.0
444/tcp  open  ssl/http      Microsoft IIS httpd 10.0
445/tcp  open  microsoft-ds?
465/tcp  open  smtp          Microsoft Exchange smtpd
587/tcp  open  smtp          Microsoft Exchange smtpd
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
808/tcp  open  ccproxy-http?
993/tcp  open  ssl/imap      Microsoft Exchange 2007-2010 imapd
995/tcp  open  ssl/pop3      Microsoft Exchange 2007-2010 pop3d
1801/tcp open  msmq?
2103/tcp open  msrpc         Microsoft Windows RPC
2105/tcp open  msrpc         Microsoft Windows RPC
2107/tcp open  msrpc         Microsoft Windows RPC
2525/tcp open  smtp          Microsoft Exchange smtpd
3389/tcp open  ms-wbt-server Microsoft Terminal Services
3800/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
3801/tcp open  mc-nmf        .NET Message Framing
3828/tcp open  mc-nmf        .NET Message Framing
6001/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
6565/tcp open  msrpc         Microsoft Windows RPC
6566/tcp open  msrpc         Microsoft Windows RPC
6667/tcp open  msrpc         Microsoft Windows RPC
6668/tcp open  msrpc         Microsoft Windows RPC
6689/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: mx1.company.local; OS: Windows; CPE: cpe:/o:microsoft:windows
 
Nmap scan report for 10.31.3.50
Host is up (0.0035s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-03-10 06:32:07Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: company.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: company.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: Host: NS1; OS: Windows; CPE: cpe:/o:microsoft:windows
```

### 10.31.4.0/24 ( Сегмент OFFICE )

```shell 
$ nmap 10.31.4.0/24 -sV

Nmap scan report for 10.31.4.6
Host is up (0.0098s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
 
Nmap scan report for 10.31.4.8
Host is up (0.0053s latency).
Not shown: 992 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
22/tcp    open  ssh                OpenSSH for_Windows_8.6 (protocol 2.0)
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: company)
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: BUCHGARM; OS: Windows; CPE: cpe:/o:microsoft:windows
 
Nmap scan report for 10.31.4.10
Host is up (0.0036s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
 
Nmap scan report for 10.31.4.13
Host is up (0.0036s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

### 10.31.239.0/24 (сегмент АСУ ТП [I] )

```shell
$ nmap 10.31.239.0/24 -sV

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

### 10.31.240.0/24 (сегмент АСУ ТП [II] )
```shell
$ nmap 10.31.240.0/24 -sV

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



> Из результатов убраны ip с последним октетом .1 .2 .3 .4

---

## 10.31.2.10

- По 80 порту расположен сайт на WordPress.
- Результаты скана nikto:
```shell
$ nikto -h 10.31.2.10
 Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.31.2.10
+ Target Hostname:    10.31.2.10
+ Target Port:        80
+ Start Time:         2022-03-10 11:36:52 (GMT3)
---------------------------------------------------------------------------
+ Server: nginx/1.14.2
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ Uncommon header 'link' found, with contents: <http://10.31.2.10/wp-json/>; rel="https://api.w.org/"
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Uncommon header 'x-redirect-by' found, with contents: WordPress
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Entry '/wp-admin/' in robots.txt returned a non-forbidden or redirect HTTP code (302)
+ "robots.txt" contains 2 entries which should be manually viewed.
+ /wp-content/plugins/akismet/readme.txt: The WordPress Akismet plugin 'Tested up to' version usually matches the WordPress version
+ /wp-links-opml.php: This WordPress script reveals the installed version.
+ OSVDB-3092: /license.txt: License file found may identify site software.
+ /wp-app.log: Wordpress' wp-app.log may leak application/system details.
+ /wordpresswp-app.log: Wordpress' wp-app.log may leak application/system details.
+ /: A Wordpress installation was found.
+ /wordpress: A Wordpress installation was found.
+ Cookie wordpress_test_cookie created without the httponly flag
+ /wp-login.php: Wordpress login found
+ 7893 requests: 0 error(s) and 16 item(s) reported on remote host
+ End Time:           2022-03-10 11:41:02 (GMT3) (250 seconds)
```

- robots.txt

```
User-agent: * 
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php

Sitemap: http://10.31.2.10/wp-sitemap.xml
```

- http://10.31.2.10/wp-admin/ редирект на страницу авторизации
- WordPress 5.9.1
- Установлен Akismet plugin
- http://10.31.2.10/wp-json/ открытое API
- http://10.31.2.10/wp-json/wp/v2/users пользователь admin
```json
{
  "id": 1,
  "name": "admin",
  "url": "http://10.31.2.10",
  "description": "",
  "link": "http://10.31.2.10/author/admin/",
  "slug": "admin",
  "avatar_urls": {
    "24": "http://0.gravatar.com/avatar/f65445725c053a5c7d4e80d8e20dbf0e?s=24&d=mm&r=g",
    "48": "http://0.gravatar.com/avatar/f65445725c053a5c7d4e80d8e20dbf0e?s=48&d=mm&r=g",
    "96": "http://0.gravatar.com/avatar/f65445725c053a5c7d4e80d8e20dbf0e?s=96&d=mm&r=g"
  },
  "meta": [],
  "_links": {
    "self": [
      {
        "href": "http://10.31.2.10/wp-json/wp/v2/users/1"
      }
    ],
    "collection": [
      {
        "href": "http://10.31.2.10/wp-json/wp/v2/users"
      }
    ]
  }
}
```
