# Attacks on Password-based Systems  <!-- omit in toc -->

## Table of Contents  <!-- omit in toc -->

- [Introduction](#introduction)
- [Understanding Passwords](#understanding-passwords)
  - [Check your passwords](#check-your-passwords)
  - [Massive list of passwords](#massive-list-of-passwords)
  - [Checking for password robustness](#checking-for-password-robustness)
- [Attacks on Passwords](#attacks-on-passwords)

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

There are multiple sites that aggregate lots of passwords. These passwords can be used to conduct diccionary attacks, that test all the existing passwords to check if some of them works.

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
4. Check on [EFF Dice-Generate Passphrases](https://www.eff.org/dice). Look at the [wordlist diccionary](https://www.eff.org/files/2016/07/18/eff_large_wordlist.txt). Try the proposed process to create a great passphrase;
5. Also look at the [Diceware Passphrase web site](https://theworld.com/~reinhold/diceware.html);
6. Finnaly try to create a passwords/passphrases and [check its strenght](https://bitwarden.com/password-strength/).

## Attacks on Passwords

