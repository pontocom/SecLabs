# Digital certificates <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Digital certificates](#digital-certificates)
  - [Create a certification authority](#create-a-certification-authority)
  - [Verify the generated digital certificate](#verify-the-generated-digital-certificate)
  - [Generate a digital certificate request](#generate-a-digital-certificate-request)
  - [Issuance of the digital certificate](#issuance-of-the-digital-certificate)
  - [Revocation of digital certificates](#revocation-of-digital-certificates)


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

This [book in this link](https://www.feistyduck.com/library/openssl-cookbook/online/openssl-command-line/private-ca-creating-root.html) also contains detailed instructions on how to create your own private CA.

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