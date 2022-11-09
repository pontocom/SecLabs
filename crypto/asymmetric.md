# Asymmetric cryptography <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Asymmetric cryptography](#asymmetric-cryptography)
  - [Key pair generation](#key-pair-generation)
  - [Generate a key pair](#generate-a-key-pair)
  - [Generate a key pair and protect the private key](#generate-a-key-pair-and-protect-the-private-key)
  - [Print the various components of the key pair](#print-the-various-components-of-the-key-pair)
  - [Extract the public key from the key pair](#extract-the-public-key-from-the-key-pair)
  - [Encrypt using the public key](#encrypt-using-the-public-key)
  - [Decrypt using the private key](#decrypt-using-the-private-key)

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