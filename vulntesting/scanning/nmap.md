# Nmap

[Nmap](https://nmap.org/) is a very versatile tool to do networks and system scanning. 

From the Nmap web page, they define Nmap as being "*an open source tool for network exploration and security auditing. It was designed to rapidly scan large networks, although it works fine against single hosts. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. While Nmap is commonly used for security audits, many systems and network administrators find it useful for routine tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime*.".

## About Nmap

The best way to know about Nmap possibilities is to look at the help page of Nmap.

    nmap -h

You can check all the possible options:

    Nmap 7.93 ( https://nmap.org )
    Usage: nmap [Scan Type(s)] [Options] {target specification}
    TARGET SPECIFICATION:
    Can pass hostnames, IP addresses, networks, etc.
    Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
    -iL <inputfilename>: Input from list of hosts/networks
    -iR <num hosts>: Choose random targets
    --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
    --excludefile <exclude_file>: Exclude list from file
    HOST DISCOVERY:
    -sL: List Scan - simply list targets to scan
    -sn: Ping Scan - disable port scan
    -Pn: Treat all hosts as online -- skip host discovery
    -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
    -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
    -PO[protocol list]: IP Protocol Ping
    -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
    --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
    --system-dns: Use OS's DNS resolver
    --traceroute: Trace hop path to each host

From this you can see what Nmap can actually do.

Another important source of information is either the [Nmap book](https://nmap.org/book/) and the [reference guide online](https://nmap.org/book/man.html).

## Basic scanning

Imagine that you want to know which are the machines that are active on your network. In order to do that with `nmap` you should do the following:

`nmap -sn 192.168.8.0/24` (using [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) notation)

`nmap -sn 192.168.8.1-254` (scans all IP addresses from `192.168.8.1` until `192.168.8.254`)

This will produce the list of hosts which are up on the network analysed. This is a simple `ping scan`, without identification of the open ports.

    Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-14 18:13 WET
    Nmap scan report for RT-AX86U-80F8 (192.168.50.1)
    Host is up (0.010s latency).
    Nmap scan report for RE505X (192.168.50.10)
    Host is up (0.13s latency).
    Nmap scan report for 192.168.50.106
    Host is up (0.036s latency).
    Nmap scan report for home (192.168.50.231)
    Host is up (0.025s latency).
    Nmap scan report for Carloss-MBP (192.168.50.248)
    Host is up (0.0072s latency).
    Nmap done: 254 IP addresses (5 hosts up) scanned in 8.68 seconds

## Scanning for open ports

The basic operation of `nmap` allows you to discover information about a connected system. The most basic way to use `nmap` for this is simply to do the following:

`nmap 192.168.8.142`

This will result in the following:

    Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-14 18:17 WET
    Nmap scan report for 192.168.8.142
    Host is up (0.0022s latency).
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
    
    Nmap done: 1 IP address (1 host up) scanned in 0.14 seconds

`nmap` reports the open ports and the services that are running listening for connections on such ports. `nmap` can indicate the following [6 different possible port states](https://geek-university.com/port-states/):
* **open** – indicates that an application is listening for connections on the port. The primary goal of port scanning is to find these.
* **closed** – indicates that the probes were received but but there is no application listening on the port.
* **filtered** – indicates that the probes were not received and the state could not be established.
* **unfiltered** – indicates that the probes were received but a state could not be established. In other words, a port is accessible, but Nmap is unable to determine whether it is open or closed.
* **open/filtered**– indicates that the port was filtered or open but `nmap` couldn’t establish the state.
* **closed/filtered** – indicates that Nmap is unable to determine whether a port is closed or filtered.

### Specify ports

It is possible to specify the ports that we want nmap tp analyse.

`nmap -p 80 192.168.8.142` (analyses only the port 80)

`nmap -p 80,443 192.168.8.142` (analyses ports 80 and 443)

`nmap -p- 192.168.8.142` (analyses all possible ports - 65536 ports)

`nmap --top-ports 10 192.168.8.142` (analyses the top 10 most important ports)

Results in the following:

    PORT     STATE  SERVICE
    21/tcp   open   ftp
    22/tcp   open   ssh
    23/tcp   open   telnet
    25/tcp   open   smtp
    80/tcp   open   http
    110/tcp  closed pop3
    139/tcp  open   netbios-ssn
    443/tcp  closed https
    445/tcp  open   microsoft-ds
    3389/tcp closed ms-wbt-server

As you can see, some are reported as **open** while others are **closed**.

### Different scanning techniques

`nmap` can also perform different scanning techniques. Here are some examples:

`nmap -sU 192.168.8.142` (scans host for UDP services)

This will produce the something similar to this:

    PORT     STATE         SERVICE
    53/udp   open          domain
    68/udp   open|filtered dhcpc
    69/udp   open|filtered tftp
    111/udp  open          rpcbind
    137/udp  open          netbios-ns
    138/udp  open|filtered netbios-dgm
    2049/udp open          nfs

`nmap -sS 192.168.8.142` (makes a TCP SYN scan - this as to do with the [TCP 3-way handshake protocol](https://www.geeksforgeeks.org/tcp-3-way-handshake-process/), only sends the SYN packet and never establishes the connection)

`nmap -sT 192.168.8.142` (does the TCP connection, executing the complete [TCP 3-way handshake](https://www.geeksforgeeks.org/tcp-3-way-handshake-process/))

Using different scanning techniques may produce different results and increase the chances of information collection. In [this page you may find more information](https://www.digitalocean.com/community/tutorials/nmap-switches-scan-types) about these different scanning techniques.

### Service identification

`nmap` can also be used to identify the different services that are running and listening on a given host. In order to do that we may use the following:

`nmap -sV 192.168.8.142`

This will result in something like this:

    Nmap scan report for 192.168.8.142
    Host is up (0.0019s latency).
    Not shown: 977 closed tcp ports (conn-refused)
    PORT     STATE SERVICE     VERSION
    21/tcp   open  ftp         vsftpd 2.3.4
    22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
    23/tcp   open  telnet      Linux telnetd
    25/tcp   open  smtp        Postfix smtpd
    53/tcp   open  domain      ISC BIND 9.4.2
    80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
    111/tcp  open  rpcbind     2 (RPC #100000)
    139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
    445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
    512/tcp  open  exec        netkit-rsh rexecd
    513/tcp  open  login
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
    Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 11.58 seconds

From this it is possible to identify:
* port: the port number
* state: the current state of the port
* service: the service name
* version: the service details, including the version number.

### Identifying the operating system

`nmap` is also capable of identifying the operative system on the target machine. 

`nmap -O 192.168.8.142` (tries to identify the OS on the target)

Produces the following results:

    Device type: general purpose
    Running: Linux 2.6.X
    OS CPE: cpe:/o:linux:linux_kernel:2.6
    OS details: Linux 2.6.9 - 2.6.33

## Running scripts with nmap

Another functionality of nmap is the possibility of executing scripts on a target. nmap is composed of hundred of scripts, and through the [NSE - Nmap Scripting Engine](https://nmap.org/book/man-nse.html) - is capable of executing several scripts.

Usually, you may find those scripts on the following directory:

`/usr/share/nmap/scripts`

These scripts are organized according to a [set of categories](https://nmap.org/book/nse-usage.html#nse-categories):
* auth
* broadcast
* default
* discovery
* dos
* exploit
* external
* fuzzer
* intrusive
* malware
* safe
* version
* vuln

We may select to run a specific script on a target:

`nmap --script ftp-vsftpd-backdoor 192.168.8.142`

Or we may run all the scripts of a given category on a target:

`nmap --script vuln 192.168.8.142`

That results in the identification of multiple vulnerabilities on the target platform.

    Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-14 22:55 WET
    Pre-scan script results:
    | broadcast-avahi-dos:
    |   Discovered hosts:
    |     224.0.0.251
    |   After NULL UDP avahi packet DoS (CVE-2011-1002).
    |_  Hosts are all up (not vulnerable).
    Nmap scan report for 192.168.8.142
    Host is up (0.0020s latency).
    Not shown: 977 closed tcp ports (conn-refused)
    PORT     STATE SERVICE
    21/tcp   open  ftp
    | ftp-vsftpd-backdoor:
    |   VULNERABLE:
    |   vsFTPd version 2.3.4 backdoor
    |     State: VULNERABLE (Exploitable)
    |     IDs:  CVE:CVE-2011-2523  BID:48539
    |       vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.
    |     Disclosure date: 2011-07-03
    |     Exploit results:
    |       Shell command: id
    |       Results: uid=0(root) gid=0(root)
    |     References:
    |       https://www.securityfocus.com/bid/48539
    |       http://scarybeastsecurity.blogspot.com/2011/07/alert-vsftpd-download-backdoored.html
    |       https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/ftp/vsftpd_234_backdoor.rb
    |_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2523
    22/tcp   open  ssh
    23/tcp   open  telnet
    25/tcp   open  smtp
    | ssl-dh-params:
    |   VULNERABLE:
    |   Anonymous Diffie-Hellman Key Exchange MitM Vulnerability
    |     State: VULNERABLE
    |       Transport Layer Security (TLS) services that use anonymous
    |       Diffie-Hellman key exchange only provide protection against passive
    |       eavesdropping, and are vulnerable to active man-in-the-middle attacks
    |       which could completely compromise the confidentiality and integrity
    |       of any data exchanged over the resulting session.
    |     Check results:
    |       ANONYMOUS DH GROUP 1
    |             Cipher Suite: TLS_DH_anon_WITH_RC4_128_MD5
    |             Modulus Type: Safe prime
    |             Modulus Source: postfix builtin
    |             Modulus Length: 1024
    |             Generator Length: 8
    |             Public Key Length: 1024
    |     References:
    |       https://www.ietf.org/rfc/rfc2246.txt
    |   
    |   Transport Layer Security (TLS) Protocol DHE_EXPORT Ciphers Downgrade MitM (Logjam)
    |     State: VULNERABLE
    |     IDs:  CVE:CVE-2015-4000  BID:74733
    |       The Transport Layer Security (TLS) protocol contains a flaw that is
    |       triggered when handling Diffie-Hellman key exchanges defined with
    |       the DHE_EXPORT cipher. This may allow a man-in-the-middle attacker
    |       to downgrade the security of a TLS session to 512-bit export-grade
    |       cryptography, which is significantly weaker, allowing the attacker
    |       to more easily break the encryption and monitor or tamper with
    |       the encrypted stream.
    |     Disclosure date: 2015-5-19
    |     Check results:
    |       EXPORT-GRADE DH GROUP 1
    |             Cipher Suite: TLS_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA
    |             Modulus Type: Safe prime
    |             Modulus Source: Unknown/Custom-generated
    |             Modulus Length: 512
    |             Generator Length: 8
    |             Public Key Length: 512
    |     References:
    |       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4000
    |       https://www.securityfocus.com/bid/74733
    |       https://weakdh.org
    |   
    |   Diffie-Hellman Key Exchange Insufficient Group Strength
    |     State: VULNERABLE
    |       Transport Layer Security (TLS) services that use Diffie-Hellman groups
    |       of insufficient strength, especially those using one of a few commonly
    |       shared groups, may be susceptible to passive eavesdropping attacks.
    |     Check results:
    |       WEAK DH GROUP 1
    |             Cipher Suite: TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA
    |             Modulus Type: Safe prime
    |             Modulus Source: postfix builtin
    |             Modulus Length: 1024
    |             Generator Length: 8
    |             Public Key Length: 1024
    |     References:
    |_      https://weakdh.org
    | ssl-poodle:
    |   VULNERABLE:
    |   SSL POODLE information leak
    |     State: VULNERABLE
    |     IDs:  CVE:CVE-2014-3566  BID:70574
    |           The SSL protocol 3.0, as used in OpenSSL through 1.0.1i and other
    |           products, uses nondeterministic CBC padding, which makes it easier
    |           for man-in-the-middle attackers to obtain cleartext data via a
    |           padding-oracle attack, aka the "POODLE" issue.
    |     Disclosure date: 2014-10-14
    |     Check results:
    |       TLS_RSA_WITH_AES_128_CBC_SHA
    |     References:
    |       https://www.imperialviolet.org/2014/10/14/poodle.html
    |       https://www.securityfocus.com/bid/70574
    |       https://www.openssl.org/~bodo/ssl-poodle.pdf
    |_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566
    |_sslv2-drown: ERROR: Script execution failed (use -d to debug)
    | smtp-vuln-cve2010-4344:
    |_  The SMTP server is not Exim: NOT VULNERABLE

## Additional Information

[Nmap Cheat Sheet](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/)

[Nmap Cheat Sheet 2022: All the Commands, Flags & Switches](https://www.stationx.net/nmap-cheat-sheet/)

[Bypassing Firewall Rules](https://nmap.org/book/firewall-subversion.html)

