# Hashes and Message Authentication Codes <!-- omit in toc -->

## Index <!-- omit in toc -->


- [Hash generation](#hash-generation)
  - [Generate hash of a file](#generate-hash-of-a-file)
  - [Generate a Message Authentication Code](#generate-a-message-authentication-code)


## Hash generation

OpenSSL also has features for generating hashes of various types.

### Generate hash of a file

Using SHA1

    openssl sha1 <FILE> 

    SHA1(<FILE>)= 95e91837dc1a4a8eeb42208420d8620cb8d7785f

Using SHA256

    openssl sha256 <FILE> 

    SHA256(<FILE>)= b110ed205353743923c7d66811a2916e2cc3bb3a06e7411e79a4b124ca1322d0

Using RIPEMD160

    openssl ripemd160 <FILE> 

    RIPEMD160(<FILE>)= f9bb6ff349a5f03531af89774f9a6578a785aec8

It is also possible to use the openssl `dgst` command to create hashes from data. 

### Listing the available hash functions

To list the available hash functions, simply do:

    openssl dgst -list

This will present all the hash functions that are supported by openssl:

    Supported digests:
    -blake2b512                -blake2s256                -md4
    -md5                       -md5-sha1                  -mdc2
    -ripemd                    -ripemd160                 -rmd160
    -sha1                      -sha224                    -sha256
    -sha3-224                  -sha3-256                  -sha3-384
    -sha3-512                  -sha384                    -sha512
    -sha512-224                -sha512-256                -shake128
    -shake256                  -sm3                       -ssl3-md5
    -ssl3-sha1                 -whirlpool

### Create a hash from a file

To create a hash from a specific file, we need to do:

    openssl dgst -sha256 <FILE>

That produces the appropriate value:

    SHA2-256(<FILE>)= 76e3bccb0c1e24061c17f286e5eec8425e9fc22800a446dfbfe6fad3d3ecd950

## Create a hash with a "salt"

The following command will create a hash value from a password (`mypassword`) with a "salt" value (`mysaltvalue`) - using the **SHA256**:

    openssl passwd -5 -salt mysaltvalue mypassword 

## Generate a Message Authentication Code

In order to generate a hash-based message authentication code, we need to provide the hash algorithm to be used and the secret key to encrypt the hash.

    openssl sha1 -hmac 24899ec1e452d219121f0d07cd6975b7 <FILE>

    HMAC-SHA1(<FILE>)= 384773867cb7bce6ffd97a95c26e65666c045ea7

