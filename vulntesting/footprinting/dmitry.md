# dmitry <!-- omit in toc -->


[dmitry](https://github.com/jaygreig86/dmitry) (Deepmagic Information Gathering Tool) is also a tool to collect as much as possible information about a particular target. The application is considered a tool to assist in information gathering when information is required quickly by removing the need to enter multiple commands and the timely process of searching through data from multiple sources.

Look at the tool and the help to discover its multiple options:

    dmitry -h

These options include:

    Deepmagic Information Gathering Tool
    "There be some deep magic going on"

    dmitry: invalid option -- 'h'
    Usage: dmitry [-winsepfb] [-t 0-9] [-o %host.txt] host
    -o     Save output to %host.txt or to file specified by -o file
    -i     Perform a whois lookup on the IP address of a host
    -w     Perform a whois lookup on the domain name of a host
    -n     Retrieve Netcraft.com information on a host
    -s     Perform a search for possible subdomains
    -e     Perform a search for possible email addresses
    -p     Perform a TCP port scan on a host
    * -f     Perform a TCP port scan on a host showing output reporting filtered ports
    * -b     Read in the banner received from the scanned port
    * -t 0-9 Set the TTL in seconds when scanning a TCP port ( Default 2 )
    *Requires the -p flagged to be passed

Now that you know the options try to obtain information about a target. Here is an example on how to do it.

    dmitry -i -w -n -s -e some-target.com

In this example we will try to:
    
* perform a whois lookup of the target
* perform a whois lookup of the domain name
* obtain netcraft information
* look for subdomains
* look for email addresses

After getting the results from the tool, try to interpret them.
