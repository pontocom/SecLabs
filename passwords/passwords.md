# Attacks on Password-based Systems  <!-- omit in toc -->

## Table of Contents  <!-- omit in toc -->

- [Introduction](#introduction)
- [Understanding Passwords](#understanding-passwords)
  - [Check your passwords](#check-your-passwords)
  - [Massive list of passwords](#massive-list-of-passwords)
  - [Checking for password robustness](#checking-for-password-robustness)
- [Attacks on Passwords](#attacks-on-passwords)
  - [Looking for password-based services](#looking-for-password-based-services)

## Introduction

This is lab where you can test some content related with the usage of passwords. You'll find some examples where you can test if your password is compromised, or how robust are your passwords.

In order to use this lab, it is recommended to use:

- a web browser
- [Kali Linux](https://www.kali.org/), with some tools installed
- [Metasploitable 2](https://sourceforge.net/projects/metasploitable/) VM (used as a target).

## Understanding Passwords

This part is useful for checking the security of passwords and to understand its robustness.

### Check your passwords

There have been some data leakages on the Internet that contain a massive amount of accounts, with the passwords of millions and millions of users. [Troy Hunt](https://www.troyhunt.com/), a security researcher, as created a web site called "[have I been pwned](https://haveibeenpwned.com/)" that allows any user to look for an email address or telephone number, that might be part of an existing data leak.

![haveibeenpwned](assets/picture01.png)

Using this web site try to look for the following:

- Look to see if some of your accounts have been compromised (also look at the details of those accounts);
- Look at the amazing [list of Pwned web sites](https://haveibeenpwned.com/PwnedWebsites). Found anything [interesting](https://haveibeenpwned.com/PwnedWebsites#TAPAirPortugal)?
- Look for information online about the most recent Facebook data breach (2021).

### Massive list of passwords

There are multiple sites that aggregate lots of passwords. These passwords can be used to conduct dictionary attacks, that test all the existing passwords to check if some of them works.

One of the most well know data breach that involved non-encrypted user accounts was the [Rockyou](https://techcrunch.com/2009/12/14/rockyou-hack-security-myspace-facebook-passwords/?guccounter=1) social application site, mainly developing widgets for Facebook. Rockyou sufered [a data breach](https://en.wikipedia.org/wiki/RockYou) that resulted in the exposure of 32 million user accounts.

- [Common Password List ( rockyou.txt )](https://www.kaggle.com/datasets/wjburns/common-password-list-rockyoutxt)
- [Direct download](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt 
) of the Rockyou passwords
- [Seclists](https://github.com/danielmiessler/SecLists)

### Checking for password robustness

One of the most important measures in terms of security for a password is its robustness. One way to determine the password robustness is through the measure of the [password entropy](https://www.okta.com/identity-101/password-entropy/). Password entropy predicts how difficult a given password would be to crack through guessing, brute force or dictionary attacks or other common methods. Entropy is measured in bits.

Just for checking the entropy of the passwords lets do the following:

1. Visit the web site of GeneratePasswords and look and the [password entropy calculation calculation formula](https://generatepasswords.org/how-to-calculate-entropy/);
2. Also look at [why the password strength meters are not that great](https://generatepasswords.org/why-password-strength-meters-are-not-so-great-after-all/) (those you find on most websites);
3. Check the entropy of the different types of passwords using a [password strength calculator](http://www.bee-man.us/computer/password_strength.html);
4. Check on [EFF Dice-Generate Passphrases](https://www.eff.org/dice). Look at the [wordlist dictionary](https://www.eff.org/files/2016/07/18/eff_large_wordlist.txt). Try the proposed process to create a great passphrase;
5. Also look at the [Diceware Passphrase web site](https://theworld.com/~reinhold/diceware.html);
6. Finally try to create a passwords/passphrases and [check its strength](https://bitwarden.com/password-strength/).

## Attacks on Passwords

There are two generic ways to attack passwords: either through a **dictionary attack** or a **brute-force attack**. There are other techniques, but these are the most common ones.

Let us simulate a situation in which we have an attacker that is going to try to exploit a victim. For the attacker, [Kali Linux](https://www.kali.org/) will be used. For the victim, we will use the [Metasploitable 2](https://sourceforge.net/projects/metasploitable/).

Lets assume that the victim has the following email address: **192.168.8.148**.

### Looking for password-based services 

Next we are going to analyze the system either using "`nmap`" or "`massscan`". Let's use `nmap` first:

    nmap 192.168.8.142

And obtain the following results:

    Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-04 23:56 WET
    Nmap scan report for 192.168.8.142
    Host is up (0.0046s latency).
    Not shown: 977 closed tcp ports (conn-refused)
    PORT     STATE SERVICE
    21/tcp   open  ftp
    22/tcp   open  ssh
    23/tcp   open  telnet
    25/tcp   open  smtp
    53/tcp   open  domain
    80/tcp   open  http
    111/tcp  open  rpcbind
    139/tcp  open  netbios-ssn
    445/tcp  open  microsoft-ds
    512/tcp  open  exec
    513/tcp  open  login
    514/tcp  open  shell
    1099/tcp open  rmiregistry
    1524/tcp open  ingreslock
    2049/tcp open  nfs
    2121/tcp open  ccproxy-ftp
    3306/tcp open  mysql
    5432/tcp open  postgresql
    5900/tcp open  vnc
    6000/tcp open  X11
    6667/tcp open  irc
    8009/tcp open  ajp13
    8180/tcp open  unknown

    Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds

And now let's use the `massscan` tool (it requires it to run as `sudo`):

    sudo masscan 192.168.8.142 --top-ports

Resulting in:

    Starting masscan 1.3.2 (http://bit.ly/14GZzcT) at 2022-11-05 00:00:46 GMT
    Initiating SYN Stealth Scan
    Scanning 1 hosts [1000 ports/host]
    Discovered open port 21/tcp on 192.168.8.142                                   
    Discovered open port 1524/tcp on 192.168.8.142                                 
    Discovered open port 80/tcp on 192.168.8.142                                   
    Discovered open port 1099/tcp on 192.168.8.142                                 
    Discovered open port 8180/tcp on 192.168.8.142                                 
    Discovered open port 111/tcp on 192.168.8.142                                  
    Discovered open port 22/tcp on 192.168.8.142                                   
    Discovered open port 8009/tcp on 192.168.8.142                                 
    Discovered open port 5900/tcp on 192.168.8.142                                 
    Discovered open port 2049/tcp on 192.168.8.142                                 
    Discovered open port 53/tcp on 192.168.8.142                                   
    Discovered open port 139/tcp on 192.168.8.142                                  
    Discovered open port 514/tcp on 192.168.8.142                                  
    Discovered open port 6667/tcp on 192.168.8.142                                 
    Discovered open port 6000/tcp on 192.168.8.142                                 
    Discovered open port 512/tcp on 192.168.8.142                                  
    Discovered open port 25/tcp on 192.168.8.142                                   
    Discovered open port 5432/tcp on 192.168.8.142                                 
    Discovered open port 2121/tcp on 192.168.8.142                                 
    Discovered open port 513/tcp on 192.168.8.142                                  
    Discovered open port 445/tcp on 192.168.8.142                                  
    Discovered open port 3306/tcp on 192.168.8.142                                 
    Discovered open port 23/tcp on 192.168.8.142 

So it was possible to conclude that there are plenty of services open on the machine. To this point we can understand the exposition degree of the victim. It is possible to understand there are services such as **ftp**, **ssh**, and **http** which are running on the machine.
