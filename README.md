# Домашнее задание к занятию 12.1 "`Уязвимости и атаки на информационные системы`" - `Падеев Василий`


   
### Задание 1. 


Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя nmap.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

Какие сетевые службы в ней разрешены?

Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)

Приведите ответ в свободной форме.

Решение...


Сетевые службы:

21/tcp   open  ftp         vsftpd 2.3.4 - Выполнение бэкдор-команд (vsftpd 2.3.4 - Backdoor Command Execution)
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) - 
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login       OpenBSD or Solaris rlogind
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1


Уязвимости:

1. FTP (vsftpd 2.3.4) - Выполнение бэкдор-команд (CVE-2011-2523)

2. OpenSSH 4.7p1 - Проблемы с безопасностью (CVE-2008-5161). SSH-протокол сам по себе безопасен, ошибки в реализации версии OpenSSH 4.7p1 могут позволить атакующему выполнить команду.

3. Metasploitable root shell (порт 1524, bindshell). На порту 1524 работает сервис, предоставляющий доступ к корневой оболочке через bind shell. Это позволяет злоумышленникам получить доступ с привилегиями суперпользователя, если они подключатся к этому порту. 

`При необходимости прикрепитe сюда скриншоты

![answer1](https://github.com/Vasiliy-Ser/homework_12.1/blob/3d5fc3940e284d3065ef26abd576c0e2d9b45b61/img/12.11.png)


---

### Задание 2. 


Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?

Как отвечает сервер?

Приведите ответ в свободной форме.


Решение...

Отличия режимов сканирования:

SYN:
В этом режиме nmap отправляет пакет с флагом SYN, чтобы установить соединение, но не завершает его (не отправляет флаг ACK). Это создаёт "полуоткрытое" соединение.
Сетевой трафик выглядит как инициированное соединение, но не доведённое до завершения.

FIN:
Сетевой трафик будет содержать пакеты с флагом FIN (что означает попытку завершить соединение), и в случае открытого порта сервер может не отправить ответа, а в случае закрытого порта отправит RST.

Xmas:
Пакет включает флаги FIN, URG и PUSH, что делает его необычным. Некоторые системы могут неправильно обрабатывать такие пакеты, что даёт возможность скрыть сканирование.
Трафик выглядит как странные пакеты с несколькими флагами одновременно.

UDP:
Пакеты отправляются через UDP без установления соединений. В отличие от TCP, на открытые порты серверы не отправляют подтверждений. Ответ может быть ICMP Destination Unreachable для закрытых портов.
Сетевой трафик будет включать запросы к UDP-портам.


Ответ сервера:

SYN Scan: Если порт открыт, сервер отправляет ответ SYN-ACK, если закрыт — RST.

FIN Scan: Если порт открыт, сервер может не ответить, а если закрыт — отправляет RST.

Xmas Scan: Если порт открыт, сервер может не отправить ответа, если закрыт — отправляет RST.

UDP Scan: Если порт открыт, ответа может не быть, если порт закрыт — сервер отправит ICMP сообщение о недоступности.

`При необходимости прикрепитe сюда скриншоты

![answer2](https://github.com/Vasiliy-Ser/homework_12.1/blob/3d5fc3940e284d3065ef26abd576c0e2d9b45b61/img/12.21.png)

![answer3](https://github.com/Vasiliy-Ser/homework_12.1/blob/3d5fc3940e284d3065ef26abd576c0e2d9b45b61/img/12.22.png)

![answer4](https://github.com/Vasiliy-Ser/homework_12.1/blob/3d5fc3940e284d3065ef26abd576c0e2d9b45b61/img/12.23.png)

![answer5](https://github.com/Vasiliy-Ser/homework_12.1/blob/3d5fc3940e284d3065ef26abd576c0e2d9b45b61/img/12.24.png)

