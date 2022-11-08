# John the Ripper  <!-- omit in toc -->

## Table of Contents  <!-- omit in toc -->

- [John the Ripper (JtR)](#john-the-ripper-jtr)
  - [Using JtR to crack passwords on a Linux system](#using-jtr-to-crack-passwords-on-a-linux-system)
    - [Using a brute-force approach](#using-a-brute-force-approach)
    - [Using a dictionary approach](#using-a-dictionary-approach)
  - [Cracking MD5 password files](#cracking-md5-password-files)
  - [Cracking password protected files](#cracking-password-protected-files)
- [References/Additional information](#referencesadditional-information)


## John the Ripper (JtR)

[John the Ripper](https://github.com/openwall/john) (JtR) is [another tool](https://www.kali.org/tools/john/) that can be used to attack password-based systems.

![JtR logo](assets/picture04.png)

It is a tool best suited to crack a large number of passwords (based on different algorithms), using dictionary attacks and brute-force (offline attacks). In order to understand the options that this tool offers you should use following command and analyze them in detail:

    john --help

JtR is essentially used for attacks on **offline** files that contain some type of passwords. Imagine a scenario where you obtain a file with a set of passwords (which are protected in some way) and you want to find out (crack) the original passwords - this is a task for JtR.

This tool can take advantage of extra hardware in the machine, such as GPUs, to speed up the password discovery process.


### Using JtR to crack passwords on a Linux system

In this case, let us try to find out the passwords of users on a Linux system. The first we need to do (for demo purposes) is to get the passwords and save them to a file (we can do this on the Kali Linux system):

    sudo unshadow /etc/passwd /etc/shadow > allpasswords

This will save the passwords in a file (`allpasswords`). Lets open the file and check its content (if you can't get the file, [download it from here](files/allpasswords)).

    cat allpasswords

Format:

    user:$y$j9T$S1FDA0LVtwxjLBDnNm0NZ0$s5oO9av92JIXitmfoLWKh.mBj4gMkqUwCygc94OvgU/:1000:1000:User One,,,:/home/user:/usr/bin/zsh

The passwords on a Linux system are usually encrypted using an algorithm called [crypt](https://linux.die.net/man/3/crypt). Therefore this information is **important** for the JtR tool.

#### Using a brute-force approach

To do a brute-force attack we can simply do:

    john --format=crypt allpasswords

`--format` indicates the format of the passwords on the file (`allpasswords`)

As you may notice, this will take a **looooong time**! Look at the CPU consumption and check the ETA...

    Using default input encoding: UTF-8
    Loaded 1 password hash (crypt, generic crypt(3) [?/64])
    Cost 1 (algorithm [1:descrypt 2:md5crypt 3:sunmd5 4:bcrypt 5:sha256crypt 6:sha512crypt]) is 0 for all loaded hashes
    Cost 2 (algorithm specific iterations) is 1 for all loaded hashes
    Proceeding with single, rules:Single
    Press 'q' or Ctrl-C to abort, almost any other key for status
    0g 0:00:00:09 19.19% 1/3 (ETA: 11:16:13) 0g/s 100.8p/s 100.8c/s 100.8C/s Useronel..Uusery
    0g 0:00:00:23 38.12% 1/3 (ETA: 11:16:26) 0g/s 98.67p/s 98.67c/s 98.67C/s Muser..SUser
    0g 0:00:01:07 85.71% 1/3 (ETA: 11:16:45) 0g/s 92.22p/s 92.22c/s 92.22C/s Ouser11111..oneuser111111
    0g 0:00:01:14 94.22% 1/3 (ETA: 11:16:44) 0g/s 92.82p/s 92.82c/s 92.82C/s ouser2023..oneuser1964
    Session aborted


#### Using a dictionary approach

In order to save time, we may try to use a dictionary attack instead. In order to do that we need to do the following, using a dictionary (`passwords.txt`):

    john --format=crypt --wordlist=passwords.txt allpasswords

If the word list contains a password we obtain a result really fast.

    Using default input encoding: UTF-8
    Loaded 1 password hash (crypt, generic crypt(3) [?/64])
    Cost 1 (algorithm [1:descrypt 2:md5crypt 3:sunmd5 4:bcrypt 5:sha256crypt 6:sha512crypt]) is 0 for all loaded hashes
    Cost 2 (algorithm specific iterations) is 1 for all loaded hashes
    Press 'q' or Ctrl-C to abort, almost any other key for status
    Warning: Only 8 candidates left, minimum 96 needed for performance.
    password         (user)     
    1g 0:00:00:00 DONE (2022-11-08 11:22) 7.692g/s 61.53p/s 61.53c/s 61.53C/s 123456
    Use the "--show" option to display all of the cracked passwords reliably
    Session completed.

Note: if nothing is refered, the discovered passwords are stored in the `~/.john/john.pot` file. If we like to specify any other pot location, we need to use the `--pot` option.


### Cracking MD5 password files

Now we are going to use larger dimension files, to explore JtR funcionalities. First we are going to download the Rockyou password file (`rockyou.txt`). We can [download](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt) this file from [here](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt).

After this, we are going to download some files from the "[Crack Me If You Can (DEFCON 2012)](https://contest-2012.korelogic.com/)" page.

We this page we can [download this file](http://contest-2012.korelogic.com/cmiyc_2012_password_hash_files.tar.bz2).

Uncompress the file:

    tar -xf cmiyc_2012_password_hash_files.tar.bz2

From all the files that are extracted, we are going to use this one:

    hashes-9.raw-md5.txt

This file contains raw MD5 passwords and has the following format:

    bigibson:ef31bd90925824302dde7d0b16f772a3:1676:0:::

So lets try to find some passwords using JtR to do a dictionary attack:

    john --format=raw-md5 --wordlist=rockyou.txt hashes-9.raw-md5.txt -pot=found.pot

And lets cross our fingers. Here are the results:

    Using default input encoding: UTF-8
    Loaded 3413 password hashes with no different salts (Raw-MD5 [MD5 256/256 AVX2 8x3])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    confused         (reddych)     
    firetruck        (aascott)     
    outcast          (chrism)     
    everyone         (tadams)     
    bigballer        (jthomas)     
    blacksmith       (lauraw)     
    fellowship       (bobj)     
    baking           (seanj)     
    Miller           (kim.harris)     
    firsttime        (bhill)     
    pig              (maxa)     
    afterwards       (whiteco)     
    winwinwin        (bradl)     
    one              (gajohnston)     
    fountains        (hlopez)     
    Vernon           (raybe)     
    Unfaithful       (wirahman)     
    semicolon        (hughesli)     
    phantom01        (thomasm)     
    here             (jamieg)     
    sorcery          (smithd)     
    interbank        (lewisa)     
    defensive        (court.miller)     
    Tomas            (brjackson)     

All the dicovered passwords are recorded to the `found.pot` file - as specified in the command. Look at its content:

    $dynamic_0$1a7f2a5ad77128b2f81feddac78df213:confused
    $dynamic_0$320824ed82a8da4f1250345c09c28aac:firetruck
    $dynamic_0$8667db851472b942fb1e49c3a62c5b8e:outcast
    $dynamic_0$ed881bac6397ede33c0a285c9f50bb83:everyone
    $dynamic_0$ca21ff79eeab1645fafe86164f4e9528:bigballer
    $dynamic_0$c2428781706d00727581426181d0b7e6:blacksmith
    $dynamic_0$c08c1825c664280120a3587051819f0b:fellowship
    $dynamic_0$d567fa2fc32e8ef7b736c121e4975689:baking
    $dynamic_0$8f22e8ff6e2be10b6bc7e6cf7b47d782:Miller
    $dynamic_0$0691ffc309dda62cb45a9dcdfe21c64b:firsttime
    $dynamic_0$f74c6af46a78becb2f1bd3f95bbd5858:pig
    $dynamic_0$622f90ceba19c667b7d2620169144476:afterwards
    $dynamic_0$b6be03b4e958a964a552ae354d82a4ea:winwinwin
    $dynamic_0$f97c5d29941bfb1b2fdab0874906ab82:one
    $dynamic_0$1825036c05462007255c6081088d30d0:fountains
    $dynamic_0$b928a50c7e877267d29c7f20d01668fb:Vernon


Another important issue to consider when using a dictionary attack is to look at **word mangling rules** that JtR can use to do combinations with the words in the dictionary. For more information about this, please check [this](https://www.openwall.com/john/doc/MODES.shtml) and [this](https://www.openwall.com/john/doc/RULES.shtml), and then try to use.

We can also use a brute force approach for this. In order to do that we can do the following:

    john --incremental cmiyc_2012_password_hash_files/hashes-9.raw-md5.txt

But this will take a very long time!!!


### Cracking password protected files

Another useful feature that JtR offers is the possibility to crack a password of a file, namely zip files. So lets try to do that.

First we need a password-protected zip file. Either you can create your own file, or [you can use this file](files/logo.zip), that I've prepared previously (`logo.zip`).

The first thing we need to do is to extract the hash part of the file that contains the password. To to that we'll use the following command:

    zip2john logo.zip > file.hash

The `file.hash` has the following content.

    logo.zip:$pkzip$2*1*1*0*8*24*e732*c4f0fa14e585eb76e2bf36d47c19e8cb2d98f9de3094a69bcbdf869e51d8a9c3b1ec2707*2*0*f4*157*d112e403*27*3c*8*f4*d112*6ac3e43dcdf72a7d2617ef745d527759ef6936e34aec0ec33da650ab3516d379a1d0514d7a0c3c56a9a7ede23c3fe491409ff3b70e7f05a6a8ded2e15a514427f300b0ada06c050aad425ca92035e9cd088897a252dc2bee1651fc51417990ae2d1829e7ec7fda7e1fb9a516f95e15c8b277ac264c2d6498f6db0f3b498eec5cb94fea036ecf296ff2724d719b8931f7fe65290d955eed09d79396eef47091b8f6a4ffa31926f3c8f9b9a1e8ca0da5ae0f634f01bc174d329c544b5dee16e763fe9060daea76e43a5754de356240ebe884ecec2f4a55ee6dd78762d8711cf9b00c538ac0335519138be7a0164fb6688819d778aa*$/pkzip$::logo.zip:__MACOSX/._aim-health-logo.png, aim-health-logo.png:logo.zip

Next, we'll use a dictionary attack to try to find the password. The dictionary that we'll use is the `rockyou.txt` file. The command is as follows:

    john --wordlist=rockyou.txt file.hash -pot=found.pot 

And lets look at what we've found.

    Using default input encoding: UTF-8
    Loaded 1 password hash (PKZIP [32/64])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    password         (logo.zip)     
    1g 0:00:00:00 DONE (2022-11-08 19:27) 100.0g/s 6400p/s 6400c/s 6400C/s 123456..charlie
    Use the "--show" option to display all of the cracked passwords reliably
    Session completed.


## References/Additional information

1. [https://www.golinuxcloud.com/john-the-ripper-password-cracker/](https://www.golinuxcloud.com/john-the-ripper-password-cracker/)
2. [https://techofide.com/blogs/how-to-use-john-the-ripper-john-the-ripper-password-cracker-techofide/](https://techofide.com/blogs/how-to-use-john-the-ripper-john-the-ripper-password-cracker-techofide/)
3. [https://www.hackingarticles.in/beginner-guide-john-the-ripper-part-1/](https://www.hackingarticles.in/beginner-guide-john-the-ripper-part-1/)
