# Digital signatures <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Digital signatures](#digital-signatures)
  - [Generate a digital signature for a file](#generate-a-digital-signature-for-a-file)
  - [Verify the digital signature of a file](#verify-the-digital-signature-of-a-file)


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