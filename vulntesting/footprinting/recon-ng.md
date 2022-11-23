# Recon-ng 

Another important tool/framework for reconnaissance is [Recon-ng](https://github.com/lanmaster53/recon-ng). Recon-ng is a full-featured reconnaissance framework designed with the goal of providing a powerful environment to conduct open source web-based reconnaissance quickly and thoroughly.

When invoking `recon-ng` you are presented with an welcome screen, where you can do a set of commands. First, lets look at the help of the tool, by entering the command "**help**".

    [*] Version check disabled.

        _/_/_/    _/_/_/_/    _/_/_/    _/_/_/    _/      _/            _/      _/    _/_/_/
    _/    _/  _/        _/        _/      _/  _/_/    _/            _/_/    _/  _/       
    _/_/_/    _/_/_/    _/        _/      _/  _/  _/  _/  _/_/_/_/  _/  _/  _/  _/  _/_/_/
    _/    _/  _/        _/        _/      _/  _/    _/_/            _/    _/_/  _/      _/ 
    _/    _/  _/_/_/_/    _/_/_/    _/_/_/    _/      _/            _/      _/    _/_/_/    


                                            /\
                                            / \\ /\
        Sponsored by...               /\  /\/  \\V  \/\
                                    / \\/ // \\\\\ \\ \/\
                                    // // BLACK HILLS \/ \\
                                www.blackhillsinfosec.com

                    ____   ____   ____   ____ _____ _  ____   ____  ____
                    |____] | ___/ |____| |       |   | |____  |____ |
                    |      |   \_ |    | |____   |   |  ____| |____ |____
                                    www.practisec.com

                        [recon-ng v5.1.2, Tim Tomes (@lanmaster53)]                       

    [1] Recon modules

    [recon-ng][default] > help

    Commands (type [help|?] <topic>):
    ---------------------------------
    back            Exits the current context
    dashboard       Displays a summary of activity
    db              Interfaces with the workspace's database
    exit            Exits the framework
    help            Displays this menu
    index           Creates a module index (dev only)
    keys            Manages third party resource credentials
    marketplace     Interfaces with the module marketplace
    modules         Interfaces with installed modules
    options         Manages the current context options
    pdb             Starts a Python Debugger session (dev only)
    script          Records and executes command scripts
    shell           Executes shell commands
    show            Shows various framework items
    snapshots       Manages workspace snapshots
    spool           Spools output to a file
    workspaces      Manages workspaces

    [recon-ng][default] > 


We can create a **new workspace** for a reconnaissance project.

    workspaces create PROJECTNAME

Next, we can **add domains** to test in our project workspace.

    db insert domains

And **enter** the domain name.

    [recon-ng][PROJECTNAME] > db insert domains
    domain (TEXT): mydomain.com
    notes (TEXT): 
    [*] 1 rows affected.

We can **list the domains** in the database.

show domains

And we got.

    [recon-ng][PROJECTNAME] > show domains

    +---------------------------------------------+
    | rowid |    domain    | notes |    module    |
    +---------------------------------------------+
    | 1     | mydomain.com |       | user_defined |
    +---------------------------------------------+

    [*] 1 rows returned


Now, we need to select the modules from `recon-ng`. There are plenty of available modules. The [list is here](https://github.com/lanmaster53/recon-ng-marketplace).

Using the `marketplace` command, it is possible to check available modules.

    marketplace search

This produces a list.

    +---------------------------------------------------------------------------------------------------+
    |                        Path                        | Version |     Status    |  Updated   | D | K |
    +---------------------------------------------------------------------------------------------------+
    | discovery/info_disclosure/cache_snoop              | 1.1     | not installed | 2020-10-13 |   |   |
    | discovery/info_disclosure/interesting_files        | 1.2     | not installed | 2021-10-04 |   |   |
    | exploitation/injection/command_injector            | 1.0     | not installed | 2019-06-24 |   |   |
    | exploitation/injection/xpath_bruter                | 1.2     | not installed | 2019-10-08 |   |   |
    | import/csv_file                                    | 1.1     | not installed | 2019-08-09 |   |   |
    | import/list                                        | 1.1     | not installed | 2019-06-24 |   |   |
    | import/masscan                                     | 1.0     | not installed | 2020-04-07 |   |   |
    | import/nmap                                        | 1.1     | not installed | 2020-10-06 |   |   |
    | recon/companies-contacts/bing_linkedin_cache       | 1.0     | not installed | 2019-06-24 |   | * |
    | recon/companies-contacts/censys_email_address      | 2.0     | not installed | 2021-05-11 | * | * |
    | recon/companies-contacts/pen                       | 1.1     | not installed | 2019-10-15 |   |   |
    | recon/companies-domains/censys_subdomains          | 2.0     | not installed | 2021-05-10 | * | * |
    | recon/companies-domains/pen                        | 1.1     | not installed | 2019-10-15 |   |   |
    | recon/companies-domains/viewdns_reverse_whois      | 1.1     | not installed | 2021-08-24 |   |   |
    | recon/companies-domains/whoxy_dns                  | 1.1     | not installed | 2020-06-17 |   | * |
    | recon/companies-hosts/censys_org                   | 2.0     | not installed | 2021-05-11 | * | * |
    | recon/companies-hosts/censys_tls_subjects          | 2.0     | not installed | 2021-05-11 | * | * |
    | recon/companies-multi/github_miner                 | 1.1     | not installed | 2020-05-15 |   | * |
    | recon/companies-multi/shodan_org                   | 1.1     | not installed | 2020-07-01 | * | * |
    | recon/companies-multi/whois_miner                  | 1.1     | not installed | 2019-10-15 |   |   |
    | recon/contacts-contacts/abc                        | 1.0     | not installed | 2019-10-11 | * |   |
    | recon/contacts-contacts/mailtester                 | 1.0     | not installed | 2019-06-24 |   |   |
    | recon/contacts-contacts/mangle                     | 1.0     | not installed | 2019-06-24 |   |   |


We can **search for a specific module** to use in `recon-ng`.

    marketplace search hackertarget

And we can see is the module exists and the information about it.

    [*] Searching module index for 'hackertarget'...

    +-----------------------------------------------------------------------------+
    |               Path               | Version |   Status  |  Updated   | D | K |
    +-----------------------------------------------------------------------------+
    | recon/domains-hosts/hackertarget | 1.1     | installed | 2020-05-17 |   |   |
    +-----------------------------------------------------------------------------+

    D = Has dependencies. See info for details.
    K = Requires keys. See info for details.

If the module exists, we may **install it**.

    marketplace install recon/domains-hosts/hackertarget

After the module is installed, we **can load it**.

    modules load recon/domains-hosts/hackertarget

If we need to **know which modules are installed**, we can use:

    modules search

And we get the list of modules.

    [recon-ng][PROJECTNAME] > modules search

    Recon
    -----
        recon/domains-hosts/hackertarget

After selecting anf loading a module, we can **run** it. We can discover which are the options of the module.

    info

And we receive all the information of the module.

    [recon-ng][PROJECTNAME][hackertarget] > info

        Name: HackerTarget Lookup
        Author: Michael Henriksen (@michenriksen)
    Version: 1.1

    Description:
    Uses the HackerTarget.com API to find host names. Updates the 'hosts' table with the results.

    Options:
    Name    Current Value  Required  Description
    ------  -------------  --------  -----------
    SOURCE  default        yes       source of input (see 'info' for details)

    Source Options:
    default        SELECT DISTINCT domain FROM domains WHERE domain IS NOT NULL
    <string>       string representing a single input
    <path>         path to a file containing a list of inputs
    query <sql>    database query returning one column of inputs

    [recon-ng][PROJECTNAME][hackertarget] >

From the information, we see that we need to set a "**SOURCE**" for the module. So lets do that.

    options set SOURCE mydomain.com

So, lets run the module and check the results.

    run

And lets look at the results.

    ---------
    MYDOMAIN.COM
    ---------
    [*] Country: None
    [*] Host: mydomain.com
    [*] Ip_Address: X.9.66.XX
    [*] Latitude: None
    [*] Longitude: None
    [*] Notes: None
    [*] Region: None
    [*] --------------------------------------------------
    [*] Country: None
    [*] Host: o7.ptr6980.mydomain.com
    [*] Ip_Address: X.72.144.XX
    [*] Latitude: None
    [*] Longitude: None
    [*] Notes: None
    [*] Region: None
    [*] --------------------------------------------------
    [*] Country: None
    [*] Host: vpn1.mydomain.com
    [*] Ip_Address: X.45.124.XX
    [*] Latitude: None
    [*] Longitude: None
    [*] Notes: None
    [*] Region: None
    [*] --------------------------------------------------
    [*] Country: None
    [*] Host: apacvpn1.mydomain.com
    [*] Ip_Address: X.244.131.XX
    [*] Latitude: None
    [*] Longitude: None
    [*] Notes: None
    [*] Region: None
    [*] --------------------------------------------------

## Gathering information for a person

You can also look for personal information that might be present on a web-site of an organization. There are specific `recon-ng` modules to do that.

So, first of all lets create a new workspace.

    workspaces create PROJECT-PERSONAL

Let us select and install the appropriate module.

        marketplace install recon/domains-contacts/whois_pocs

And load the module.

modules load recon/domains-contacts/whois_pocs

Check the options to run the module.

    info

The module options are:

    [recon-ng][PROJECT-PERSONAL][whois_pocs] > info

        Name: Whois POC Harvester
        Author: Tim Tomes (@lanmaster53)
    Version: 1.0

    Description:
    Uses the ARIN Whois RWS to harvest POC data from whois queries for the given domain. Updates the
    'contacts' table with the results.

    Options:
    Name    Current Value  Required  Description
    ------  -------------  --------  -----------
    SOURCE  default        yes       source of input (see 'info' for details)

    Source Options:
    default        SELECT DISTINCT domain FROM domains WHERE domain IS NOT NULL
    <string>       string representing a single input
    <path>         path to a file containing a list of inputs
    query <sql>    database query returning one column of inputs        

It is necessary to set the "**SOURCE**".

    options set SOURCE gmail.com

And run the module.

    [recon-ng][PROJECT-PERSONAL][whois_pocs] > run

    ---------
    GMAIL.COM
    ---------
    [*] URL: http://whois.arin.net/rest/pocs;domain=gmail.com
    [*] URL: http://whois.arin.net/rest/poc/XXXX
    [*] Country: United States
    [*] Email: XXXX@gmail.com
    [*] First_Name: XXXXX
    [*] Last_Name: XXXXX
    [*] Middle_Name: None
    [*] Notes: None
    [*] Phone: None
    [*] Region: Columbus, OH
    [*] Title: Whois contact
    [*] --------------------------------------------------
    [*] URL: http://whois.arin.net/rest/poc/XXXX
    [*] Country: Taiwan, Province Of China
    [*] Email: XXXX@gmail.com
    [*] First_Name: XXXX
    [*] Last_Name: XXXX
    [*] Middle_Name: None
    [*] Notes: None
    [*] Phone: None
    [*] Region: New Taipei City
    [*] Title: Whois contact

Explore the multiple options and modules of `recon-ng`.