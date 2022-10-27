# Cryptography with OpenSSL/LibreSSL

## Index <!-- omit in toc -->
- [Cryptography with OpenSSL/LibreSSL](#cryptography-with-openssllibressl)
  - [INTRODUCTION](#introduction)
  - [SETUP](#setup)
  - [BASIC COMMANDS](#basic-commands)
    - [CHECK OPENSSL VERSION](#check-openssl-version)
  - [OPENSSL INTERACTIVE CONSOLE ACCESS](#openssl-interactive-console-access)
  - [CHECK OPTIONS FOR A PARTICULAR COMMAND](#check-options-for-a-particular-command)
  - [SYMMETRIC CRYPTOGRAPHY](#symmetric-cryptography)
    - [LIST OPTIONS OF AN ALGORITHM](#list-options-of-an-algorithm)
    - [ENCRYPT A FILE](#encrypt-a-file)
    - [DECRYPT A FILE](#decrypt-a-file)
    - [CIPHERING AND DECIPHERING AN IMAGE](#ciphering-and-deciphering-an-image)
  - [HASH GENERATION](#hash-generation)
    - [GENERATE HASH OF A FILE](#generate-hash-of-a-file)
    - [GENERATE A MESSAGE AUTHENTICATION CODE](#generate-a-message-authentication-code)
  - [ASYMMETRIC CRYPTOGRAPHY](#asymmetric-cryptography)
    - [KEY PAIR GENERATION](#key-pair-generation)
    - [Generate a key pair](#generate-a-key-pair)
    - [Generate a key pair and protect the private key](#generate-a-key-pair-and-protect-the-private-key)
    - [Print the various components of the key pair](#print-the-various-components-of-the-key-pair)
    - [EXTRACT THE PUBLIC KEY FROM THE KEY PAIR](#extract-the-public-key-from-the-key-pair)
    - [ENCRYPT USING THE PUBLIC KEY](#encrypt-using-the-public-key)
    - [DECRYPT USING THE PRIVATE KEY](#decrypt-using-the-private-key)
  - [USING ELLIPTIC CURVE CRYPTOGRAPHY](#using-elliptic-curve-cryptography)
    - [LISTING THE AVAILABLE CURVES](#listing-the-available-curves)
    - [GENERATE A PRIVATE KEY USING ECC](#generate-a-private-key-using-ecc)
    - [GENERATE A PUBLIC KEY USING ECC](#generate-a-public-key-using-ecc)
    - [ENCRYPT WITH ECC](#encrypt-with-ecc)

## INTRODUCTION
This document aims to demonstrate the use of cryptographic mechanisms based on the OpenSSL/LibreSSL library. This library has the ability to work with cryptographic mechanisms such as symmetric cryptography, asymmetric cryptography, generation of message authentication codes as well as work with digital certificates and more.

To run these labs the following requirements are needed:
- OpenSSL/LibreSSL.

The goal of these labs is to demonstrate the use of the OpenSSL library to provide a range of cryptographic functionality.

The examples presented here only demonstrate just a small sample of the full capabilities of OpenSSL.

## SETUP
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

## BASIC COMMANDS
Here we will just list some of the basic commands for working with OpenSSL.
### CHECK OPENSSL VERSION
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

## OPENSSL INTERACTIVE CONSOLE ACCESS
    openssl

    OpenSSL> 

## CHECK OPTIONS FOR A PARTICULAR COMMAND
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

## SYMMETRIC CRYPTOGRAPHY
Here we will demonstrate some of the features of the OpenSSL library for performing symmetric key cryptography.
### LIST OPTIONS OF AN ALGORITHM
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

### ENCRYPT A FILE
We are going to encrypt a file (image.jpg), with a key, generated from a passphrase (a password selected by the user, which will be converted to a 128-bit secret key, using an algorithm called [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2)):

    openssl aes-128-ecb -in ./imagem.jpg -out ./imagem.aes.jpg -e -a -k secretpass -pbkdf2

### DECRYPT A FILE
Let's now decrypt a file (image.aes.jpg) and recover the original format (image.orig.jpg).

    openssl aes-128-ecb -in ./imagem.aes.jpg -out ./imagem.orig.jpg -d -a -k secretpass -pbkdf2

### CIPHERING AND DECIPHERING AN IMAGE
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

## HASH GENERATION
OpenSSL also has features for generating hashes of various types.
### GENERATE HASH OF A FILE
Using SHA1

    openssl sha1 ./imagem.jpg 

    SHA1(./imagem.jpg)= 95e91837dc1a4a8eeb42208420d8620cb8d7785f

Using SHA256
    openssl sha256 ./imagem.jpg 

    SHA256(./imagem.jpg)= b110ed205353743923c7d66811a2916e2cc3bb3a06e7411e79a4b124ca1322d0

Using RIPEMD160
    openssl ripemd160 ./imagem.jpg 

    RIPEMD160(./imagem.jpg)= f9bb6ff349a5f03531af89774f9a6578a785aec8

### GENERATE A MESSAGE AUTHENTICATION CODE
In order to generate a hash-based message authentication code, we need to provide the hash algorithm to be used and the secret key to encrypt the hash.

    openssl sha1 -hmac 24899ec1e452d219121f0d07cd6975b7 ./imagem.jpg

    HMAC-SHA1(./imagem.jpg)= 384773867cb7bce6ffd97a95c26e65666c045ea7

## ASYMMETRIC CRYPTOGRAPHY
OpenSSL also allows you to implement a series of asymmetric cryptographic features-key pair generation, encryption of decryption of information with OpenSSL.
### KEY PAIR GENERATION
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

### EXTRACT THE PUBLIC KEY FROM THE KEY PAIR
When generating the key pair, both keys are stored in the same file, so if you want to extract the public key, you have to do it explicitly.

    openssl rsa -in keypair.pem -pubout -out ./publickey.pem

### ENCRYPT USING THE PUBLIC KEY
It is only suitable for encrypting small blocks of information. In this case a "`secretkey`" file was created, with a small random value using the command "`openssl rand -out ./secretkey 32`". Then this file was encrypted using the public key.

    openssl pkeyutl -encrypt -inkey -pubin ./publickey.pem -in ./secretkey -out ./secretkey.enc

### DECRYPT USING THE PRIVATE KEY
With the following command it is possible to get the original text back by decrypting it with the corresponding private key.

    openssl pkeyutl -decrypt -inkey ./keypair.pem -in ./secretkey.enc -out ./secretkey.dec 

## USING ELLIPTIC CURVE CRYPTOGRAPHY
The following section of this document is related to the usage of ECC – Elliptic Curve Cryptography. ECC is an approach to public-key cryptography based on the algebraic structure of elliptic curves over finite fields. ECC allows smaller keys compared to non-EC cryptography (based on plain Galois fields) to provide equivalent security. Elliptic curves are applicable for key agreement, digital signatures, pseudo-random generators and other tasks. Indirectly, they can be used for encryption by combining the key agreement with a symmetric encryption scheme. Elliptic curves are also used in several integer factorization algorithms based on elliptic curves that have applications in cryptography, such as Lenstra elliptic-curve factorization.
### LISTING THE AVAILABLE CURVES
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

### GENERATE A PRIVATE KEY USING ECC
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

### GENERATE A PUBLIC KEY USING ECC
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

### ENCRYPT WITH ECC
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

