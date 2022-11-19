# theHarvester <!-- omit in toc -->

[theHarvester](https://github.com/laramies/theHarvester) is a tool that cen be used to collect information about multiple sources about a target. The tool gathers names, emails, IPs, subdomains, and URLs by using
multiple public resources.

You may look at the help of the tool to check those public resources.

    theHarvester -h

You can look at several options.

    *******************************************************************
    *  _   _                                            _             *
    * | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
    * | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
    * | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
    *  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
    *                                                                 *
    * theHarvester 4.2.0                                              *
    * Coded by Christian Martorella                                   *
    * Edge-Security Research                                          *
    * cmartorella@edge-security.com                                   *
    *                                                                 *
    *******************************************************************
    usage: theHarvester [-h] -d DOMAIN [-l LIMIT] [-S START] [-p] [-s] [--screenshot SCREENSHOT] [-v] [-e DNS_SERVER] [-r] [-n] [-c] [-f FILENAME] [-b SOURCE]

    theHarvester is used to gather open source intelligence (OSINT) on a company or domain.

    options:
    -h, --help            show this help message and exit
    -d DOMAIN, --domain DOMAIN
                            Company name or domain to search.
    -l LIMIT, --limit LIMIT
                            Limit the number of search results, default=500.
    -S START, --start START
                            Start with result number X, default=0.
    -p, --proxies         Use proxies for requests, enter proxies in proxies.yaml.
    -s, --shodan          Use Shodan to query discovered hosts.
    --screenshot SCREENSHOT
                            Take screenshots of resolved domains specify output directory: --screenshot output_directory
    -v, --virtual-host    Verify host name via DNS resolution and search for virtual hosts.
    -e DNS_SERVER, --dns-server DNS_SERVER
                            DNS server to use for lookup.
    -r, --take-over       Check for takeovers.
    -n, --dns-lookup      Enable DNS server lookup, default False.
    -c, --dns-brute       Perform a DNS brute force on the domain.
    -f FILENAME, --filename FILENAME
                            Save the results to an XML and JSON file.
    -b SOURCE, --source SOURCE
                            anubis, baidu, bevigil, binaryedge, bing, bingapi, bufferoverun, censys, certspotter, crtsh, dnsdumpster, duckduckgo, fullhunt, github-code,
                            hackertarget, hunter, intelx, omnisint, otx, pentesttools, projectdiscovery, qwant, rapiddns, rocketreach, securityTrails, sublist3r, threatcrowd,
                            threatminer, urlscan, virustotal, yahoo, zoomeye

As you can see, the **list of sources** of information include the following:

    anubis, baidu, bevigil, binaryedge, bing, bingapi, bufferoverun, censys, certspotter, crtsh, dnsdumpster, duckduckgo, fullhunt, github-code, hackertarget, hunter, intelx, omnisint, otx, pentesttools, projectdiscovery, qwant, rapiddns, rocketreach, securityTrails, sublist3r, threatcrowd, threatminer, urlscan, virustotal, yahoo, zoomeye

So lets go and try to **collect information from a target**.

    theHarvester -d your-target.com -l 500 -b all

And try to interpret the possible results obtained.