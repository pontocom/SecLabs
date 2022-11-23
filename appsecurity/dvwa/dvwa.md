# DVWA - Attacks and Tests 

Next some of the possible attacks on this application (DVWA) will be demonstrated. The possible attacks are the following:
- [Brute-Force](bruteforce.md)
- [Command Injection](commandinjection.md)
- [File inclusion](fileinclusion.md)
- [File upload](fileupload.md)
- [SQL Injection](sqli.md)
- [SQL Injection (Blind)](sqliblind.md)
- [XSS (Reflected)](xssreflected.md)
- [XSS (Stored)](xssstored.md).

In some of these attacks it is necessary to take into account that the web application was developed in PHP, however most of these attacks also work on other development technologies (Java, Ruby, .Net, Perl, among others).

All the vulnerabilities that we are going to test and exploit on DVWA, are in "**low**" security mode. There's a great guide for DVWA exploiting that you might [find here](https://bughacking.com/dvwa-ultimate-guide-first-steps-and-walkthrough/), that complements the information in this lab, and even explain some approaches to solving challenges in different security modes.