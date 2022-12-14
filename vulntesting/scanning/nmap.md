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

This will produce the list of hosts which are up on the network analysed.

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

`nmap` reports the open ports and the services that are running listening for connections on such ports.

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

### Scanning different protocols

`nmap` can also scan for different listening protocols on the hosts (TCP or UDP)