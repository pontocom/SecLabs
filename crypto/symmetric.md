# Symmetric Cryptography <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Symmetric Cryptography](#symmetric-cryptography)
  - [List of options for an algorithm](#list-of-options-for-an-algorithm)
  - [Encrypt a file](#encrypt-a-file)
  - [Decrypt a file](#decrypt-a-file)
  - [Ciphering and deciphering an image](#ciphering-and-deciphering-an-image)

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

I have a sample image that might be used to do this -> [tux bitmap image](assets/tux_original.bmp).

![](assets/tux_original.png)

As a note to help you with this task, in order to separate the header from the image data (body), you can use the following commands:

    head -c 54 IMAGEM.bmp > header.file
    tail -c +55 IMAGEM.bmp > body.file

If by any change you are not able to do a `head` and a `tail` on a file, you can find the tux file header [here](assets/header.file) and the body [here](assets/body.file).

We will generate a random key to be able to encrypt the image:

    openssl rand -hex 32

We will encrypt the image using ECB mode (we use the random key obtained in the previous step):

    openssl aes-256-ecb -e -in ./body.file -out ./body_ecb -K 2ffd27e0675b9bd6c34e37109c4ebef378f356f0fa7eeeaf11eb2433e21e980e

After you have encrypted the image data (body), you can re-attach the header to get the image in the full format:

    cat header.file body_ecb > imagem_ECB.bmp

![](assets/tux_ecb.png)

We are going to cipher the image now with CBC mode. For this mode we need to use an IV. To create this IV we can generate it randomly:

    openssl rand -hex 16

We will encrypt the image using CBC mode (we use the random key obtained in the previous step and the IV):

    openssl aes-256-cbc -e -in ./body -out ./body_cbc -K 2ffd27e0675b9bd6c34e37109c4ebef378f356f0fa7eeeaf11eb2433e21e980e -iv 0faf3e077df51ebd98cd11925ad9dcd1

After you have encrypted the image data (body), you can re-attach the header to get the image in the full format:

    cat header body_cbc > imagem_CBC.bmp

![](assets/tux_cbc.png)