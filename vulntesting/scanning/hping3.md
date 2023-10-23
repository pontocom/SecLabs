# Hping3

**[hping3](https://linux.die.net/man/8/hping3)** is a network tool able to send custom **ICMP/UDP/TCP** packets and to display target replies like ping does with ICMP replies. It handles fragmentation and arbitrary packet body and size, and can be used to transfer files under supported protocols. Using **hping3**, you can test firewall rules, perform (spoofed) port scanning, test network performance using different protocols, do path MTU discovery, perform traceroute-like actions under different protocols, fingerprint remote operating systems, audit TCP/IP stacks, etc. hping3 is scriptable using the Tcl language.

![](../assets/hping-logo.png)

## Basic Usage

The best way to learn how to use **hping3** is to look at the help of the tool.

`hping3 -h`

That displays all the different options.

    usage: hping3 host [options]
    -h  --help      show this help
    -v  --version   show version
    -c  --count     packet count
    -i  --interval  wait (uX for X microseconds, for example -i u1000)
        --fast      alias for -i u10000 (10 packets for second)
        --faster    alias for -i u1000 (100 packets for second)
        --flood      sent packets as fast as possible. Don't show replies.
    -n  --numeric   numeric output
    -q  --quiet     quiet
    -I  --interface interface name (otherwise default routing interface)
    -V  --verbose   verbose mode
    -D  --debug     debugging info
    -z  --bind      bind ctrl+z to ttl           (default to dst port)
    -Z  --unbind    unbind ctrl+z
        --beep      beep for every matching packet received
    Mode
    default mode     TCP
    -0  --rawip      RAW IP mode
    -1  --icmp       ICMP mode
    -2  --udp        UDP mode
    -8  --scan       SCAN mode.
                    Example: hping --scan 1-30,70-90 -S www.target.host
    -9  --listen     listen mode
    IP
    -a  --spoof      spoof source address
    --rand-dest      random destionation address mode. see the man.
    --rand-source    random source address mode. see the man.
    -t  --ttl        ttl (default 64)
    -N  --id         id (default random)
    -W  --winid      use win* id byte ordering
    -r  --rel        relativize id field          (to estimate host traffic)
    -f  --frag       split packets in more frag.  (may pass weak acl)
    -x  --morefrag   set more fragments flag
    -y  --dontfrag   set don't fragment flag
    -g  --fragoff    set the fragment offset
    -m  --mtu        set virtual mtu, implies --frag if packet size > mtu
    -o  --tos        type of service (default 0x00), try --tos help
    -G  --rroute     includes RECORD_ROUTE option and display the route buffer
    --lsrr           loose source routing and record route
    --ssrr           strict source routing and record route
    -H  --ipproto    set the IP protocol field, only in RAW IP mode
    ICMP
    -C  --icmptype   icmp type (default echo request)
    -K  --icmpcode   icmp code (default 0)
        --force-icmp send all icmp types (default send only supported types)
        --icmp-gw    set gateway address for ICMP redirect (default 0.0.0.0)
        --icmp-ts    Alias for --icmp --icmptype 13 (ICMP timestamp)
        --icmp-addr  Alias for --icmp --icmptype 17 (ICMP address subnet mask)
        --icmp-help  display help for others icmp options
    UDP/TCP
    -s  --baseport   base source port             (default random)
    -p  --destport   [+][+]<port> destination port(default 0) ctrl+z inc/dec
    -k  --keep       keep still source port
    -w  --win        winsize (default 64)
    -O  --tcpoff     set fake tcp data offset     (instead of tcphdrlen / 4)
    -Q  --seqnum     shows only tcp sequence number
    -b  --badcksum   (try to) send packets with a bad IP checksum
                    many systems will fix the IP checksum sending the packet
                    so you'll get bad UDP/TCP checksum instead.
    -M  --setseq     set TCP sequence number
    -L  --setack     set TCP ack
    -F  --fin        set FIN flag
    -S  --syn        set SYN flag
    -R  --rst        set RST flag
    -P  --push       set PUSH flag
    -A  --ack        set ACK flag
    -U  --urg        set URG flag
    -X  --xmas       set X unused flag (0x40)
    -Y  --ymas       set Y unused flag (0x80)
    --tcpexitcode    use last tcp->th_flags as exit code
    --tcp-mss        enable the TCP MSS option with the given value
    --tcp-timestamp  enable the TCP timestamp option to guess the HZ/uptime
    Common
    -d  --data       data size                    (default is 0)
    -E  --file       data from file
    -e  --sign       add 'signature'
    -j  --dump       dump packets in hex
    -J  --print      dump printable characters
    -B  --safe       enable 'safe' protocol
    -u  --end        tell you when --file reached EOF and prevent rewind
    -T  --traceroute traceroute mode              (implies --bind and --ttl 1)
    --tr-stop        Exit when receive the first not ICMP in traceroute mode
    --tr-keep-ttl    Keep the source TTL fixed, useful to monitor just one hop
    --tr-no-rtt       Don't calculate/show RTT information in traceroute mode
    ARS packet description (new, unstable)
    --apd-send       Send the packet described with APD (see docs/APD.txt)

As you can see, you have plenty of different options for using **hping3**.

## Basic ping usage with HPing3

In order to do a basic ping with **hping3**, you need to do (most of these **hping3** commands require sudo permissions):

`sudo hping3 192.168.8.142`

Resulting in:

```
HPING 192.168.8.142 (eth0 192.168.8.142): NO FLAGS are set, 40 headers + 0 data bytes
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=0 win=0 rtt=3.8 ms
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=1 win=0 rtt=3.5 ms
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=2 win=0 rtt=11.2 ms
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=3 win=0 rtt=3.0 ms
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=4 win=0 rtt=6.6 ms
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=5 win=0 rtt=6.2 ms
^C
--- 192.168.8.142 hping statistic ---
6 packets transmitted, 6 packets received, 0% packet loss
round-trip min/avg/max = 3.0/5.7/11.2 ms
```

We can also limit the number of ping requests:

`sudo hping3 192.168.8.142 -c 1`

This simply send one ping.

```
HPING 192.168.8.142 (eth0 192.168.8.142): NO FLAGS are set, 40 headers + 0 data bytes
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=0 flags=RA seq=0 win=0 rtt=11.8 ms

--- 192.168.8.142 hping statistic ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 11.8/11.8/11.8 ms
```

We can also ping just a specific port number. For instance lets ping just port 80.

`sudo hping3 -S 192.168.8.142 -p 80 -c 1`

```
HPING 192.168.8.142 (eth0 192.168.8.142): S set, 40 headers + 0 data bytes
len=46 ip=192.168.8.142 ttl=64 DF id=0 sport=80 flags=SA seq=0 win=5840 rtt=3.7 ms

--- 192.168.8.142 hping statistic ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 3.7/3.7/3.7 ms
```

# Hping3 Scanning

**hping3** can be also used to perform several types of scanning on hosts. We can use it to check which are the ports and services status on a given target.

`sudo hping3 --scan 21,22,80 -S 192.168.8.142`

In this case we are trying to check the status of ports 21, 22 and 80.

```
Scanning 192.168.8.142 (192.168.8.142), port 21,22,80
3 ports to scan, use -V to see all the replies
+----+-----------+---------+---+-----+-----+-----+
|port| serv name |  flags  |ttl| id  | win | len |
+----+-----------+---------+---+-----+-----+-----+
   21 ftp        : .S..A...  64     0  5840    46
   22 ssh        : .S..A...  64     0  5840    46
   80 http       : .S..A...  64     0  5840    46
All replies received. Done.
Not responding ports:
```

We can also scan for given port ranges (10-1000):

`sudo hping3 --scan 10-1000 -S 192.168.8.142`

```
Scanning 192.168.8.142 (192.168.8.142), port 10-1000
991 ports to scan, use -V to see all the replies
+----+-----------+---------+---+-----+-----+-----+
|port| serv name |  flags  |ttl| id  | win | len |
+----+-----------+---------+---+-----+-----+-----+
   21 ftp        : .S..A...  64     0  5840    46
   22 ssh        : .S..A...  64     0  5840    46
   23 telnet     : .S..A...  64     0  5840    46
   25 smtp       : .S..A...  64     0  5840    46
   53 domain     : .S..A...  64     0  5840    46
   80 http       : .S..A...  64     0  5840    46
  111 sunrpc     : .S..A...  64     0  5840    46
  139 netbios-ssn: .S..A...  64     0  5840    46
  445 microsoft-d: .S..A...  64     0  5840    46
  512 exec       : .S..A...  64     0  5840    46
  513 login      : .S..A...  64     0  5840    46
  514 shell      : .S..A...  64     0  5840    46
All replies received. Done.
Not responding ports:
```

It is also possible to check the uptime of a given service:

`sudo hping3 -p 80 -S 192.168.8.142 --tcp-timestamp`

```
HPING 192.168.8.142 (eth0 192.168.8.142): S set, 40 headers + 0 data bytes
len=56 ip=192.168.8.142 ttl=64 DF id=0 sport=80 flags=SA seq=0 win=5792 rtt=3.7 ms
  TCP timestamp: tcpts=7343280

len=56 ip=192.168.8.142 ttl=64 DF id=0 sport=80 flags=SA seq=1 win=5792 rtt=3.3 ms
  TCP timestamp: tcpts=7343380
  HZ seems hz=100
  System uptime seems: 0 days, 20 hours, 23 minutes, 53 seconds
```

It is even possible to do a SYN flood on a victim. Beware of this since it may cause a DoS. If you are using virtual machines, they might hang.

`sudo hping3 -p 80 -S 192.168.8.142 -a 192.168.8.254 --flood`

(in this case, we are flooding 192.168.8.142 with SYN requests on port 80, and we are changing the return IP address (spoofing) to 192.168.8.254)

If we try to use a web browser to access 192.268.8.142 websites, it would be impossible.

```
HPING 192.168.8.142 (eth0 192.168.8.142): S set, 40 headers + 0 data bytes
hping in flood mode, no replies will be shown
^C
--- 192.168.8.142 hping statistic ---
4209623 packets transmitted, 0 packets received, 100% packet loss
round-trip min/avg/max = 0.0/0.0/0.0 ms
```

# Further references

- [Hping3: Full tutorial from noob to pro](https://techyrick.com/hping3-full-tutorial-for-dummies-to-pro/)
- [DOS Flood With hping3](https://linuxhint.com/hping3/)
- [10 hping3 examples in Kali Linux a complete Guide for beginners](https://www.cyberpratibha.com/blog/scan-network-using-packet-assembleranalyzer-hping3-in-kali-linux/)
- [15+ hping3 command examples in Linux [Cheat Sheet]](https://www.golinuxcloud.com/hping3-command-in-linux/)