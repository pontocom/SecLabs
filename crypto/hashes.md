# Hashes and Message Authentication Codes <!-- omit in toc -->

## Index <!-- omit in toc -->


- [Hash generation](#hash-generation)
  - [Generate hash of a file](#generate-hash-of-a-file)
  - [Generate a Message Authentication Code](#generate-a-message-authentication-code)


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

