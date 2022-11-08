# Hashcat  <!-- omit in toc -->

## Table of Contents  <!-- omit in toc -->

- [Hashcat](#hashcat)
  - [Using Hashcat to do a brute-force attack](#using-hashcat-to-do-a-brute-force-attack)
- [References/Additional information](#referencesadditional-information)

## Hashcat

[Hashcat](https://hashcat.net/hashcat/) is another password-cracking tool. It is mostly used towards cracking hash-based passwords. As JtR it can take advantage of external hardware, such as GPUs.

![hashcat logo](assets/picture05.png)

First thing to do: check the help of the Hashcat tool:

    hashcat --help

Look in detail for the help and be amazed with the amount of options it supports.

Hashcat supports the following attack modes:

- **Brute-Force attack (3)**: This type of attack consists of massive character combination tries. This attack technique was discontinued on Hashcat and was replaced by Mask attacks.

- **Combination attack (1)**: This mode allows to append each word contained in a wordlist to the end of each word container in a second wordlist.

- **Dictionary attack (0)**: This mode, also called “Straight mode,” tries all lines contained in a file as a password. This is a simple wordlist attack.

- **Hybrid attack**: The Hybrid attack mode allows combining a dictionary attack with a brute force attack. By using this mode, you can append or prepend wordlist elements to a bruteforce attack.

- **Mask attack (6 or 7)**: The Mask attack is an improvement of the brute force attack, aiming to design “intelligent” brute force attacks in which the user has control over the password candidate generation process. For example, the Mask attack allows users to define patterns like a capital letter for the first position of the password candidat only, or append dates at the end of the password candidate, or before, etc. The 6 mode enables Hybrid Wordlist + Mask, while the 7 mode enables Hybrid Mask + Wordlist.

### Using Hashcat to do a brute-force attack

To do a simple brute-force attack on a file with raw MD5 passwords (`md5-passwords.txt`) (download it [here](files/md5-passwords.txt)), we can simply run the command:

    hashcat -m 0 md5-passwords.txt

The option `-m 0` means that we are using MD5 hashes.

    OpenCL API (OpenCL 3.0 PoCL 3.0+debian  Linux, None+Asserts, RELOC, LLVM 13.0.1, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
    ============================================================================================================================================
    * Device #1: pthread-Intel(R) Core(TM) i9-9880H CPU @ 2.30GHz, 1428/2921 MB (512 MB allocatable), 1MCU

    Minimum password length supported by kernel: 0
    Maximum password length supported by kernel: 256

    Hashes: 11 digests; 11 unique digests, 1 unique salts
    Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
    Rules: 1

    Optimizers applied:
    * Zero-Byte
    * Early-Skip
    * Not-Salted
    * Not-Iterated
    * Single-Salt
    * Raw-Hash

    ATTENTION! Pure (unoptimized) backend kernels selected.
    Pure kernels can crack longer passwords, but drastically reduce performance.
    If you want to switch to optimized kernels, append -O to your commandline.
    See the above message to find out about the exact limits.

    Watchdog: Temperature abort trigger set to 90c

    Host memory required for this attack: 0 MB

    Starting attack in stdin mode

    Session..........: hashcat
    Status...........: Running
    Hash.Mode........: 0 (MD5)
    Hash.Target......: md5-passwords.txt
    Time.Started.....: Tue Nov  8 20:14:02 2022 (20 secs)
    Time.Estimated...: Tue Nov  8 20:14:22 2022 (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: Pipe
    Speed.#1.........:        0 H/s (0.00ms) @ Accel:256 Loops:1 Thr:1 Vec:8
    Recovered........: 0/11 (0.00%) Digests (total), 0/11 (0.00%) Digests (new)
    Progress.........: 0
    Rejected.........: 0
    Restore.Point....: 0
    Restore.Sub.#1...: Salt:0 Amplifier:0-0 Iteration:0-1
    Candidate.Engine.: Device Generator
    Candidates.#1....: [Copying]
    Hardware.Mon.#1..: Util:  3%


And expect this to take a loooooooot of time to complete.


## References/Additional information

1. [https://www.tunnelsup.com/getting-started-cracking-password-hashes/](https://www.tunnelsup.com/getting-started-cracking-password-hashes/)
2. [https://resources.infosecinstitute.com/topic/hashcat-tutorial-beginners/](https://resources.infosecinstitute.com/topic/hashcat-tutorial-beginners/)
3. [https://linuxhint.com/hashcat-tutorial/](https://linuxhint.com/hashcat-tutorial/)
4. [https://www.cyberpratibha.com/hashcat-tutorial-for-password-cracking/](https://www.cyberpratibha.com/hashcat-tutorial-for-password-cracking/)