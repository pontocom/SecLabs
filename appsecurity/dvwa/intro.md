# Introduction 

This document aims to demonstrate a set of Web application security laboratories, based on a set of vulnerability analysis tools and on some PHP exploitation techniques. 

For these laboratories will be used an application purposely vulnerable as a way of demonstrating some of the techniques applied. This application is called [Damn Vulnerable Web Application (DVWA)](https://github.com/digininja/DVWA). The DVWA is a Web application developed in PHP with a MySQL database, which is purposely insecure.

To run these labs the following requirements are needed:
- Kali Linux, in particular the following tools (although others can also be used):
  - Nikto
  - W3af (note: this tool is no longer supported by Kali Linux)
  - Skipfish
  - Burp Suite
  - OWASP ZAP
  - Metasploit
  - sqlmap
  - Hydra
- Apache Web Server
- Database server MySQL.

The goal of these laboratories is even to demonstrate how a Web application can be exploited, and some of these attacks only result in an insecure application, with few security concerns, either in terms of development or configuration.

It is important to emphasize that it is not recommended to apply some of these techniques in real Web applications (spread across the Web), and students should apply them only to laboratories developed for this purpose.
