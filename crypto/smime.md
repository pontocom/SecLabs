# S/MIME <!-- omit in toc -->

## Index <!-- omit in toc -->

- [S/MIME](#smime)
  - [Create keys and certificates](#create-keys-and-certificates)
    - [Create key pairs and CSR for certificate request](#create-key-pairs-and-csr-for-certificate-request)
    - [Request and issue the certificates](#request-and-issue-the-certificates)
  - [Encrypt MIME information](#encrypt-mime-information)
  - [Decrypt MIME information](#decrypt-mime-information)
  - [Sign MIME information](#sign-mime-information)
  - [Verify digital signature of MIME information](#verify-digital-signature-of-mime-information)


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
