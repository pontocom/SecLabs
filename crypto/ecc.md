# Elliptic Curve Cryptography (ECC) <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Using the Elliptic Curve Cryptography](#using-the-elliptic-curve-cryptography)
  - [Listing the available curves](#listing-the-available-curves)
  - [Generate a private key using ECC](#generate-a-private-key-using-ecc)
  - [Generate a public key using ECC](#generate-a-public-key-using-ecc)
  - [Encrypt with ECC](#encrypt-with-ecc)


## Using the Elliptic Curve Cryptography

The following section of this document is related to the usage of **ECC – Elliptic Curve Cryptography**. ECC is an approach to public-key cryptography based on the algebraic structure of elliptic curves over finite fields. ECC allows smaller keys compared to non-EC cryptography (based on plain Galois fields) to provide equivalent security. Elliptic curves are applicable for key agreement, digital signatures, pseudo-random generators and other tasks. Indirectly, they can be used for encryption by combining the key agreement with a symmetric encryption scheme. Elliptic curves are also used in several integer factorization algorithms based on elliptic curves that have applications in cryptography, such as Lenstra elliptic-curve factorization.

### Listing the available curves

The first think that is necessary to do is to select the appropriate curve to work with. In order to do that, the following command can be used:

    openssl ecparam -list_curves

As a result, we obtain a list of possible curves (this is just a small sample):

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