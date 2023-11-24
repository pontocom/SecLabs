# Cryptography with OpenSSL/LibreSSL <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Introduction](#introduction)
- [Setup](#setup)
- [Basic commands](#basic-commands)
  - [Check the OpenSSL version](#check-the-openssl-version)
- [OpenSSL interactive console access](#openssl-interactive-console-access)
- [Check the options for a particular command](#check-the-options-for-a-particular-command)
- [Symmetric Cryptography](#symmetric-cryptography)
  - [List of options for an algorithm](#list-of-options-for-an-algorithm)
  - [Encrypt a file](#encrypt-a-file)
  - [Decrypt a file](#decrypt-a-file)
  - [Ciphering and deciphering an image](#ciphering-and-deciphering-an-image)
- [Hash generation](#hash-generation)
  - [Generate hash of a file](#generate-hash-of-a-file)
  - [Generate a Message Authentication Code](#generate-a-message-authentication-code)
- [Asymmetric cryptography](#asymmetric-cryptography)
  - [Key pair generation](#key-pair-generation)
  - [Generate a key pair](#generate-a-key-pair)
  - [Generate a key pair and protect the private key](#generate-a-key-pair-and-protect-the-private-key)
  - [Print the various components of the key pair](#print-the-various-components-of-the-key-pair)
  - [Extract the public key from the key pair](#extract-the-public-key-from-the-key-pair)
  - [Encrypt using the public key](#encrypt-using-the-public-key)
  - [Decrypt using the private key](#decrypt-using-the-private-key)
- [Using the Elliptic Curve Cryptography](#using-the-elliptic-curve-cryptography)
  - [Listing the available curves](#listing-the-available-curves)
  - [Generate a private key using ECC](#generate-a-private-key-using-ecc)
  - [Generate a public key using ECC](#generate-a-public-key-using-ecc)
  - [Encrypt with ECC](#encrypt-with-ecc)
- [Using Diffie-Hellman (DH) for key exchange](#using-diffie-hellman-dh-for-key-exchange)
  - [Generate DH public key parameters](#generate-dh-public-key-parameters)
  - [Generate the DH private and public keys](#generate-the-dh-private-and-public-keys)
  - [Encrypt and decrypt with DH](#encrypt-and-decrypt-with-dh)
- [Digital signatures](#digital-signatures)
  - [Generate a digital signature for a file](#generate-a-digital-signature-for-a-file)
  - [Verify the digital signature of a file](#verify-the-digital-signature-of-a-file)
- [Digital certificates](#digital-certificates)
  - [Create a certification authority](#create-a-certification-authority)
  - [Verify the generated digital certificate](#verify-the-generated-digital-certificate)
  - [Generate a digital certificate request](#generate-a-digital-certificate-request)
  - [Issuance of the digital certificate](#issuance-of-the-digital-certificate)
  - [Revocation of digital certificates](#revocation-of-digital-certificates)
- [OCSP - Online Certificate Status Protocol](#ocsp---online-certificate-status-protocol)
  - [Getting the server certificate](#getting-the-server-certificate)
  - [Getting the intermediate certificates](#getting-the-intermediate-certificates)
  - [Get the OCSP responder for a certificate](#get-the-ocsp-responder-for-a-certificate)
  - [Make the OCSP request](#make-the-ocsp-request)
- [S/MIME](#smime)
  - [Create keys and certificates](#create-keys-and-certificates)
    - [Create key pairs and CSR for certificate request](#create-key-pairs-and-csr-for-certificate-request)
    - [Request and issue the certificates](#request-and-issue-the-certificates)
  - [Encrypt MIME information](#encrypt-mime-information)
  - [Decrypt MIME information](#decrypt-mime-information)
  - [Sign MIME information](#sign-mime-information)
  - [Verify digital signature of MIME information](#verify-digital-signature-of-mime-information)
- [Transport Layer Security/ Secure Sockets Layer (SSL/TLS)](#transport-layer-security-secure-sockets-layer-ssltls)
  - [Check a SSL/TLS connection to a server](#check-a-ssltls-connection-to-a-server)
  - [Get a server digital certificate](#get-a-server-digital-certificate)
  - [SSLscan](#sslscan)
  - [SSLyze](#sslyze)
  - [testssl.sh](#testsslsh)
  - [SSL/TLS online tools](#ssltls-online-tools)
- [References/Additional information](#referencesadditional-information)

## Introduction
This document aims to demonstrate the use of cryptographic mechanisms based on the OpenSSL/LibreSSL library. This library has the ability to work with cryptographic mechanisms such as symmetric cryptography, asymmetric cryptography, generation of message authentication codes as well as work with digital certificates and more.

To run these labs the following requirements are needed:
- OpenSSL/LibreSSL.

The goal of these labs is to demonstrate the use of the OpenSSL library to provide a range of cryptographic functionality.

The examples presented here only demonstrate just a small sample of the full capabilities of OpenSSL.

## Setup

There is nothing very relevant to do in terms of setup. You just need to install the OpenSSL library - if you use the Kali Linux distribution (or any other Linux distribution, such as Ubuntu, Debian or Parrot OS), it may already be installed by default.

![](assets/picture01.png)

On Windows, OpenSSL can be installed using for example the Cygwin software package. On MacOS, OpenSSL is installed by default, but if you need to use a newer version of it, you can use the Brew  tool (https://brew.sh).

For Windows there are also other options that you can use. So, the main alternatives for installing OpenSSL on Windows 10/11, are as follows (from the simplest to the most complicated):
1. Use Chocolatey (https://chocolatey.org/) to install OpenSSL on Windows 10 with Powershell, or directly install the OpenSSL binary from the web site;
2. Use Windows Subsystem for Linux (WSL), which allows you to run Linux applications on Windows 10/11 - in practice it is like installing Linux on top of Windows 10/11;
3. Use a Linux distribution, inside a virtualization tool like VMware (https://www.vmware.com/) or VirtualBox (https://www.virtualbox.org/), on top of Windows 10/11;
4. Any other, which may involve the use of containers such as Docker (https://www.docker.com/).

Regarding option 1, it is described in the following links:
- https://adamtheautomator.com/openssl-windows-10/ 
- https://slproweb.com/products/Win32OpenSSL.html ; https://medium.com/swlh/installing-openssl-on-windows-10-and-updating-path-80992e26f6a1 

Regarding option 2, it is explained in the following links: 
- https://www.windowscentral.com/install-windows-subsystem-linux-windows-10 
- https://docs.microsoft.com/en-us/windows/wsl/install 

Regarding option 3, it consists of:
- Download a Linux distribution, such as Ubuntu (https://ubuntu.com/download/desktop/thank-you?version=20.04.3&architecture=amd64) 
- Download and install either VMware Workstation Player (https://www.vmware.com/products/workstation-player.html) or VirtualBox  (https://www.virtualbox.org/) on Windows 10/11. This site even has already prepared Ubuntu images to be installed in both virtualization environments;
- Install the Linux distribution in the selected virtualization environment.

[This page](https://thesecmaster.com/procedure-to-install-openssl-on-the-windows-platform/) contains also important and relevant information on how to install OpenSSL on Windows. It provides a detailed step-by-step process on how to do it.

If you choose, you may also not install OpenSSL natively and use a web-based version that can be found [here](https://www.cryptool.org/en/cto/openssl).

## Basic commands

Here we will just list some of the basic commands for working with OpenSSL.

### Check the OpenSSL version

    openssl version

    OpenSSL 1.1.1n  15 Mar 2022
    OPENSSL COMMAND LIST
    openssl help

    Standard commands
    asn1parse         ca                ciphers           cms
    crl               crl2pkcs7         dgst              dhparam
    dsa               dsaparam          ec                ecparam
    enc               engine            errstr            gendsa
    genpkey           genrsa            help              list
    nseq              ocsp              passwd            pkcs12
    pkcs7             pkcs8             pkey              pkeyparam
    pkeyutl           prime             rand              rehash
    req               rsa               rsautl            s_client
    s_server          s_time            sess_id           smime
    speed             spkac             srp               storeutl
    ts                verify            version           x509

    Message Digest commands (see the `dgst' command for more details)
    blake2b512        blake2s256        gost              md4
    md5               mdc2              rmd160            sha1
    sha224            sha256            sha3-224          sha3-256
    sha3-384          sha3-512          sha384            sha512
    sha512-224        sha512-256        shake128          shake256
    sm3

    Cipher commands (see the `enc' command for more details)
    aes-128-cbc       aes-128-ecb       aes-192-cbc       aes-192-ecb
    aes-256-cbc       aes-256-ecb       aria-128-cbc      aria-128-cfb
    aria-128-cfb1     aria-128-cfb8     aria-128-ctr      aria-128-ecb
    aria-128-ofb      aria-192-cbc      aria-192-cfb      aria-192-cfb1
    aria-192-cfb8     aria-192-ctr      aria-192-ecb      aria-192-ofb
    aria-256-cbc      aria-256-cfb      aria-256-cfb1     aria-256-cfb8
    aria-256-ctr      aria-256-ecb      aria-256-ofb      base64
    bf                bf-cbc            bf-cfb            bf-ecb
    bf-ofb            camellia-128-cbc  camellia-128-ecb  camellia-192-cbc
    camellia-192-ecb  camellia-256-cbc  camellia-256-ecb  cast
    cast-cbc          cast5-cbc         cast5-cfb         cast5-ecb
    cast5-ofb         des               des-cbc           des-cfb
    des-ecb           des-ede           des-ede-cbc       des-ede-cfb
    des-ede-ofb       des-ede3          des-ede3-cbc      des-ede3-cfb
    des-ede3-ofb      des-ofb           des3              desx
    idea              idea-cbc          idea-cfb          idea-ecb
    idea-ofb          rc2               rc2-40-cbc        rc2-64-cbc
    rc2-cbc           rc2-cfb           rc2-ecb           rc2-ofb
    rc4               rc4-40            seed              seed-cbc
    seed-cfb          seed-ecb          seed-ofb          sm4-cbc
    sm4-cfb           sm4-ctr           sm4-ecb           sm4-ofb

## OpenSSL interactive console access

    openssl

    OpenSSL> 

## Check the options for a particular command

    openssl ca -help

    Usage: ca [options]
    Valid options are:
    -help                   Display this summary
    -verbose        - Talk alot while doing things
    -config file    - A config file
    -name arg       - The particular CA definition to use
    -gencrl         - Generate a new CRL
    -crldays days   - Days is when the next CRL is due
    -crlhours hours - Hours is when the next CRL is due
    -startdate YYMMDDHHMMSSZ  - certificate validity notBefore
    -enddate YYMMDDHHMMSSZ    - certificate validity notAfter (overrides -days)
    -days arg       - number of days to certify the certificate for
    -md arg         - md to use, one of md2, md5, sha or sha1
    -policy arg     - The CA 'policy' to support
    -keyfile arg    - private key file
    -keyform arg    - private key file format (PEM or ENGINE)
    -key arg        - key to decode the private key if it is encrypted
    -cert file      - The CA certificate
    -selfsign       - sign a certificate with the key associated with it
    -in file        - The input PEM encoded certificate request(s)
    -out file       - Where to put the output file(s)
    -outdir dir     - Where to put output certificates
    -infiles ....   - The last argument, requests to process
    -spkac file     - File contains DN and signed public key and challenge
    -ss_cert file   - File contains a self signed cert to sign
    -preserveDN     - Don't re-order the DN
    -noemailDN      - Don't add the EMAIL field into certificate' subject
    -batch          - Don't ask questions
    -msie_hack      - msie modifications to handle all those universal strings
    -revoke file    - Revoke a certificate (given in file)
    -subj arg       - Use arg instead of request's subject
    -utf8           - input characters are UTF8 (default ASCII)
    -multivalue-rdn - enable support for multivalued RDNs
    -extensions ..  - Extension section (override value in config file)
    -extfile file   - Configuration file with X509v3 extentions to add
    -crlexts ..     - CRL extension section (override value in config file)
    -engine e       - use engine e, possibly a hardware device.
    -status serial  - Shows certificate status given the serial number
    -updatedb       - Updates db for expired certificates

Most of the times, it is also better to read the documentation on the [OpenSSL website](https://www.openssl.org), since it is must more verbose than the information provided by the CLI.

## Symmetric Cryptography

Here we will demonstrate some of the features of the OpenSSL library for performing symmetric key cryptography.

### List of options for an algorithm

    openssl aes-128-ecb -help

    usage: enc -ciphername [-AadePp] [-base64] [-bufsize number] [-debug]
        [-in file] [-iv IV] [-K key] [-k password]
        [-kfile file] [-md digest] [-none] [-nopad] [-nosalt]
        [-out file] [-pass arg] [-S salt] [-salt]

    -A                 Process base64 data on one line (requires -a)
    -a                 Perform base64 encoding/decoding (alias -base64)
    -bufsize size      Specify the buffer size to use for I/O
    -d                 Decrypt the input data
    -debug             Print debugging information
    -e                 Encrypt the input data (default)
    -in file           Input file to read from (default stdin)
    -iv IV             IV to use, specified as a hexadecimal string
    -K key             Key to use, specified as a hexadecimal string
    -md digest         Digest to use to create a key from the passphrase
    -none              Use NULL cipher (no encryption or decryption)
    -nopad             Disable standard block padding
    -out file          Output file to write to (default stdout)
    -P                 Print out the salt, key and IV used, then exit
                        (no encryption or decryption is performed)
    -p                 Print out the salt, key and IV used
    -pass source       Password source
    -S salt            Salt to use, specified as a hexadecimal string
    -salt              Use a salt in the key derivation routines (default)
    -v                 Verbose

### Encrypt a file

We are going to encrypt a file (image.jpg), with a key, generated from a passphrase (a password selected by the user, which will be converted to a 128-bit secret key, using an algorithm called [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2)):

    openssl aes-128-ecb -in ./imagem.jpg -out ./imagem.aes.jpg -e -a -k secretpass -pbkdf2

### Decrypt a file

Let's now decrypt a file (image.aes.jpg) and recover the original format (image.orig.jpg).

    openssl aes-128-ecb -in ./imagem.aes.jpg -out ./imagem.orig.jpg -d -a -k secretpass -pbkdf2

### Ciphering and deciphering an image

It is intended that in this activity you can use symmetric key cryptography to encrypt an image using different modes of operation (ECB and CBC). In order to perform this activity you will have to get an uncompressed image (using for example Windows Bitmap (.bmp) format), in which you will have to separate the header from the image data (body).
You can choose any image on the Web or you can create your own. Don't forget that it has to be of the BMP type. It is preferable to use an image with high color contrast, for a better visual effect.

As a note to help you with this task, in order to separate the header from the image data (body), you can use the following commands:

    head -c 54 IMAGEM.bmp > header
    tail -c +55 IMAGEM.bmp > body

We will generate a random key to be able to encrypt the image:

    openssl rand -hex 32

We will encrypt the image using ECB mode (we use the random key obtained in the previous step):

    openssl aes-256-ecb -e -in ./body -out ./body_ecb -K 2ffd27e0675b9bd6c34e37109c4ebef378f356f0fa7eeeaf11eb2433e21e980e

After you have encrypted the image data (body), you can re-attach the header to get the image in the full format:

    cat header body_ecb > imagem_ECB.bmp

We are going to cipher the image now with CBC mode. For this mode we need to use an IV. To create this IV we can generate it randomly:

    openssl rand -hex 16

We will encrypt the image using CBC mode (we use the random key obtained in the previous step and the IV):

    openssl aes-256-cbc -e -in ./body -out ./body_cbc -K 2ffd27e0675b9bd6c34e37109c4ebef378f356f0fa7eeeaf11eb2433e21e980e -iv 0faf3e077df51ebd98cd11925ad9dcd1

After you have encrypted the image data (body), you can re-attach the header to get the image in the full format:

    cat header body_cbc > imagem_CBC.bmp

## Hash generation

OpenSSL also has features for generating hashes of various types.

### Generate hash of a file

Using SHA1

    openssl sha1 ./imagem.jpg 

    SHA1(./imagem.jpg)= 95e91837dc1a4a8eeb42208420d8620cb8d7785f

Using SHA256
    openssl sha256 ./imagem.jpg 

    SHA256(./imagem.jpg)= b110ed205353743923c7d66811a2916e2cc3bb3a06e7411e79a4b124ca1322d0

Using RIPEMD160
    openssl ripemd160 ./imagem.jpg 

    RIPEMD160(./imagem.jpg)= f9bb6ff349a5f03531af89774f9a6578a785aec8

### Generate a Message Authentication Code

In order to generate a hash-based message authentication code, we need to provide the hash algorithm to be used and the secret key to encrypt the hash.

    openssl sha1 -hmac 24899ec1e452d219121f0d07cd6975b7 ./imagem.jpg

    HMAC-SHA1(./imagem.jpg)= 384773867cb7bce6ffd97a95c26e65666c045ea7

## Asymmetric cryptography

OpenSSL also allows you to implement a series of asymmetric cryptographic features-key pair generation, encryption of decryption of information with OpenSSL.

### Key pair generation

You can see which parameters the key generation uses:

    openssl genrsa -help

    usage: genrsa [args] [numbits]
    -des            encrypt the generated key with DES in cbc mode
    -des3           encrypt the generated key with DES in ede cbc mode (168 bit key)
    -idea           encrypt the generated key with IDEA in cbc mode
    -seed
                    encrypt PEM output with cbc seed
    -aes128, -aes192, -aes256
                    encrypt PEM output with cbc aes
    -camellia128, -camellia192, -camellia256
                    encrypt PEM output with cbc camellia
    -out file       output the key to 'file
    -passout arg    output file pass phrase source
    -f4             use F4 (0x10001) for the E value
    -3              use 3 for the E value
    -engine e       use engine e, possibly a hardware device.
    -rand file:file:...
                    load the file (or the files in the directory) into
                    the random number generator

### Generate a key pair

Let's generate a key pair with 4096 bits of dimension.

    openssl genrsa -out ./keypair.pem 4096

    Generating RSA private key, 4096 bit long modulus
    ........................................................................................................................................................................++
    ..........................++
    e is 65537 (0x10001)

The generated key is stored in [PKCS#1](https://en.wikipedia.org/wiki/PKCS) format, with the following structure ([ASN.1](https://en.wikipedia.org/wiki/ASN.1)):

    RSAPrivateKey ::= SEQUENCE {
        version           Version,
        modulus           INTEGER,  -- n
        publicExponent    INTEGER,  -- e
        privateExponent   INTEGER,  -- d
        prime1            INTEGER,  -- p
        prime2            INTEGER,  -- q
        exponent1         INTEGER,  -- d mod (p-1)
        exponent2         INTEGER,  -- d mod (q-1)
        coefficient       INTEGER,  -- (inverse of q) mod p
        otherPrimeInfos   OtherPrimeInfos OPTIONAL
    }

To view it, we can do:

    openssl rsa -in keypair.pem -text

### Generate a key pair and protect the private key

We are going to create a keypair and protect the private key with a password (PKCS#5).

    openssl genrsa -out ./keypair.pem -aes128 4096

    Generating RSA private key, 4096 bit long modulus
    ...................................................................................++
    .......................................................................................................................................................................................................................................................................................................................................................................................................................++
    e is 65537 (0x10001)
    Enter pass phrase for ./keypair.pem:
    Verifying - Enter pass phrase for ./keypair.pem:

### Print the various components of the key pair

Prints the key components, in [PKCS#1](https://en.wikipedia.org/wiki/PKCS) format (see above).
    openssl rsa -in ./keypair.pem -text

    Enter pass phrase for ./keypair.pem:
    Private-Key: (4096 bit)
    modulus:
        00:d0:5c:05:50:df:6e:de:64:b3:57:de:60:15:d9:
        d5:7b:35:be:fa:6c:59:3e:bf:81:0d:db:4e:1d:9a:
        09:48:79:c7:92:fa:b4:25:38:f2:11:20:c3:da:c2:
        7c:0e:ff:c4:5a:cc:54:b5:c6:51:c1:6e:d3:4d:7f:
        ef:2c:04:38:da:cb:78:16:cf:08:28:3f:8c:cc:22:
        59:41:57:f1:8d:ae:27:ea:78:01:2b:58:e5:b4:60:
        0a:3a:aa:dd:f8:e5:5d:db:67:67:43:4a:97:78:eb:
        3a:e6:8b:f2:84:fd:24:e5:a3:06:98:a7:cf:d0:c5:
        b7:0a:ce:09:89:f0:53:d3:78:ac:6d:22:f8:8d:af:
        85:60:8f:c6:c0:cc:4b:35:01:51:61:1c:69:e1:16:
        b7:ae:4f:a4:2f:1f:66:fe:73:0f:81:ac:17:e8:22:
        68:45:40:75:71:25:48:73:42:fe:be:97:9c:d2:8c:
        aa:f3:f8:e5:41:00:24:3d:64:0f:e9:ab:45:0f:61:
        49:28:91:d9:7b:fd:6c:1b:c4:d7:73:be:ad:e4:fd:
        e9:ab:d8:aa:dc:68:b4:5d:30:98:33:9c:9c:f6:be:
        0d:64:dc:69:a8:a4:9e:f7:68:98:d0:4e:21:ca:0a:
        c8:7c:0f:8d:6d:a2:c4:4e:bc:eb:99:37:41:ff:8e:
        25:52:04:de:43:34:e1:77:16:7d:de:70:3b:52:02:
        45:19:1d:c9:25:bf:b4:8d:92:a4:d4:b8:c4:fc:14:
        8c:23:53:57:13:81:0a:8b:ab:db:9b:29:a6:39:29:
        4f:8c:24:e3:f2:1a:a1:39:e9:ed:7c:1d:53:ad:20:
        7e:92:ec:4d:14:cb:24:69:2f:cc:e4:56:68:d0:b5:
        77:28:0c:89:5a:5e:d6:e6:ac:0e:0a:6f:0d:75:49:
        df:a1:7f:3d:a9:b8:35:6a:69:32:14:b0:e0:f9:4f:
    (…)

### Extract the public key from the key pair

When generating the key pair, both keys are stored in the same file, so if you want to extract the public key, you have to do it explicitly.

    openssl rsa -in keypair.pem -pubout -out ./publickey.pem

### Encrypt using the public key

It is only suitable for encrypting small blocks of information. In this case a "`secretkey`" file was created, with a small random value using the command "`openssl rand -out ./secretkey 32`". Then this file was encrypted using the public key.

    openssl pkeyutl -encrypt -pubin -inkey ./publickey.pem -in ./secretkey -out ./secretkey.enc

### Decrypt using the private key

With the following command it is possible to get the original text back by decrypting it with the corresponding private key.

    openssl pkeyutl -decrypt -inkey ./keypair.pem -in ./secretkey.enc -out ./secretkey.dec 

## Using the Elliptic Curve Cryptography

The following section of this document is related to the usage of ECC – Elliptic Curve Cryptography. ECC is an approach to public-key cryptography based on the algebraic structure of elliptic curves over finite fields. ECC allows smaller keys compared to non-EC cryptography (based on plain Galois fields) to provide equivalent security. Elliptic curves are applicable for key agreement, digital signatures, pseudo-random generators and other tasks. Indirectly, they can be used for encryption by combining the key agreement with a symmetric encryption scheme. Elliptic curves are also used in several integer factorization algorithms based on elliptic curves that have applications in cryptography, such as Lenstra elliptic-curve factorization.

### Listing the available curves

The first think that is necessary to do is to select the appropriate curve to work with. In order to do that, the following command can be used:

    openssl ecparam -list_curves

As a result, we obtain a list of possible curves (this is just a sample):

    secp112r1 : SECG/WTLS curve over a 112 bit prime field
    secp112r2 : SECG curve over a 112 bit prime field
    secp128r1 : SECG curve over a 128 bit prime field
    secp128r2 : SECG curve over a 128 bit prime field
    secp160k1 : SECG curve over a 160 bit prime field
    secp160r1 : SECG curve over a 160 bit prime field
    secp160r2 : SECG/WTLS curve over a 160 bit prime field
    secp192k1 : SECG curve over a 192 bit prime field
    secp224k1 : SECG curve over a 224 bit prime field
    secp224r1 : NIST/SECG curve over a 224 bit prime field
    secp256k1 : SECG curve over a 256 bit prime field
    secp384r1 : NIST/SECG curve over a 384 bit prime field
    secp521r1 : NIST/SECG curve over a 521 bit prime field
    prime192v1: NIST/X9.62/SECG curve over a 192 bit prime field
    prime192v2: X9.62 curve over a 192 bit prime field
    prime192v3: X9.62 curve over a 192 bit prime field
    prime239v1: X9.62 curve over a 239 bit prime field
    prime239v2: X9.62 curve over a 239 bit prime field
    prime239v3: X9.62 curve over a 239 bit prime field
    prime256v1: X9.62/SECG curve over a 256 bit prime field

### Generate a private key using ECC

Let us generate a private key using ECC and an appropriate curve (in this case, we are going to use prime256v1 [[1]](https://neuromancer.sk/std/x962/prime256v1) [[2]](https://blog.qrator.net/en/ecc-tls-13_59/ )).

![](assets/picture02.png)
 
    openssl ecparam -name prime256v1 -genkey -noout

If needed, it may be stored on a file:

    openssl ecparam -name prime256v1 -genkey -noout -out private-key.pem

The contents of the file:

    -----BEGIN EC PRIVATE KEY-----
    MHcCAQEEICfuGbJ609FiPS8cQ5Wf5+73RgxFj033iN7Ny1y2DuxMoAoGCCqGSM49
    AwEHoUQDQgAE8FK5nC4t2Zgn0X7jtqylxJR8gype4sr9hl2qzdcMPgLnZwn/2xoY
    /auPLm0jWuKBvIahi1GwmEzhDZj6UhY5SA==
    -----END EC PRIVATE KEY-----

Display the parameters of the private key:

    openssl ec -in private-key.pem -noout -text

The contents of the private key:

    read EC key
    Private-Key: (256 bit)
    priv:
        d3:e4:e9:82:26:fc:bc:f9:32:98:d3:7b:67:7f:76:
        b2:dc:cc:f3:51:bf:54:6b:51:59:fa:33:a3:bf:c2:
        7c:23
    pub:
        04:19:8d:b4:ba:4f:ed:99:ad:49:91:f3:62:5d:b5:
        8f:b2:15:d5:c5:fc:3e:63:61:a8:c9:c1:f6:b7:9b:
        4e:71:fe:f4:77:3b:4c:69:79:a8:cc:a1:f2:42:ef:
        04:bd:ca:d5:68:13:7c:e9:8f:bd:ab:e7:76:82:c8:
        7f:59:42:78:18
    ASN1 OID: prime256v1
    NIST CURVE: P-256

### Generate a public key using ECC

Next what we need to do is to create a public key from an ECC private key:

    openssl ec -in private-key.pem -pubout -out public-key.pem

This is the content of the public key:

    -----BEGIN PUBLIC KEY-----
    MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEGY20uk/tma1JkfNiXbWPshXVxfw+
    Y2GoycH2t5tOcf70dztMaXmozKHyQu8EvcrVaBN86Y+9q+d2gsh/WUJ4GA==
    -----END PUBLIC KEY-----

Let’s visualize the parameters of the public key:

    openssl ec -in public-key.pem -pubin -noout -text

This is the content of the public key:

    read EC key
    Public-Key: (256 bit)
    pub:
        04:19:8d:b4:ba:4f:ed:99:ad:49:91:f3:62:5d:b5:
        8f:b2:15:d5:c5:fc:3e:63:61:a8:c9:c1:f6:b7:9b:
        4e:71:fe:f4:77:3b:4c:69:79:a8:cc:a1:f2:42:ef:
        04:bd:ca:d5:68:13:7c:e9:8f:bd:ab:e7:76:82:c8:
        7f:59:42:78:18
    ASN1 OID: prime256v1
    NIST CURVE: P-256

### Encrypt with ECC

As mentioned before, ECC can be used to do some key agreement between two parties, to derive a common key that might be used for encryption and decryption, using an appropriate symmetric cryptography algorithm.

Let’s imagine that you have **Alice** and **Bob** that want to use ECC to create a common shared key. The following should be done.

**Alice**, creates a private key and extracts the public key:

    openssl ecparam -name prime256v1 -genkey -noout -out alice-private-key.pem
    openssl ec -in alice-private-key.pem -pubout -out alice-public-key.pem

**Bob**, creates a private key and extracts the public key:

    openssl ecparam -name prime256v1 -genkey -noout -out bob-private-key.pem
    openssl ec -in bob-private-key.pem -pubout -out bob-public-key.pem

**Alice** and **Bob**, exchange their public keys. So **Alice** sends `alice-public-key.pem` to **Bob**, and **Bob** sends `bob-public-key.pem` to **Alice**.

In **Alice**, creates a shared key (`alice_shared_key.bin`):

    openssl pkeyutl -derive -inkey alice-private-key.pem -peerkey bob-public-key.pem -out alice_shared_key.bin

In Bob, creates a shared key (`bob_shared_key.bin`):

    openssl pkeyutl -derive -inkey bob-private-key.pem -peerkey alice-public-key.pem -out bob_shared_key.bin

If we verify, both `alice_shared_key.bin` and `bob_shared_key.bin` are identical:

    hexdump alice_shared_key.bin

    00000000  10 2c 05 53 62 8d 94 48  f0 bd 34 10 06 53 c2 2d  |.,.Sb..H�4..S�-|
    00000010  e3 d0 cf 31 c1 4a 2d eb  a7 f1 3c 5e d2 3c 31 17  |���1�J-��<^�<1.|
    00000020

    hexdump bob_shared_key.bin

    00000000  10 2c 05 53 62 8d 94 48  f0 bd 34 10 06 53 c2 2d  |.,.Sb..H�4..S�-|
    00000010  e3 d0 cf 31 c1 4a 2d eb  a7 f1 3c 5e d2 3c 31 17  |���1�J-��<^�<1.|
    00000020

So, this common 256-bit shared key (`102c0553628d9448f0bd34100653c22de3d0cf31c14a2deba7f13c5ed23c3117`) can be used to encrypt and decrypt data.

**Encrypt**:

    openssl aes-256-ecb -base64 -K 102c0553628d9448f0bd34100653c22de3d0cf31c14a2deba7f13c5ed23c3117 -e -in plain.txt -out cipher.txt

**Decrypt**:

    openssl aes-256-ecb -base64 -K 102c0553628d9448f0bd34100653c22de3d0cf31c14a2deba7f13c5ed23c3117 -d -in cipher.txt -out original.txt

## Using Diffie-Hellman (DH) for key exchange

Diffie-Hellman is one of the oldest public-key cryptosystems. It is often called a key-agreement protocol, because it allows the establishment of a common secret key, that can be used to encrypt and decrypt messages.

### Generate DH public key parameters

Generate the DH public key parameters and save them to a file (this may take a while to complete, be patient).

    openssl dhparam -out dhparams.pem 2048

Here are the parameters generated in base64.

    -----BEGIN DH PARAMETERS-----
    MIIBCAKCAQEA/J7CXLjNaOZoL9QNkhlPI8iGjuI1hVLHKCxdLP5LhLqqPfOiKLBE
    8wU8LgiAOiH7jMW4BfohuoCRG7E8xWnplt1Rn1SAiiZzdeKKY4t08fiCAmO3EUHj
    sxXPwV537IbbN2d2MRxIodCla9nIx/x906foXfhpr4mi3k+dfg5lv/2HOnjST0qR
    NccwjqH+k3ye03bhuB6D9pEL2prRrDxn13q4UGoh0Uwp/wcxHQk1sy6ouvaaEJYm
    OKemxWVZ19EPOpIVxvgVTa0e28z7zlDP/9+xN7fCu46Irv0SWVxa2JnAogP308rk
    8xAgeRJA4lRKOAgFb2dJZSNT65w19SeBqwIBAg==
    -----END DH PARAMETERS-----

To view the contents of the parameters of the file, please do:

    openssl pkeyparam -in dhparams.pem -text

And you'll have access to the parameters created.

    -----BEGIN DH PARAMETERS-----
    MIIBCAKCAQEA/J7CXLjNaOZoL9QNkhlPI8iGjuI1hVLHKCxdLP5LhLqqPfOiKLBE
    8wU8LgiAOiH7jMW4BfohuoCRG7E8xWnplt1Rn1SAiiZzdeKKY4t08fiCAmO3EUHj
    sxXPwV537IbbN2d2MRxIodCla9nIx/x906foXfhpr4mi3k+dfg5lv/2HOnjST0qR
    NccwjqH+k3ye03bhuB6D9pEL2prRrDxn13q4UGoh0Uwp/wcxHQk1sy6ouvaaEJYm
    OKemxWVZ19EPOpIVxvgVTa0e28z7zlDP/9+xN7fCu46Irv0SWVxa2JnAogP308rk
    8xAgeRJA4lRKOAgFb2dJZSNT65w19SeBqwIBAg==
    -----END DH PARAMETERS-----
    DH Parameters: (2048 bit)
        prime:
            00:fc:9e:c2:5c:b8:cd:68:e6:68:2f:d4:0d:92:19:
            4f:23:c8:86:8e:e2:35:85:52:c7:28:2c:5d:2c:fe:
            4b:84:ba:aa:3d:f3:a2:28:b0:44:f3:05:3c:2e:08:
            80:3a:21:fb:8c:c5:b8:05:fa:21:ba:80:91:1b:b1:
            3c:c5:69:e9:96:dd:51:9f:54:80:8a:26:73:75:e2:
            8a:63:8b:74:f1:f8:82:02:63:b7:11:41:e3:b3:15:
            cf:c1:5e:77:ec:86:db:37:67:76:31:1c:48:a1:d0:
            a5:6b:d9:c8:c7:fc:7d:d3:a7:e8:5d:f8:69:af:89:
            a2:de:4f:9d:7e:0e:65:bf:fd:87:3a:78:d2:4f:4a:
            91:35:c7:30:8e:a1:fe:93:7c:9e:d3:76:e1:b8:1e:
            83:f6:91:0b:da:9a:d1:ac:3c:67:d7:7a:b8:50:6a:
            21:d1:4c:29:ff:07:31:1d:09:35:b3:2e:a8:ba:f6:
            9a:10:96:26:38:a7:a6:c5:65:59:d7:d1:0f:3a:92:
            15:c6:f8:15:4d:ad:1e:db:cc:fb:ce:50:cf:ff:df:
            b1:37:b7:c2:bb:8e:88:ae:fd:12:59:5c:5a:d8:99:
            c0:a2:03:f7:d3:ca:e4:f3:10:20:79:12:40:e2:54:
            4a:38:08:05:6f:67:49:65:23:53:eb:9c:35:f5:27:
            81:ab
        generator: 2 (0x2)

### Generate the DH private and public keys

Using the DH public parameters that were created in the previous step, it is now necessary to create the public and private keys of both entities that need to communicate (again **Alice** and **Bob**).

So, both **Alice** and **Bob**, need to have the `dhparams.pem` file created in the previous step.

Now, **Alice** can do the following to create its private key:

    openssl genpkey -paramfile dhparams.pem -out alice_dh_key.pem

Let’s look at the content of **Alice** private key:

    openssl pkey -in alice_dh_key.pem -text -noout

This is the contents of the private key:

    DH Private-Key: (2048 bit)
        private-key:
            68:fd:1e:8b:01:a1:6b:b1:80:2d:a5:9d:19:f5:74:
            5c:5a:9f:d2:ea:a0:64:69:3e:99:e4:e0:85:fc:e5:
            d0:cc:f5:f9:76:61:07:22:4c:1f:2b:7b:f2:9a:6b:
            0c:dc:11:1c:70:7c:29:ff:e9:13:48:7b:01:b2:11:
            e9:13:92:a1:a9:59:22:2c:de:66:55:d5:c2:12:93:
            cf:d4:ed:d8:46:45:2b:86:32:c8:06:ff:33:58:43:
            02:a5:df:f1:8d:19:a9:d6:26:c2:04:71:54:65:55:
            67:d3:8f:e2:48:ba:e7:58:f2:58:18:f3:56:5a:ca:
            b2:ae:3a:08:a1:d2:fc:6e:95:31:7e:d3:94:cf:72:
            66:2c:58:5e:3f:16:b2:3b:a3:0a:d4:00:b7:e4:e3:
            34:5f:91:e6:60:75:3c:7f:ae:a9:37:37:2b:e4:e2:
            f4:82:0a:28:5a:18:b2:56:f8:26:81:76:8e:1f:07:
            9d:f0:de:a6:6c:34:34:44:ce:20:d2:fb:55:7c:07:
            28:f9:cd:02:81:7d:0d:bd:57:7b:13:a9:1d:fd:f6:
            15:39:d1:ce:03:ff:ef:5b:32:6d:5c:f5:2d:66:76:
            ae:70:5b:a8:f3:f0:34:fd:b6:cc:09:32:a7:02:78:
            e1:9c:0d:cd:c5:43:59:db:36:fc:ae:f3:61:4f:27:
            18
        public-key:
            75:e6:9b:4b:d2:6a:f3:6e:07:e2:0f:10:50:e9:fd:
            a2:4c:99:8b:88:59:85:e3:5a:a4:f6:82:72:c3:55:
            bc:94:b3:6c:08:d9:30:e8:15:33:42:cd:bb:7d:3b:
            8f:0f:82:b8:94:b9:de:60:34:cd:f0:61:00:3f:01:
            6a:19:04:f9:04:63:9d:06:94:5c:22:22:62:86:b8:
            21:5b:c4:2c:be:00:6a:ad:e3:a7:13:57:31:04:9c:
            a1:df:01:20:11:af:83:2e:2e:cd:9c:7b:06:2f:a4:
            ae:c5:31:01:8a:39:b5:e1:80:53:b0:fe:0c:00:6d:
            76:0c:9c:69:09:0c:44:98:c8:b5:7e:d1:ed:58:db:
            39:05:b8:c0:71:20:64:1f:71:3b:b3:69:5b:9b:3c:
            11:3d:ce:db:dc:2f:72:2d:ee:4d:75:8c:e5:07:f1:
            ad:e1:08:a9:62:25:ba:67:13:83:00:85:97:d2:74:
            be:ea:1e:cd:11:4e:bb:4c:e1:01:99:3f:5a:86:69:
            5e:23:ec:6f:a8:ce:58:f7:3d:cf:50:7f:7d:08:0c:
            b7:74:47:f7:5e:76:89:fb:97:44:05:53:16:98:2a:
            d7:ca:38:c1:b2:a1:3d:6e:32:38:46:95:f2:fd:6e:
            6f:a9:be:47:78:68:04:37:52:b8:4f:b0:a7:f3:8d:
            9c
        prime:
            00:fc:9e:c2:5c:b8:cd:68:e6:68:2f:d4:0d:92:19:
            4f:23:c8:86:8e:e2:35:85:52:c7:28:2c:5d:2c:fe:
            4b:84:ba:aa:3d:f3:a2:28:b0:44:f3:05:3c:2e:08:
            80:3a:21:fb:8c:c5:b8:05:fa:21:ba:80:91:1b:b1:
            3c:c5:69:e9:96:dd:51:9f:54:80:8a:26:73:75:e2:
            8a:63:8b:74:f1:f8:82:02:63:b7:11:41:e3:b3:15:
            cf:c1:5e:77:ec:86:db:37:67:76:31:1c:48:a1:d0:
            a5:6b:d9:c8:c7:fc:7d:d3:a7:e8:5d:f8:69:af:89:
            a2:de:4f:9d:7e:0e:65:bf:fd:87:3a:78:d2:4f:4a:
            91:35:c7:30:8e:a1:fe:93:7c:9e:d3:76:e1:b8:1e:
            83:f6:91:0b:da:9a:d1:ac:3c:67:d7:7a:b8:50:6a:
            21:d1:4c:29:ff:07:31:1d:09:35:b3:2e:a8:ba:f6:
            9a:10:96:26:38:a7:a6:c5:65:59:d7:d1:0f:3a:92:
            15:c6:f8:15:4d:ad:1e:db:cc:fb:ce:50:cf:ff:df:
            b1:37:b7:c2:bb:8e:88:ae:fd:12:59:5c:5a:d8:99:
            c0:a2:03:f7:d3:ca:e4:f3:10:20:79:12:40:e2:54:
            4a:38:08:05:6f:67:49:65:23:53:eb:9c:35:f5:27:
            81:ab
        generator: 2 (0x2)

Now **Bob**, needs to do the same thing:

    openssl genpkey -paramfile dhparams.pem -out bob_dh_key.pem

After this, **Bob** and **Alice** need to exchange their public keys. These need to get extracted from their key files. 

So, **Alice** does:

    openssl pkey -in alice_dh_key.pem -pubout -out alice_dh_public.pem

And **Bob**, does:

    openssl pkey -in bob_dh_key.pem -pubout -out bob_dh_public.pem

Now, **Alice** can send `alice_dh_public.pem` to **Bob**, and **Bob** can send `bob_dh_public.pem` to **Alice**.

Finally, both **Alice** and **Bob**, can compute a common secret key that can be used to exchange encrypted information. **Alice**, needs to derive its key:

    openssl pkeyutl -derive -inkey alice_dh_key.pem -peerkey bob_dh_public.pem -out alice_dh_common_key.bin

And **Bob**, can do the same:

    openssl pkeyutl -derive -inkey bob_dh_key.pem -peerkey alice_dh_public.pem -out bob_dh_common_key.bin

If we campare them both, they are equal.

    hexdump -C alice_dh_common_key.bin

Here are the contents of the key:

    00000000  c7 5f cf 8d 1e 00 6f 54  7b cc 7d 0c 85 d0 99 15  |�_�...oT{�}..�..|
    00000010  78 47 bc 32 fa 09 67 73  15 b5 9e 42 e6 e0 13 3c  |xG�2�.gs.�.B��.<|
    00000020  d7 d3 1c 17 a1 ef 45 1f  4b 2f 65 b0 d0 87 f9 d0  |��..��E.K/e��.��|
    00000030  2e 99 fe 66 91 fe e3 c1  01 2a b0 e2 d5 e1 a7 4e  |..�f.���.*����N|
    00000040  b8 8c 97 e1 c4 ec 29 58  41 06 77 50 f4 95 23 fb  |�..���)XA.wP�.#�|
    00000050  13 29 01 43 22 05 8a db  38 1a f8 21 30 8c f8 f7  |.).C"..�8.�!0.��|
    00000060  22 25 7b 88 89 7a 05 0c  b5 fa 30 ce 61 2c c6 e4  |"%{..z..��0�a,��|
    00000070  24 ca 23 e2 78 05 13 b3  2e 13 a9 2c e4 19 d7 eb  |$�#�x..�..�,�.��|
    00000080  be 8a 61 b4 d1 20 df ed  9e a2 31 89 4b b0 d5 6b  |�.a�� ��.�1.K��k|
    00000090  ac d8 8a 01 46 1a cc b9  fc aa e0 68 6d 82 35 52  |��..F.̹��hm.5R|
    000000a0  45 c5 10 6d bd 07 ac a1  a9 4f 3f 23 12 e3 d8 4e  |E�.m�.���O?#.��N|
    000000b0  0e e9 18 04 68 da b4 93  5d e0 86 c0 ad 1f d6 fc  |.�..hڴ.]�.��.��|
    000000c0  d7 e1 70 64 77 96 6b 83  57 77 47 5a 60 1d 45 92  |��pdw.k.WwGZ`.E.|
    000000d0  70 46 67 4d 3a f8 64 d7  ad f9 db a4 0d 92 cf e6  |pFgM:�d׭�ۤ..��|
    000000e0  d0 43 86 be b6 2e 65 08  95 dc 7d d9 6d 0c 03 b9  |�C.��.e..�}�m..�|
    000000f0  cb 2a 46 5e 6a 78 36 2f  51 ff ba b5 7a 17 45 33  |�*F^jx6/Q���z.E3|
    00000100

### Encrypt and decrypt with DH

Now that we have a common key between **Alice** and **Bob**, this key can be used to do symmetric cryptography between the two parties communicating.
To encrypt data, we can use:

    openssl aes-256-ecb -base64 -kfile alice_dh_common_key.bin -e -in plain.txt -out cipher.txt -pbkdf2

To decrypt data, we can do:

    openssl aes-256-ecb -base64 -kfile dh_common_key.bin -d -in cipher.txt -out clear.txt -pbkdf2

## Digital signatures

In this example we will see how you can use OpenSSL to generate and verify digital signatures.

### Generate a digital signature for a file

To generate the digital signature of a file, we will perform the following operations:
1.	Create a key pair and save it to a file:
    
        openssl genrsa -out ./keypair.pem 4096

2.	Extract the public key from the key pair:

        openssl rsa -in ./keypair.pem -out ./publickey.pem -outform PEM -pubout

3.	Generate a hash of a file we want to sign:

        openssl dgst -ripemd160 < ./snowden.jpg > ./hash1

4.	Using the private key, sign the hash and save it to a file:
    
        openssl rsautl -sign -inkey ./keypair.pem -keyform PEM -in ./hash1 > signature

### Verify the digital signature of a file

To check the previously generated subscription, you need to perform the following operations:

1.	Create a hash of the file whose signature we want to verify:
        
        openssl dgst -ripemd160 < ./snowden.jpg > ./hash2
2.	Verify the digital signature using the public key:
        
        openssl rsautl -verify -inkey ./publickey.pem -keyform PEM -pubin -in ./signature > verified
3.	Verify the difference between the original verified hash and the newly generated hash:

        diff -s ./verified ./hash2 
        Files ./verified and ./hash2 are identical

## Digital certificates
 
OpenSSL is also an excellent tool for creating Certificate Authorities and generating digital certificates. In this part we will see how we can create a Certificate Authority, generate requests for digital certificates and issue those digital certificates. On most of this commands, we are going to use the [openssl.cnf](openssl.cnf) file. This `openssl.cnf` file is usually distributed with the Openssl distribution.

### Create a certification authority

To create our own Certificate Authority (CA) we will create a folder structure that contains a series of CA information.

    mkdir private certs newcerts crl

Next we create two files that serve as a database for storing some of CA's supporting information:

    touch index.txt

    echo '01' > serial

And we will create the application for the certificate itself based on a set of characteristics:
- We will use the OpenSSL configuration file (it will come with your OpenSSL installation)
- The output will be generated in an X.509 structure
- We specify what a set of extensions will contain
- We specify where the key pair and certificate will be stored
- We specify that the certificate will be generated for a period of 5 years (1825 days)

Use this command (parts of the command, depend on the configuration present in `openssl.cnf`, namely the `-extensions`):

    openssl req -config ./openssl.cnf -new -x509 -extensions v3_ca -keyout private/ca.key -out certs/ca.crt -days 1825


And then request the necessary information:

    Generating a 1024 bit RSA private key
    ..++++++
    .......++++++
    writing new private key to 'private/ca.key'
    Enter PEM pass phrase:
    Verifying - Enter PEM pass phrase:
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:PT
    State or Province Name (full name) [Some-State]:Lisboa
    Locality Name (eg, city) []:Lisboa
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:ISCTE-IUL
    Organizational Unit Name (eg, section) []:ISTA
    Common Name (e.g. server FQDN or YOUR name) []:SRSI
    Email Address []:

After this, the certificate will be produced.

### Verify the generated digital certificate

We can check the generated (self-signed) digital certificate by viewing the X.509 structure.

    openssl x509 -noout -text -in certs/ca.crt

This is the structure of the certificate:

    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 10561673065884090221 (0x929299b1fdd4ff6d)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=PT, ST=Lisboa, L=Lisboa, O=ISCTE-IUL, OU=ISTA, CN=SRSI
            Validity
                Not Before: Nov  4 17:42:17 2015 GMT
                Not After : Nov  2 17:42:17 2020 GMT
            Subject: C=PT, ST=Lisboa, L=Lisboa, O=ISCTE-IUL, OU=ISTA, CN=SRSI
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (1024 bit)
                    Modulus:
                        00:cf:fd:f5:8b:4f:29:81:a4:de:38:bf:65:c5:8f:
                        5a:7b:23:db:6a:bd:89:db:9d:eb:c2:64:2e:86:89:
                        04:b0:61:47:2e:88:4e:85:06:22:76:d8:94:bd:f0:
                        f8:79:f3:40:11:8b:4b:6d:77:d8:f6:6a:40:d9:c5:
                        c1:72:b9:ca:71:5a:ad:48:75:4d:9f:5c:7d:34:c2:
                        4e:5c:e7:fc:e5:24:28:90:6f:07:61:97:b0:90:31:
                        a9:2f:9d:20:fd:0b:eb:60:1c:99:5d:f3:6c:c5:ba:
                        41:f4:7c:35:64:55:06:b8:f3:7d:d9:b7:65:22:0a:
                        b3:df:04:9b:86:e5:89:6c:b3
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Subject Key Identifier: 
                    CC:A9:43:88:06:8F:D4:9D:35:40:96:D9:2B:76:86:D7:39:8B:1D:43
                X509v3 Authority Key Identifier: 
                    keyid:CC:A9:43:88:06:8F:D4:9D:35:40:96:D9:2B:76:86:D7:39:8B:1D:43

                X509v3 Basic Constraints: 
                    CA:TRUE
        Signature Algorithm: sha256WithRSAEncryption
            58:c2:5c:6d:cf:d8:f6:af:aa:94:42:62:a9:87:8e:47:24:4e:
            f3:13:34:08:15:62:ac:92:f6:fc:69:ba:2c:68:22:f7:88:81:
            b7:d8:ab:d6:a0:70:31:54:6b:1b:79:55:13:6a:50:c9:cc:9e:
            34:06:3f:dc:99:d5:17:3f:0b:ed:e7:20:22:06:6b:b7:6e:da:
            a9:23:3e:c0:8c:fa:e9:37:58:d4:6f:e7:07:4c:6f:61:3b:66:
            e3:11:61:25:4d:23:bc:c5:e3:3a:4b:c5:48:81:2c:f0:6a:07:
            a4:9e:c7:35:c4:68:e1:5c:cd:1d:7d:79:fc:3f:e1:6d:6e:2d:
            bf:8f

### Generate a digital certificate request

Let's now start operating the CA as if it were a real CA. One of the important steps is that when someone tries to request a certificate from a CA they need to create a Certificate Signing Request (CSR). So we are going to create that.

In this specific case we chose not to protect the private key with a password (`-nodes`), which is not recommended in a production environment. On the other hand, the request will be made for one year (365 days).

    openssl req -config ./openssl.cnf -new -nodes -keyout ./private/server.key -out ./server.csr -days 365

Then supply the request information:

    Generating a 1024 bit RSA private key
    ....++++++
    ...................++++++
    writing new private key to './private/server.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:PT
    State or Province Name (full name) [Some-State]:Lisboa
    Locality Name (eg, city) []:Lisboa
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:ISCTE-IUL
    Organizational Unit Name (eg, section) []:ISTA
    Common Name (e.g. server FQDN or YOUR name) []:SRSI
    Email Address []:

    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:

We can verify the request using the following command:

    openssl req -noout -text -in ./server.csr 

Check the request that is about to be made:

    Certificate Request:
        Data:
            Version: 0 (0x0)
            Subject: C=PT, ST=Lisboa, L=Lisboa, O=ISCTE-IUL, OU=ISTA, CN=SRSI
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (1024 bit)
                    Modulus:
                        00:d6:b2:31:8b:52:36:1c:c6:19:05:ee:ee:9f:00:
                        f2:fa:06:92:86:41:01:58:73:7c:b3:d0:23:b2:64:
                        75:2c:64:19:7a:a1:ba:5d:5a:74:69:a3:cf:66:2d:
                        7c:9b:eb:9e:5b:0f:36:8d:f6:7f:06:7c:47:51:fd:
                        f0:20:41:fd:ca:1e:ca:6a:d4:70:b7:a7:e9:27:33:
                        e6:0d:18:9e:1c:0b:3c:3a:24:3d:88:9d:d1:ae:d0:
                        a0:9d:6f:ca:c9:f7:70:e4:a3:a7:67:4b:1d:d3:80:
                        f4:b2:e8:79:31:4a:75:3b:93:6f:0b:f1:84:a7:dc:
                        34:1e:20:40:8c:e1:40:56:53
                    Exponent: 65537 (0x10001)
            Attributes:
                a0:00
        Signature Algorithm: sha256WithRSAEncryption
            b0:03:ca:65:4e:c3:97:ce:e0:96:e9:1f:3a:da:6b:d8:a7:07:
            9e:5a:08:32:0e:f4:21:78:43:50:b5:f9:c4:dc:97:db:e9:a5:
            9c:ad:70:83:47:dc:25:9a:03:34:92:a2:db:d9:a6:f6:80:65:
            3f:8c:62:29:d7:89:40:a4:97:9e:f6:78:aa:cb:3d:12:53:6e:
            a3:5e:03:93:d2:9b:07:57:24:cb:fb:a3:99:7f:7b:96:d0:e7:
            b4:65:54:29:fb:2c:39:70:d0:6f:1b:66:a1:fb:e8:34:4f:db:
            e3:00:09:2d:41:45:3b:df:de:00:cb:cb:7f:8c:fd:a9:9a:22:
            fb:6f

Before issuing the certificate (signing it with its own private key), a CA must check the contents of the CSR.

### Issuance of the digital certificate

This step is important because it is performed by a CA to issue a digital certificate to an entity that requests it. To generate this digital certificate, the previously generated CSR must be used.

    openssl ca -config ./openssl.cnf -policy policy_anything -out certs/server.crt -infiles server.csr

This is the issuance process:

    Using configuration from ./openssl.cnf
    Enter pass phrase for ./private/ca.key:
    Check that the request matches the signature
    Signature ok
    Certificate Details:
            Serial Number: 1 (0x1)
            Validity
                Not Before: Nov  4 18:44:27 2015 GMT
                Not After : Nov  3 18:44:27 2016 GMT
            Subject:
                countryName               = PT
                stateOrProvinceName       = Lisboa
                localityName              = Lisboa
                organizationName          = ISCTE-IUL
                organizationalUnitName    = ISTA
                commonName                = SRSI
            X509v3 extensions:
                X509v3 Basic Constraints: 
                    CA:FALSE
                Netscape Comment: 
                    OpenSSL Generated Certificate
                X509v3 Subject Key Identifier: 
                    95:DD:D8:EF:E5:83:92:44:CD:67:3A:0F:C8:F0:8B:0F:23:D7:9D:C3
                X509v3 Authority Key Identifier: 
                    keyid:CC:A9:43:88:06:8F:D4:9D:35:40:96:D9:2B:76:86:D7:39:8B:1D:43

    Certificate is to be certified until Nov  3 18:44:27 2016 GMT (365 days)
    Sign the certificate? [y/n]:y


    1 out of 1 certificate requests certified, commit? [y/n]y
    Write out database with 1 new entries
    Data Base Updated

To confirm that the certificate was issued correctly, we can view it using the command we saw earlier:

    openssl x509 -noout -text -in ./certs/server.crt 

And look at the certificate data:

    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 1 (0x1)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=PT, ST=Lisboa, L=Lisboa, O=ISCTE-IUL, OU=ISTA, CN=SRSI
            Validity
                Not Before: Nov  4 18:44:27 2015 GMT
                Not After : Nov  3 18:44:27 2016 GMT
            Subject: C=PT, ST=Lisboa, L=Lisboa, O=ISCTE-IUL, OU=ISTA, CN=SRSI
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (1024 bit)
                    Modulus:
                        00:d6:b2:31:8b:52:36:1c:c6:19:05:ee:ee:9f:00:
                        f2:fa:06:92:86:41:01:58:73:7c:b3:d0:23:b2:64:
                        75:2c:64:19:7a:a1:ba:5d:5a:74:69:a3:cf:66:2d:
                        7c:9b:eb:9e:5b:0f:36:8d:f6:7f:06:7c:47:51:fd:
                        f0:20:41:fd:ca:1e:ca:6a:d4:70:b7:a7:e9:27:33:
                        e6:0d:18:9e:1c:0b:3c:3a:24:3d:88:9d:d1:ae:d0:
                        a0:9d:6f:ca:c9:f7:70:e4:a3:a7:67:4b:1d:d3:80:
                        f4:b2:e8:79:31:4a:75:3b:93:6f:0b:f1:84:a7:dc:
                        34:1e:20:40:8c:e1:40:56:53
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Basic Constraints: 
                    CA:FALSE
                Netscape Comment: 
                    OpenSSL Generated Certificate
                X509v3 Subject Key Identifier: 
                    95:DD:D8:EF:E5:83:92:44:CD:67:3A:0F:C8:F0:8B:0F:23:D7:9D:C3
                X509v3 Authority Key Identifier: 
                    keyid:CC:A9:43:88:06:8F:D4:9D:35:40:96:D9:2B:76:86:D7:39:8B:1D:43

        Signature Algorithm: sha256WithRSAEncryption
            1e:5c:02:31:3e:ca:c7:05:0a:84:5c:9f:c1:5c:49:8d:0d:15:
            25:cd:15:da:73:b6:73:7e:ee:6a:b7:bd:6f:7e:b8:5e:f0:44:
            84:7f:ab:eb:58:88:92:27:7e:38:77:a8:ec:fc:c5:f4:43:c5:
            cc:5b:4b:7e:53:6c:08:65:5c:aa:61:1e:92:36:33:f6:41:f4:
            91:97:8b:d6:24:8b:db:79:88:96:6c:06:e0:f9:41:ce:4d:5b:
            d5:3c:5d:e6:73:75:7b:87:02:55:c8:22:ae:15:65:1c:56:aa:
            1e:49:93:5c:b2:82:01:c6:91:9f:bd:16:2e:0c:ff:a9:b8:6a:
            92:91

### Revocation of digital certificates

One of the functions of a CA is also to maintain information about the digital certificates it issues. Whenever a certificate, for whatever reason, is no longer valid, it must be revoked. One of the ways to revoke and communicate this revocation to "third parties" is through a Certificate Revocation List (CRL).

To revoke a certificate, using OpenSSl, we can do the following:

    openssl ca -config ./openssl.cnf -revoke certs/server.crt

And the proper certificate gets revoked:

    Using configuration from ./openssl.cnf
    Enter pass phrase for ./private/ca.key:
    Revoking Certificate 01.
    Data Base Updated

The certificate is thus revoked and to switch to the CRL the following must be done:
    
    openssl ca -config ./openssl.cnf -gencrl -out crl/ca.crl

To view the certificates that are part of the CRL, we can do the following:

    openssl crl -in crl/ca.crl -noout -text

Look at the revocationlist of the CA:

    Certificate Revocation List (CRL):
            Version 1 (0x0)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: /C=PT/ST=Lisboa/L=Lisboa/O=ISCTE-IUL/OU=ISTA/CN=SRSI
            Last Update: Nov  4 18:56:19 2015 GMT
            Next Update: Dec  4 18:56:19 2015 GMT
    Revoked Certificates:
        Serial Number: 01
            Revocation Date: Nov  4 18:54:37 2015 GMT
        Signature Algorithm: sha256WithRSAEncryption
            84:a1:ec:46:ce:5a:7d:52:bb:bc:79:ad:de:b3:ff:5b:c2:d6:
            4c:41:db:27:68:79:c4:e6:33:cb:17:2b:6b:25:f9:c5:c8:eb:
            7c:1d:06:94:9f:44:b3:8f:0d:64:27:2a:08:5c:05:10:c9:3d:
            40:e5:67:3b:70:ff:50:13:41:0e:fc:f0:da:7a:69:9d:c1:8c:
            97:2b:7b:18:62:99:30:38:26:92:6e:7f:f3:de:b1:b4:14:91:
            31:f9:57:7a:be:f4:18:d9:0c:30:dd:4d:d2:8e:23:d2:0e:33:
            b7:69:9c:00:b8:4f:b7:3b:ba:35:c2:26:26:e6:fc:61:83:56:
            d8:1e

## OCSP - Online Certificate Status Protocol

[OCSP](https://www.ietf.org/rfc/rfc2560.txt) is an interactive way to enable applications to check for the revokation of an identified certificate. In order to test for OCSP validity, we require the certificate that we are going to test, the certificate chain of such certificate, and the OCSP responder address.

### Getting the server certificate

Get the server certificate that we want to check for OCSP validity:

    openssl s_client -connect www.akamai.com:443 < /dev/null 2>&1 |  sed -n '/-----BEGIN/,/-----END/p' > certificate.pem

Certificate will be store on the `certificate.pem` file.

### Getting the intermediate certificates

Getting the multiple intermediary certificates for an entity.

    openssl s_client -showcerts -connect www.akamai.com:443 < /dev/null 2>&1 |  sed -n '/-----BEGIN/,/-----END/p' > chain.pem

These certificates will be store in `chain.pem` file.

### Get the OCSP responder for a certificate

Now we need to obatin the address of the OCSP responder, to know where to connect for querying. This might be done through manual inspection, or by using the command:

    openssl x509 -noout -ocsp_uri -in certificate.pem

### Make the OCSP request

Finnaly we may do the request to the OCSP responder, using the following command:

    openssl ocsp -issuer chain.pem -cert certificate.pem -text -url http://ocsp.digicert.com

And we obtain the following result.

    OCSP Request Data:
        Version: 1 (0x0)
        Requestor List:
            Certificate ID:
            Hash Algorithm: sha1
            Issuer Name Hash: E4E395A229D3D4C1C31FF0980C0B4EC0098AABD8
            Issuer Key Hash: FCEA9148BBCE3C6AA8902FBDD1DE95038C40809C
            Serial Number: 0A1E4B89556FDE711D7BB2E2DFFF804F
        Request Extensions:
            OCSP Nonce:
                041007AEEFEBA331089CA9113F7E4646D525
    OCSP Response Data:
        OCSP Response Status: successful (0x0)
        Response Type: Basic OCSP Response
        Version: 1 (0x0)
        Responder Id: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
        Produced At: Nov  2 20:31:11 2022 GMT
        Responses:
        Certificate ID:
        Hash Algorithm: sha1
        Issuer Name Hash: E4E395A229D3D4C1C31FF0980C0B4EC0098AABD8
        Issuer Key Hash: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
        Serial Number: 0A1E4B89556FDE711D7BB2E2DFFF804F
        Cert Status: good
        This Update: Nov  2 20:15:01 2022 GMT
        Next Update: Nov  9 19:30:01 2022 GMT

        Signature Algorithm: sha256WithRSAEncryption
            9f:e5:42:ae:ea:a5:89:98:97:4d:7c:9b:77:e2:94:2d:63:46:
            3a:66:79:cb:3a:cd:f6:24:c9:56:39:97:e3:8d:a8:1d:42:c0:
            60:3b:12:ab:3c:b5:7a:3e:7c:ef:1e:0b:c9:97:aa:c2:42:79:
            59:fe:b4:b5:80:58:e7:d9:f5:3c:39:bd:1d:78:77:8a:b2:26:
            be:93:fc:bf:cb:92:9d:97:93:31:ca:dc:2b:a1:33:c3:e1:12:
            45:5f:82:2f:24:00:8f:99:a3:8b:5c:61:84:be:eb:4f:90:8a:
            48:31:43:f0:7d:93:8d:6d:16:6f:a6:42:c5:00:65:b9:26:e7:
            3d:8e:65:3d:0b:3e:2a:67:a4:fa:d7:6c:4b:f4:1c:6d:da:e0:
            17:8e:78:74:f9:4a:fe:4e:a1:6e:c5:c7:20:1e:cb:72:69:64:
            bf:f5:3b:6f:35:34:a7:5f:f3:54:af:3a:c2:42:f2:e8:a0:a5:
            cc:ad:84:9e:5a:cd:c6:2d:1b:47:7c:40:01:08:56:f9:80:5e:
            29:80:ac:0f:71:3b:a8:c8:5a:f6:e7:a0:dd:76:1e:e3:8a:93:
            d4:3d:57:7e:f3:30:97:e3:9d:d3:8a:fe:0b:6b:23:af:4b:8b:
            3e:4c:bf:e8:dc:b8:d2:61:3c:de:68:fa:ce:5c:d9:46:a2:f7:
            81:f5:51:e1
    WARNING: no nonce in response
    Response Verify Failure
    140704274896064:error:27FFF076:OCSP routines:CRYPTO_internal:signer certificate not found:/AppleInternal/Library/BuildRoots/810eba08-405a-11ed-86e9-6af958a02716/Library/Caches/com.apple.xbs/Sources/libressl/libressl-3.3/crypto/ocsp/ocsp_vfy.c:89:
    certificate.pem: ERROR: No Status found.

## S/MIME

S/MIME is a standard for encrypting and signing MIME (Multipurpose Internet Mail Extensions) data. It is widely used for electronic mail. OpenSSL provides support for this type of functionality.

For the next operations let's assume that there are two entities, **Alice** and **Bob** who want to exchange and sign information using public key cryptography - to do this we will create key pairs and certificates (containing the public key) for each of them.

### Create keys and certificates

Let's create a set of key pairs and certificates for **Alice** and **Bob**.

#### Create key pairs and CSR for certificate request

Create a CSR for Alice:

    openssl req -config ./openssl.cnf -new -nodes -keyout ./alice.key -out ./alice.csr -days 365

Create a CSR for Bob:
    
    openssl req -config ./openssl.cnf -new -nodes -keyout ./bob.key -out ./bob.csr -days 365

#### Request and issue the certificates

The following command is going to be used to issue the certificate for **Alice**:

    openssl ca -config ./openssl.cnf -policy policy_anything -out alice.crt -infiles alice.csr

Results in the following:

    Using configuration from ./openssl.cnf
    Enter pass phrase for ./private/ca.key:
    Check that the request matches the signature
    Signature ok
    Certificate Details:
            Serial Number: 2 (0x2)
            Validity
                Not Before: Nov  5 08:44:07 2015 GMT
                Not After : Nov  4 08:44:07 2016 GMT
            Subject:
                countryName               = PT
                stateOrProvinceName       = Lisboa
                localityName              = Lisboa
                organizationName          = Internet Widgits Pty Ltd
                commonName                = Alice
            X509v3 extensions:
                X509v3 Basic Constraints: 
                    CA:FALSE
                Netscape Comment: 
                    OpenSSL Generated Certificate
                X509v3 Subject Key Identifier: 
                    BA:85:F1:09:B1:72:0C:0E:3D:93:49:34:C0:16:83:72:34:8E:CE:F4
                X509v3 Authority Key Identifier: 
                    keyid:CC:A9:43:88:06:8F:D4:9D:35:40:96:D9:2B:76:86:D7:39:8B:1D:43

    Certificate is to be certified until Nov  4 08:44:07 2016 GMT (365 days)
    Sign the certificate? [y/n]:y


    1 out of 1 certificate requests certified, commit? [y/n]y
    Write out database with 1 new entries
    Data Base Updated

The following command is going to be used to issue the certificate for **Bob**:

    openssl ca -config ./openssl.cnf -policy policy_anything -out bob.crt -infiles bob.csr

Results in the following:

    Using configuration from ./openssl.cnf
    Enter pass phrase for ./private/ca.key:
    Check that the request matches the signature
    Signature ok
    Certificate Details:
            Serial Number: 3 (0x3)
            Validity
                Not Before: Nov  5 08:45:14 2015 GMT
                Not After : Nov  4 08:45:14 2016 GMT
            Subject:
                countryName               = PT
                stateOrProvinceName       = Lisboa
                localityName              = Lisboa
                organizationName          = Internet Widgits Pty Ltd
                commonName                = Bob
            X509v3 extensions:
                X509v3 Basic Constraints: 
                    CA:FALSE
                Netscape Comment: 
                    OpenSSL Generated Certificate
                X509v3 Subject Key Identifier: 
                    C3:F6:E7:E8:11:49:09:A6:DC:64:69:3E:73:BB:54:54:37:2B:3F:38
                X509v3 Authority Key Identifier: 
                    keyid:CC:A9:43:88:06:8F:D4:9D:35:40:96:D9:2B:76:86:D7:39:8B:1D:43

    Certificate is to be certified until Nov  4 08:45:14 2016 GMT (365 days)
    Sign the certificate? [y/n]:y


    1 out of 1 certificate requests certified, commit? [y/n]y
    Write out database with 1 new entries
    Data Base Updated

### Encrypt MIME information

**Alice** will now encrypt a file (`SecretMessage.txt`) that she wants to send securely to **Bob**, using the public key contained in **Bob**'s certificate (`bob.crt`) (which will have *been obtained at some earlier ti*me).

    openssl smime -encrypt -aes128 -in ./mensagemSecreta.txt -out ./mensagemSecreta.txt.enc -outform PEM ./bob.crt

The result of the cipher is as follows:

    -----BEGIN PKCS7-----
    MIIBmAYJKoZIhvcNAQcDoIIBiTCCAYUCAQAxggEAMIH9AgEAMGYwYTELMAkGA1UE
    BhMCUFQxDzANBgNVBAgMBkxpc2JvYTEPMA0GA1UEBwwGTGlzYm9hMRIwEAYDVQQK
    DAlJU0NURS1JVUwxDTALBgNVBAsMBElTVEExDTALBgNVBAMMBFNSU0kCAQMwDQYJ
    KoZIhvcNAQEBBQAEgYBHwxlei9Ww5CxBQzxgTt5mMw8NOm0TshvRKke1yAPWFt10
    PjQ+9AM4Szr2YD2q93ziMO0fqveSJ82jjeL2TeFjjfJKo4ejycuaCggFSvyiNEou
    s3LFcgSUq0Cu48JmNBxYOcZHdo0N/xCSW2ZPwUES6JbX49kmd5OaFQoRUlmy1TB8
    BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAECBBC2Lg/h53AjIOgcSUy4xYc/gFCzmQPi
    CGQU81M+3/TvAOAroNXNi6dVWxchNcm1JrFjPzl+/quZobR1MXUNO6YnHaZ4yGge
    D+NizD2C2tg+VGXUowtXlyHvmEy7KjmXmq1dCQ==
    -----END PKCS7-----

### Decrypt MIME information

**Bob**, after receiving the encrypted file (messageSecreta.txt.enc), will use his private key (`bob.key`), to decrypt the original content.

    openssl smime -decrypt -aes128 -in ./mensagemSecreta.txt.enc -out ./mensagemSecreta.txt.orig -inform PEM -inkey ./bob.key 

### Sign MIME information

**Alice**, wants to sign the message and then send it **to** Bob. To do this, she does the following:

    openssl smime -sign -in ./mensagemSecreta.txt -out ./mensagemsecreta.sig -signer ./alice.crt -inkey alice.key

The result of signing the message is as follows:

    MIME-Version: 1.0
    Content-Type: multipart/signed; protocol="application/x-pkcs7-signature"; micalg="sha-256"; boundary="----F46C4B7E272BA661D77AA4FF41655372"

    This is an S/MIME signed message

    ------F46C4B7E272BA661D77AA4FF41655372
    Este é o meu email secreto que eu pretendo assinar digitalmente!

    ------F46C4B7E272BA661D77AA4FF41655372
    Content-Type: application/x-pkcs7-signature; name="smime.p7s"
    Content-Transfer-Encoding: base64
    Content-Disposition: attachment; filename="smime.p7s"

    MIIE6wYJKoZIhvcNAQcCoIIE3DCCBNgCAQExDzANBglghkgBZQMEAgEFADALBgkq
    hkiG9w0BBwGgggK4MIICtDCCAh2gAwIBAgIBAjANBgkqhkiG9w0BAQsFADBhMQsw
    CQYDVQQGEwJQVDEPMA0GA1UECAwGTGlzYm9hMQ8wDQYDVQQHDAZMaXNib2ExEjAQ
    BgNVBAoMCUlTQ1RFLUlVTDENMAsGA1UECwwESVNUQTENMAsGA1UEAwwEU1JTSTAe
    Fw0xNTExMDUwODQ0MDdaFw0xNjExMDQwODQ0MDdaMGIxCzAJBgNVBAYTAlBUMQ8w
    DQYDVQQIDAZMaXNib2ExDzANBgNVBAcMBkxpc2JvYTEhMB8GA1UECgwYSW50ZXJu
    ZXQgV2lkZ2l0cyBQdHkgTHRkMQ4wDAYDVQQDDAVBbGljZTCBnzANBgkqhkiG9w0B
    AQEFAAOBjQAwgYkCgYEA2RSZZp0l7ZeH1PDTLelHT9KBOxTri1re9dvMTpbsfMkq
    XZim/CXv2UGp9k4DxxIsSbqoARxajK8t81zvBh/ttbZjgT1MTi3K0ywpVNqfYKR2
    UKkaujfqlqF5tjErf09tzXNZcCr5hZtYYOiCLF/ThSGkOvTJDLH+TO64GkoIonEC
    AwEAAaN7MHkwCQYDVR0TBAIwADAsBglghkgBhvhCAQ0EHxYdT3BlblNTTCBHZW5l
    cmF0ZWQgQ2VydGlmaWNhdGUwHQYDVR0OBBYEFLqF8QmxcgwOPZNJNMAWg3I0js70
    MB8GA1UdIwQYMBaAFMypQ4gGj9SdNUCW2St2htc5ix1DMA0GCSqGSIb3DQEBCwUA
    A4GBAGunwN3AvSx3fRk7qLIoypaN/WD/AgJE5z6qA1h6fEjsrJQPBUqcjPWtpPOe
    NKBE5Q45zPchRW2dMXdWQwW4dCB0nWoAVCjxFHnUMWMUjWWmHG8p7n9xb85H+6lP
    BrPSLWAgQPAbrADnud79NpRK2f13QLFc3jP7JyewTQAWXTNYMYIB9zCCAfMCAQEw
    ZjBhMQswCQYDVQQGEwJQVDEPMA0GA1UECAwGTGlzYm9hMQ8wDQYDVQQHDAZMaXNi
    b2ExEjAQBgNVBAoMCUlTQ1RFLUlVTDENMAsGA1UECwwESVNUQTENMAsGA1UEAwwE
    U1JTSQIBAjANBglghkgBZQMEAgEFAKCB5DAYBgkqhkiG9w0BCQMxCwYJKoZIhvcN
    AQcBMBwGCSqGSIb3DQEJBTEPFw0xNTExMDUxMDEzMzhaMC8GCSqGSIb3DQEJBDEi
    BCCTTSIGPJF42yCzx4CWYZyjbuqxG61TBWA4hARuTLR/WTB5BgkqhkiG9w0BCQ8x
    bDBqMAsGCWCGSAFlAwQBKjALBglghkgBZQMEARYwCwYJYIZIAWUDBAECMAoGCCqG
    SIb3DQMHMA4GCCqGSIb3DQMCAgIAgDANBggqhkiG9w0DAgIBQDAHBgUrDgMCBzAN
    BggqhkiG9w0DAgIBKDANBgkqhkiG9w0BAQEFAASBgHfqoRy3pJi+BIIYNAhDou+B
    Y9WmcXeP5z7t7UCIe3FH0HHrdveCNk7G2TYeYCPHEj+6uJORxGc4WCF/SsM9kbRF
    U1CPxJuwzHfkIaWB05yCk5H6eRv85ZRG10wgMaXk2wVoDunshjvW1vMjS1sYxN6v
    vAqIZU/Pj/A/LaN79Lsc

    ------F46C4B7E272BA661D77AA4FF41655372--

### Verify digital signature of MIME information

After receiving the message signed by **Alice** (`mensagemsecreta.sig`), **Bob** uses **Alice**'s digital certificate. In this case, and to prevent OpenSSL from doing additional validations on the digital certificate (such as checking whether it was actually issued by a CA it trusts) the "`-noverify`" option is used.

    openssl smime -verify -in ./mensagemsecreta.sig -signer ./alice.crt -noverify

Resulting in:

    Este é o meu email secreto que eu pretendo assinar digitalmente!
    Verification successful

## Transport Layer Security/ Secure Sockets Layer (SSL/TLS)

Secure Sockets Layer (SSL) is a widely used protocol for secure and authenticated connections between Web browsers and Web servers.

### Check a SSL/TLS connection to a server

OpenSSL can be used to check the parameters of an SSL connection to a particular server, for example:

    openssl s_client -connect www.microsoft.com:443

Resulting in:

    CONNECTED(00000005)
    depth=2 C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root G2
    verify return:1
    depth=1 C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06
    verify return:1
    depth=0 C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = www.microsoft.com
    verify return:1
    ---
    Certificate chain
    0 s:C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = www.microsoft.com
    i:C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06
    1 s:C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06
    i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root G2
    ---
    Server certificate
    -----BEGIN CERTIFICATE-----
    MIII1jCCBr6gAwIBAgITMwBZ+Lbaholwb/ob2QAAAFn4tjANBgkqhkiG9w0BAQwF
    ADBZMQswCQYDVQQGEwJVUzEeMBwGA1UEChMVTWljcm9zb2Z0IENvcnBvcmF0aW9u
    MSowKAYDVQQDEyFNaWNyb3NvZnQgQXp1cmUgVExTIElzc3VpbmcgQ0EgMDYwHhcN
    MjIxMDA0MjMyMzExWhcNMjMwOTI5MjMyMzExWjBoMQswCQYDVQQGEwJVUzELMAkG
    A1UECBMCV0ExEDAOBgNVBAcTB1JlZG1vbmQxHjAcBgNVBAoTFU1pY3Jvc29mdCBD
    b3Jwb3JhdGlvbjEaMBgGA1UEAxMRd3d3Lm1pY3Jvc29mdC5jb20wggEiMA0GCSqG
    SIb3DQEBAQUAA4IBDwAwggEKAoIBAQC3XyMTbW+b+oTRRkirpEjsH/JiF2Dsy8Sj
    KGuCT5SSCO6qfDatzbUFzvMn7TFFAyOEWQhjp5vpAu3zZTu9jeezuXXJx1+2Ey15
    VnZJV3Q78zwJJ/3yTbiXfzhqYWr31Nm3boFzKZWZGZDHgY7epAdYwlOdLQk4QRHm
    3aJNOJWCgc1prSd/XgP+q9sYOnAYVRv9tGtkznTX6QaS82jWjDH8zHi299K8YQpk
    1aophTBJAitrpmBFSHvDgwMpurpx0rjAvq1Q27mTAWG07LMd7J7nyZiFzUxmFDDq
    2ix220yPEmL+wlz1dSqwNaIZi7f67Q9aQ5GnE0A5IPtK0eYp1AbvAgMBAAGjggSG
    MIIEgjCCAX8GCisGAQQB1nkCBAIEggFvBIIBawFpAHcA6D7Q2j71BjUy51covIlr
    yQPTy9ERa+zraeF3fW0GvW4AAAGDpVinhgAABAMASDBGAiEAnc/ZJKQ47TEBYPPo
    qSeM5wRPGELQmAw0i9hYMHUxdukCIQCoQvNpRxMnNH9beub6RbBYSHAIUSbgq6wK
    OTdFbfFNdwB1ALNzdwfhhFD4Y4bWBancEQlKeS2xZwwLh9zwAw55NqWaAAABg6VY
    p8YAAAQDAEYwRAIgPYOx2FZTNMcINed7dn+d75khk9ZstsbyvwPW8V2xtisCICGg
    1wuFd+KukXW8HeDa3mkloREsifgzeTBMGpwVwToMAHcArfe++nz/EMiLnT2cHj4Y
    arRnKV3PsQwkyoWGNOvcgooAAAGDpVinVwAABAMASDBGAiEAttaYStY88Acj2OSc
    nvx1RdlmZRFau8/aMIK/IQEYBisCIQDN/1i/tTTawY67Q4qAFnj1Eh+0kIvwUkrO
    TEV58sFRVjAnBgkrBgEEAYI3FQoEGjAYMAoGCCsGAQUFBwMCMAoGCCsGAQUFBwMB
    MDwGCSsGAQQBgjcVBwQvMC0GJSsGAQQBgjcVCIe91xuB5+tGgoGdLo7QDIfw2h1d
    goTlaYLzpz4CAWQCASUwga4GCCsGAQUFBwEBBIGhMIGeMG0GCCsGAQUFBzAChmFo
    dHRwOi8vd3d3Lm1pY3Jvc29mdC5jb20vcGtpb3BzL2NlcnRzL01pY3Jvc29mdCUy
    MEF6dXJlJTIwVExTJTIwSXNzdWluZyUyMENBJTIwMDYlMjAtJTIweHNpZ24uY3J0
    MC0GCCsGAQUFBzABhiFodHRwOi8vb25lb2NzcC5taWNyb3NvZnQuY29tL29jc3Aw
    HQYDVR0OBBYEFGcNlofXWtjH6XjUmPrgobtxutaHMA4GA1UdDwEB/wQEAwIEsDCB
    mQYDVR0RBIGRMIGOghN3d3dxYS5taWNyb3NvZnQuY29tghF3d3cubWljcm9zb2Z0
    LmNvbYIYc3RhdGljdmlldy5taWNyb3NvZnQuY29tghFpLnMtbWljcm9zb2Z0LmNv
    bYINbWljcm9zb2Z0LmNvbYIRYy5zLW1pY3Jvc29mdC5jb22CFXByaXZhY3kubWlj
    cm9zb2Z0LmNvbTAMBgNVHRMBAf8EAjAAMGQGA1UdHwRdMFswWaBXoFWGU2h0dHA6
    Ly93d3cubWljcm9zb2Z0LmNvbS9wa2lvcHMvY3JsL01pY3Jvc29mdCUyMEF6dXJl
    JTIwVExTJTIwSXNzdWluZyUyMENBJTIwMDYuY3JsMGYGA1UdIARfMF0wUQYMKwYB
    BAGCN0yDfQEBMEEwPwYIKwYBBQUHAgEWM2h0dHA6Ly93d3cubWljcm9zb2Z0LmNv
    bS9wa2lvcHMvRG9jcy9SZXBvc2l0b3J5Lmh0bTAIBgZngQwBAgIwHwYDVR0jBBgw
    FoAU1cFnOsKjnfR3UltZEjgp5lVou6UwHQYDVR0lBBYwFAYIKwYBBQUHAwIGCCsG
    AQUFBwMBMA0GCSqGSIb3DQEBDAUAA4ICAQBxYv4ot1H2312MXlIvccktJlx1/zdq
    HvYlonJRgNH9Upyf69Mpd4bOvuPbV3GFf2kOYk2TrfMIsdgXVAsHhVJlGY/ybYkx
    GnYq3p16cTIDOO1sck352gM3y1n5BK21JMJL/pt1PYIgPnl3ED4DjaD1fucVT93B
    uvkW6N9er5YHQrXG6+gEPD4RZeXhtbIC7zMeBlpcbFG8pxp7vy2NmftNzIOaTkHw
    xT66cGEUgoPEt7ugcJr4nuq6OQ+m+wBZnv14GP+Rjcx6WXGeFIk4POG8XyEDKy22
    +EVODNuvHwWIBCKfxI8rZWf00CcLtVYupyUiAmoovM1TET/41sM2+sOxvTlNdxOL
    pnbVaeiDptMZKuQ0XkFshCJUZ/RhocrjsYNLANYbHoiaJT1lAqU7sTQgQocB34oY
    /Ojy975dBu3DnCebKaXEzjHtujLJ+1fMZ9uUbIP6/FwxL/UqvIpbpTnCoznvnpNE
    ii5x/Jh6Dg+ShpD2QyUE+1LvWhui9GNwm427cnRMHdxNGYHXAuFpGK8/ky3a849p
    xKqx4LNue0y/Rxd+s/EeJRmhF/mYcxzar7G/6T6TjHcjhIR7NTC3y7Q49L3LQdOx
    OtahCotJbc95k+AXudsfPlVLKsYJ6AF9NxN4K2miTmNqz+tmGiKoqbebOTHsd62i
    AstzF05URX9Y4w==
    -----END CERTIFICATE-----
    subject=C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = www.microsoft.com

    issuer=C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06

    ---
    No client certificate CA names sent
    Peer signing digest: SHA256
    Peer signature type: RSA-PSS
    Server Temp Key: X25519, 253 bits
    ---
    SSL handshake has read 4368 bytes and written 399 bytes
    Verification: OK
    ---
    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
    Server public key is 2048 bit
    Secure Renegotiation IS NOT supported
    Compression: NONE
    Expansion: NONE
    No ALPN negotiated
    Early data was not sent
    Verify return code: 0 (ok)
    ---
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: 79E4C98601A3400F12807C54D26202A9145A2205ECE8A9ECCF35B82D5391C3C8
        Session-ID-ctx:
        Resumption PSK: BB1D8D7E0BD3B11DB702120DDB89809AA71CDC723FF9912D86142E38BA243E64C5E8FD2EA71A00FB954E5E3475703B1D
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 83100 (seconds)
        TLS session ticket:
        0000 - 00 00 35 6e 11 51 4f da-c0 0e 4e bb 71 b8 9b 41   ..5n.QO...N.q..A
        0010 - 5a be dc a8 31 fa b3 21-d9 a1 77 0f a9 5e 66 c5   Z...1..!..w..^f.
        0020 - 92 9f 82 00 e8 c8 48 cf-ff 16 8a 62 6d 94 af 59   ......H....bm..Y
        0030 - 5d 61 03 77 31 96 aa d2-5b 91 e4 92 29 8c 3d c2   ]a.w1...[...).=.
        0040 - d9 c6 79 eb 0b ce 2a 0d-fe 47 46 2a a9 7f eb a8   ..y...*..GF*....
        0050 - 7e be b6 a2 10 db 75 5c-12 77 3b 96 f5 cd 45 81   ~.....u\.w;...E.
        0060 - 5f ad e0 ff 43 a8 58 77-db 39 36 81 ab b2 6b 36   _...C.Xw.96...k6
        0070 - c4 c1 a9 0b 99 e1 5e a8-34 a1 b4 72 97 78 86 a7   ......^.4..r.x..
        0080 - 33 c7 ab ba 93 cf d7 a9-0c e4 54 4b 83 74 a0 5e   3.........TK.t.^
        0090 - ba e1 a2 99 f7 14 86 79-e4 bd 41 4a cf 8f 93 de   .......y..AJ....
        00a0 - c4 87 80 4e 18 ee 65 68-15 53 8f 2d 56 5c 53 72   ...N..eh.S.-V\Sr
        00b0 - 0b d3 40 05 9a ec 26 ff-2f e7 e0 24 e4 27 02 9c   ..@...&./..$.'..
        00c0 - 22 0b 84 36 39 ac 3a 15-7b 0b 3c 56 83 e2 82 43   "..69.:.{.<V...C
        00d0 - c9 77 41 28 1b cf 45 c7-bc ef 3d f2 9b c1 3b 1b   .wA(..E...=...;.

        Start Time: 1667493148
        Timeout   : 7200 (sec)
        Verify return code: 0 (ok)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: FDF5369ABC630434B2E11B78E29A2FC43FC0732A147D6D7EE1D370FF0D50595E
        Session-ID-ctx:
        Resumption PSK: D63C7DE07685AB98AF1C6CEEBAE63DE012A22290964DB41AC1D3B2817851C8F38677ECD4EE67E9405CE6A44F0F12688B
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 83100 (seconds)
        TLS session ticket:
        0000 - 00 00 35 6e 11 51 4f da-c0 0e 4e bb 71 b8 9b 41   ..5n.QO...N.q..A
        0010 - ad 56 82 f3 74 6d 6d 53-d5 a8 b5 a9 e6 c5 55 e7   .V..tmmS......U.
        0020 - c2 52 2e f2 d6 b5 cb 18-bd 23 82 63 df a6 8e 29   .R.......#.c...)
        0030 - f3 32 c4 51 e5 c6 e2 29-d9 2f 78 43 45 51 5d c0   .2.Q...)./xCEQ].
        0040 - be d8 8e 3b f5 b5 5b 2f-b8 ee ad ae fb bf 2b a7   ...;..[/......+.
        0050 - 42 42 b7 af 4a 23 09 13-f4 2e 1f 58 9a d6 46 90   BB..J#.....X..F.
        0060 - c4 76 f9 41 05 99 af 2e-77 85 d7 12 32 f0 21 97   .v.A....w...2.!.
        0070 - 79 92 8e 39 31 97 f3 56-9b 06 fd 43 be 97 16 aa   y..91..V...C....
        0080 - 66 49 d8 2e 44 54 8f fe-e5 56 e9 5c 70 ed 79 b7   fI..DT...V.\p.y.
        0090 - 93 4c 74 7b db d2 7f 97-76 84 a6 34 d0 67 0f e4   .Lt{....v..4.g..
        00a0 - 3f a3 86 66 14 86 ae 19-0b e8 3d bf 43 ad df 83   ?..f......=.C...
        00b0 - 29 95 b3 8c 52 9d 2d b4-95 7f e8 50 8f ea 05 04   )...R.-....P....
        00c0 - d5 42 b9 07 b9 10 d5 ae-03 ca 16 18 b7 c4 dc f1   .B..............
        00d0 - cc 8a 49 d7 4a 09 8d b2-e5 d8 3a 9a 98 ea b2 0c   ..I.J.....:.....

        Start Time: 1667493148
        Timeout   : 7200 (sec)
        Verify return code: 0 (ok)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    closed

### Get a server digital certificate

We can obtain a server's digital certificate by doing the following:

    echo | openssl s_client -connect www.google.com:443 2>&1 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > google.crt

After we have the certificate saved in a file (`google.crt`) we can view it.

    openssl x509 -noout -text -in ./google.crt 

Resulting:

    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                31:68:f6:7c:25:93:0f:e5
            Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=US, O=Google Inc, CN=Google Internet Authority G2
            Validity
                Not Before: Oct 28 17:50:22 2015 GMT
                Not After : Jan 26 00:00:00 2016 GMT
            Subject: C=US, ST=California, L=Mountain View, O=Google Inc, CN=www.google.com
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                RSA Public Key: (2048 bit)
                    Modulus (2048 bit):
                        00:b4:4a:88:99:2c:74:01:6a:d4:4a:17:8a:b6:22:
                        34:e1:b7:91:c2:63:22:68:13:3f:f4:31:57:cd:91:
                        d8:c9:b6:b5:f4:77:19:7a:21:67:88:f4:b3:3e:cd:
                        64:2f:0d:ca:bf:f7:20:0c:1b:03:db:27:3e:46:da:
                        82:0f:fe:81:41:85:40:ae:bc:fe:8d:a8:a5:a6:92:
                        54:90:e2:d1:74:c6:1e:a5:ce:3e:32:4f:04:b9:67:
                        d1:e2:59:a3:1b:7d:d9:68:15:b2:f0:90:a4:a5:30:
                        16:3e:5f:6a:d9:07:14:d1:86:05:9c:38:e0:73:65:
                        e4:d4:4a:94:b3:93:e5:b2:06:23:14:d4:f3:e7:cf:
                        35:b7:45:ec:e9:07:dd:e0:bc:cb:5b:23:88:3a:1e:
                        8e:7e:02:fa:b7:83:2e:8f:9a:5c:f5:50:10:f2:f0:
                        3d:9b:d5:af:29:19:b3:39:7f:31:69:cb:bc:a7:36:
                        54:87:a0:c0:2a:55:d3:91:57:3e:97:83:98:e3:47:
                        65:8b:e8:32:98:43:cd:c1:b1:8b:a7:55:1e:73:0e:
                        81:2f:b4:5d:9c:e1:c1:cf:a7:2e:6f:b0:30:60:5d:
                        61:a7:02:b7:bc:6b:e9:0d:b8:00:78:ca:9f:fa:70:
                        8d:1f:f1:2b:a4:f0:a6:02:72:f4:23:35:e0:78:1c:
                        19:3d
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Extended Key Usage: 
                    TLS Web Server Authentication, TLS Web Client Authentication
                X509v3 Subject Alternative Name: 
                    DNS:www.google.com
                Authority Information Access: 
                    CA Issuers - URI:http://pki.google.com/GIAG2.crt
                    OCSP - URI:http://clients1.google.com/ocsp

                X509v3 Subject Key Identifier: 
                    95:CA:1C:F5:FB:39:28:C9:1C:7D:D2:3C:0E:85:68:01:7E:98:7B:4C
                X509v3 Basic Constraints: critical
                    CA:FALSE
                X509v3 Authority Key Identifier: 
                    keyid:4A:DD:06:16:1B:BC:F6:68:B5:76:F5:81:B6:BB:62:1A:BA:5A:81:2F

                X509v3 Certificate Policies: 
                    Policy: 1.3.6.1.4.1.11129.2.5.1
                    Policy: 2.23.140.1.2.2

                X509v3 CRL Distribution Points: 
                    URI:http://pki.google.com/GIAG2.crl

        Signature Algorithm: sha256WithRSAEncryption
            36:57:20:af:df:78:82:4b:bf:83:98:01:06:db:c6:f1:c0:b8:
            6d:b3:8c:ba:38:f3:46:d6:0b:2e:7a:5e:01:42:ca:29:90:37:
            51:05:3c:e8:b3:f8:e8:42:91:0b:25:11:94:5d:f5:bc:eb:d3:
            f0:37:79:a3:c0:03:f9:f3:1e:d9:61:a7:2a:a1:81:12:db:29:
            2f:31:ee:8c:80:b2:e3:a0:5c:e4:03:97:93:31:94:44:23:fb:
            4a:48:e2:39:e1:0d:1b:b6:49:66:6d:7b:2d:fb:69:9d:00:2c:
            62:7c:dd:5c:cd:f1:4c:a0:35:cd:57:36:12:49:10:33:3e:7f:
            e7:55:ac:f5:a5:f8:0e:e9:cd:51:fc:1a:25:fe:41:8c:6f:1a:
            c1:f8:70:f9:f0:e2:b4:28:b1:ea:d9:49:f9:5e:1e:e3:51:4d:
            51:59:6e:0f:26:91:2c:a6:69:37:df:98:a8:95:dd:3e:bc:fd:
            9a:ee:4f:d4:bc:31:40:11:2c:e7:d1:2f:36:e6:26:7b:af:e6:
            6f:41:9e:f4:27:3d:0b:b8:11:f4:67:09:08:ef:40:de:0c:ad:
            fe:81:65:b6:4a:2d:de:02:78:73:43:c7:2c:06:18:b3:75:fd:
            54:dd:f7:c9:1d:ad:6c:b2:aa:70:56:7b:e3:9f:8e:e3:86:63:
            e6:b8:10:fc

### SSLscan

[SSLscan](https://github.com/rbsec/sslscan) is a tool that can be used to test the SSL/TLS protocol on a specific server and potencialy find some problems or even vulnerabilities.

Using it is quite simple:

    sslscan www.google.com

Which results in the fllowing output.

    Version: 2.0.15
    OpenSSL 3.0.7 1 Nov 2022

    Connected to 172.217.17.4

    Testing SSL server www.google.com on port 443 using SNI name www.google.com

    SSL/TLS Protocols:
    SSLv2     disabled
    SSLv3     disabled
    TLSv1.0   enabled
    TLSv1.1   enabled
    TLSv1.2   enabled
    TLSv1.3   enabled

    TLS Fallback SCSV:
    Server supports TLS Fallback SCSV

    TLS renegotiation:
    Secure session renegotiation supported

    TLS Compression:
    OpenSSL version does not support compression
    Rebuild with zlib1g-dev package for zlib support

    Heartbleed:
    TLSv1.3 not vulnerable to heartbleed
    TLSv1.2 not vulnerable to heartbleed
    TLSv1.1 not vulnerable to heartbleed
    TLSv1.0 not vulnerable to heartbleed

    Supported Server Cipher(s):
    Preferred TLSv1.3  128 bits  TLS_AES_128_GCM_SHA256        Curve 25519 DHE 253
    Accepted  TLSv1.3  256 bits  TLS_AES_256_GCM_SHA384        Curve 25519 DHE 253
    Accepted  TLSv1.3  256 bits  TLS_CHACHA20_POLY1305_SHA256  Curve 25519 DHE 253
    Preferred TLSv1.2  256 bits  ECDHE-ECDSA-CHACHA20-POLY1305 Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-ECDSA-AES128-GCM-SHA256 Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-ECDSA-AES256-GCM-SHA384 Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-ECDSA-AES128-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-ECDSA-AES256-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-RSA-CHACHA20-POLY1305   Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-RSA-AES128-GCM-SHA256   Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-GCM-SHA384   Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-RSA-AES128-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  AES128-GCM-SHA256
    Accepted  TLSv1.2  256 bits  AES256-GCM-SHA384
    Accepted  TLSv1.2  128 bits  AES128-SHA
    Accepted  TLSv1.2  256 bits  AES256-SHA
    Accepted  TLSv1.2  112 bits  TLS_RSA_WITH_3DES_EDE_CBC_SHA
    Preferred TLSv1.1  128 bits  ECDHE-ECDSA-AES128-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.1  256 bits  ECDHE-ECDSA-AES256-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.1  128 bits  ECDHE-RSA-AES128-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.1  256 bits  ECDHE-RSA-AES256-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.1  128 bits  AES128-SHA
    Accepted  TLSv1.1  256 bits  AES256-SHA
    Accepted  TLSv1.1  112 bits  TLS_RSA_WITH_3DES_EDE_CBC_SHA
    Preferred TLSv1.0  128 bits  ECDHE-ECDSA-AES128-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.0  256 bits  ECDHE-ECDSA-AES256-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.0  128 bits  ECDHE-RSA-AES128-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.0  256 bits  ECDHE-RSA-AES256-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.0  128 bits  AES128-SHA
    Accepted  TLSv1.0  256 bits  AES256-SHA
    Accepted  TLSv1.0  112 bits  TLS_RSA_WITH_3DES_EDE_CBC_SHA

    Server Key Exchange Group(s):
    TLSv1.3  128 bits  secp256r1 (NIST P-256)
    TLSv1.3  128 bits  x25519
    TLSv1.2  128 bits  secp256r1 (NIST P-256)
    TLSv1.2  128 bits  x25519

    SSL Certificate:
    Signature Algorithm: sha256WithRSAEncryption
    ECC Curve Name:      prime256v1
    ECC Key Strength:    128

    Subject:  www.google.com
    Altnames: DNS:www.google.com
    Issuer:   GTS CA 1C3

    Not valid before: Sep 26 08:23:57 2022 GMT
    Not valid after:  Dec 19 08:23:56 2022 GMT

### SSLyze

[SSLyze](https://github.com/nabla-c0d3/sslyze) is also a tool that might be use to scan for SSL/TLS on a server. Using it is also very simple.

    sslyze www.google.com

Which results in the following.

    CHECKING CONNECTIVITY TO SERVER(S)
    ----------------------------------

    www.google.com:443        => 142.250.200.132


    SCAN RESULTS FOR WWW.GOOGLE.COM:443 - 142.250.200.132
    -----------------------------------------------------

    * Certificates Information:
        Hostname sent for SNI:             www.google.com
        Number of certificates detected:   2


        Certificate #0 ( _EllipticCurvePublicKey )
        SHA1 Fingerprint:                  8d451b91f4f3d1beaf7dfd745739a0a1ff9bb7b2
        Common Name:                       www.google.com
        Issuer:                            GTS CA 1C3
        Serial Number:                     172338986348961426169022814442718324929
        Not Before:                        2022-10-17
        Not After:                         2023-01-09
        Public Key Algorithm:              _EllipticCurvePublicKey
        Signature Algorithm:               sha256
        Key Size:                          256
        Curve:                             secp256r1
        DNS Subject Alternative Names:     ['www.google.com']

        Certificate #0 - Trust
        Hostname Validation:               OK - Certificate matches server hostname
        Android CA Store (13.0.0_r8):      OK - Certificate is trusted
        Apple CA Store (iOS 15.1, iPadOS 15.1, macOS 12.1, tvOS 15.1, and watchOS 8.1):OK - Certificate is trusted
        Java CA Store (jdk-13.0.2):        OK - Certificate is trusted
        Mozilla CA Store (2022-09-18):     OK - Certificate is trusted
        Windows CA Store (2022-08-15):     OK - Certificate is trusted
        Symantec 2018 Deprecation:         OK - Not a Symantec-issued certificate
        Received Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Verified Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Received Chain Contains Anchor:    OK - Anchor certificate not sent
        Received Chain Order:              OK - Order is valid
        Verified Chain contains SHA1:      OK - No SHA1-signed certificate in the verified certificate chain

        Certificate #0 - Extensions
        OCSP Must-Staple:                  NOT SUPPORTED - Extension not found
        Certificate Transparency:          WARNING - Only 2 SCTs included but Google recommends 3 or more

        Certificate #0 - OCSP Stapling
                                            NOT SUPPORTED - Server did not send back an OCSP response


        Certificate #1 ( _RSAPublicKey )
        SHA1 Fingerprint:                  bb181548dc5db04f8194e588b4f19f1cc4d12a0d
        Common Name:                       www.google.com
        Issuer:                            GTS CA 1C3
        Serial Number:                     265906118735752094790635674560926019293
        Not Before:                        2022-10-17
        Not After:                         2023-01-09
        Public Key Algorithm:              _RSAPublicKey
        Signature Algorithm:               sha256
        Key Size:                          2048
        Exponent:                          65537
        DNS Subject Alternative Names:     ['www.google.com']

        Certificate #1 - Trust
        Hostname Validation:               OK - Certificate matches server hostname
        Android CA Store (13.0.0_r8):      OK - Certificate is trusted
        Apple CA Store (iOS 15.1, iPadOS 15.1, macOS 12.1, tvOS 15.1, and watchOS 8.1):OK - Certificate is trusted
        Java CA Store (jdk-13.0.2):        OK - Certificate is trusted
        Mozilla CA Store (2022-09-18):     OK - Certificate is trusted
        Windows CA Store (2022-08-15):     OK - Certificate is trusted
        Symantec 2018 Deprecation:         OK - Not a Symantec-issued certificate
        Received Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Verified Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Received Chain Contains Anchor:    OK - Anchor certificate not sent
        Received Chain Order:              OK - Order is valid
        Verified Chain contains SHA1:      OK - No SHA1-signed certificate in the verified certificate chain

        Certificate #1 - Extensions
        OCSP Must-Staple:                  NOT SUPPORTED - Extension not found
        Certificate Transparency:          WARNING - Only 2 SCTs included but Google recommends 3 or more

        Certificate #1 - OCSP Stapling
                                            NOT SUPPORTED - Server did not send back an OCSP response

    * SSL 2.0 Cipher Suites:
        Attempted to connect using 7 cipher suites; the server rejected all cipher suites.

    * SSL 3.0 Cipher Suites:
        Attempted to connect using 80 cipher suites; the server rejected all cipher suites.

    * TLS 1.0 Cipher Suites:
        Attempted to connect using 80 cipher suites.

        The server accepted the following 5 cipher suites:
            TLS_RSA_WITH_AES_256_CBC_SHA                      256
            TLS_RSA_WITH_AES_128_CBC_SHA                      128
            TLS_RSA_WITH_3DES_EDE_CBC_SHA                     168
            TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA                256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA                128       ECDH: prime256v1 (256 bits)

        The group of cipher suites supported by the server has the following properties:
        Forward Secrecy                    OK - Supported
        Legacy RC4 Algorithm               OK - Not Supported


    * TLS 1.1 Cipher Suites:
        Attempted to connect using 80 cipher suites.

        The server accepted the following 5 cipher suites:
            TLS_RSA_WITH_AES_256_CBC_SHA                      256
            TLS_RSA_WITH_AES_128_CBC_SHA                      128
            TLS_RSA_WITH_3DES_EDE_CBC_SHA                     168
            TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA                256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA                128       ECDH: prime256v1 (256 bits)

        The group of cipher suites supported by the server has the following properties:
        Forward Secrecy                    OK - Supported
        Legacy RC4 Algorithm               OK - Not Supported


    * TLS 1.2 Cipher Suites:
        Attempted to connect using 156 cipher suites.

        The server accepted the following 11 cipher suites:
            TLS_RSA_WITH_AES_256_GCM_SHA384                   256
            TLS_RSA_WITH_AES_256_CBC_SHA                      256
            TLS_RSA_WITH_AES_128_GCM_SHA256                   128
            TLS_RSA_WITH_AES_128_CBC_SHA                      128
            TLS_RSA_WITH_3DES_EDE_CBC_SHA                     168
            TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256       256       ECDH: X25519 (253 bits)
            TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384             256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA                256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256             128       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA                128       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256     256       ECDH: X25519 (253 bits)

        The group of cipher suites supported by the server has the following properties:
        Forward Secrecy                    OK - Supported
        Legacy RC4 Algorithm               OK - Not Supported


    * TLS 1.3 Cipher Suites:
        Attempted to connect using 5 cipher suites.

        The server accepted the following 3 cipher suites:
            TLS_CHACHA20_POLY1305_SHA256                      256       ECDH: X25519 (253 bits)
            TLS_AES_256_GCM_SHA384                            256       ECDH: X25519 (253 bits)
            TLS_AES_128_GCM_SHA256                            128       ECDH: X25519 (253 bits)


    * Deflate Compression:
                                            OK - Compression disabled

    * OpenSSL CCS Injection:
                                            OK - Not vulnerable to OpenSSL CCS injection

    * OpenSSL Heartbleed:
                                            OK - Not vulnerable to Heartbleed

    * ROBOT Attack:
                                            OK - Not vulnerable.

    * Session Renegotiation:
        Client Renegotiation DoS Attack:   OK - Not vulnerable
        Secure Renegotiation:              OK - Supported

    * Elliptic Curve Key Exchange:
        Supported curves:                  X25519, prime256v1
        Rejected curves:                   X448, prime192v1, secp160k1, secp160r1, secp160r2, secp192k1, secp224k1, secp224r1, secp256k1, secp384r1, secp521r1, sect163k1, sect163r1, sect163r2, sect193r1, sect193r2, sect233k1, sect233r1, sect239k1, sect283k1, sect283r1, sect409k1, sect409r1, sect571k1, sect571r1

    SCANS COMPLETED IN 4.242441 S
    -----------------------------

    COMPLIANCE AGAINST MOZILLA TLS CONFIGURATION
    --------------------------------------------

        Checking results against Mozilla's "intermediate" configuration. See https://ssl-config.mozilla.org/ for more details.

        www.google.com:443: FAILED - Not compliant.
            * tls_versions: TLS versions {'TLSv1', 'TLSv1.1'} are supported, but should be rejected.
            * ciphers: Cipher suites {'TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA', 'TLS_RSA_WITH_AES_256_CBC_SHA', 'TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA', 'TLS_RSA_WITH_AES_128_CBC_SHA', 'TLS_RSA_WITH_3DES_EDE_CBC_SHA', 'TLS_RSA_WITH_AES_256_GCM_SHA384', 'TLS_RSA_WITH_AES_128_GCM_SHA256'} are supported, but should be rejected.

### testssl.sh

Another interesting tool to test SSL/TLS is to use the [testssl.sh](https://github.com/drwetter/testssl.sh) tool. It is also adequate to find problems on SSL/TLS servers.

To use it simply execute:

testssl.sh carlos.serrao.me

The obtained output is something like this (just a small portion):

    Testing vulnerabilities

    Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
    CCS (CVE-2014-0224)                       not vulnerable (OK)
    Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK), no session ticket extension
    ROBOT                                     Server does not support any cipher suites that use RSA key transport
    Secure Renegotiation (RFC 5746)           supported (OK)
    Secure Client-Initiated Renegotiation     not vulnerable (OK)
    CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
    BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                            Can be ignored for static pages or if no secrets in the page
    POODLE, SSL (CVE-2014-3566)               not vulnerable (OK), no SSLv3 support
    TLS_FALLBACK_SCSV (RFC 7507)              No fallback possible (OK), no protocol below TLS 1.2 offered
    SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
    FREAK (CVE-2015-0204)                     not vulnerable (OK)
    DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                            make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                            https://search.censys.io/search?resource=hosts&virtual_hosts=INCLUDE&q=C5D0C2C5E14B71CF7342F4BEAB81322EF9AD9B9BEEBB3D72ACB5E6360EA08C15
    LOGJAM (CVE-2015-4000), experimental      common prime with 2048 bits detected: RFC3526/Oakley Group 14 (2048 bits),
                                            but no DH EXPORT ciphers
    BEAST (CVE-2011-3389)                     not vulnerable (OK), no SSL3 or TLS1
    LUCKY13 (CVE-2013-0169), experimental     not vulnerable (OK)
    RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)

### SSL/TLS online tools

The following list presents some online tools for testing also SSL/TLS.

1. [SSL Server Test by Qualys](https://www.ssllabs.com/ssltest/)
2. [SSL Checker by SSL Shopper](https://www.sslshopper.com/ssl-checker.html)
3. [SSL Scanner by SSL Tools](http://www.ssltools.com/)
4. [SSL Certificate Checker by Digicert](https://www.digicert.com/help/)
5. [SSL Check by JitBit](https://www.jitbit.com/sslcheck/)
6. [SSL Checker by SSL Store](https://www.thesslstore.com/ssltools/ssl-checker.php)
7. [SSL Tester by Wormly](https://www.wormly.com/test_ssl)
8. [SSL Security Test by ImmuniWeb](https://www.immuniweb.com/ssl/)
9. [SSL Certificate Checker](https://www.sslchecker.com/sslchecker)
10. [SSLv3 Poodle Vulnerability Scanner](https://pentest-tools.com/network-vulnerability-scanning/ssl-poodle-scanner)

## References/Additional information

1. [https://openssl.org/docs/manmaster/apps/openssl.html](https://openssl.org/docs/manmaster/apps/openssl.html)
2. [http://www.g-loaded.eu/2005/11/10/be-your-own-ca/](http://www.g-loaded.eu/2005/11/10/be-your-own-ca/)
3. [https://jamielinux.com/docs/openssl-certificate-authority/index.html](https://jamielinux.com/docs/openssl-certificate-authority/index.html)
4. [http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html](http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html)
5. [https://www.openssl.org/docs/manmaster/apps/config.html](https://www.openssl.org/docs/manmaster/apps/config.html)
6. [https://www.mkssoftware.com/docs/man1/openssl_smime.1.asp](https://www.mkssoftware.com/docs/man1/openssl_smime.1.asp)
7. [https://www.madboa.com/geek/openssl/](https://www.madboa.com/geek/openssl/)
8. [https://www.misterpki.com/openssl-verify/](https://www.misterpki.com/openssl-verify/)
9. [https://www.misterpki.com/openssl-s-client/](https://www.misterpki.com/openssl-s-client/)

