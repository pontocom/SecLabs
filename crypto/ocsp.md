# OCSP - Online Certificate Status Protocol <!-- omit in toc -->

## Index <!-- omit in toc -->

- [OCSP - Online Certificate Status Protocol](#ocsp---online-certificate-status-protocol)
  - [Getting the server certificate](#getting-the-server-certificate)
  - [Getting the intermediate certificates](#getting-the-intermediate-certificates)
  - [Get the OCSP responder for a certificate](#get-the-ocsp-responder-for-a-certificate)
  - [Make the OCSP request](#make-the-ocsp-request)


## OCSP - Online Certificate Status Protocol

[OCSP](https://www.ietf.org/rfc/rfc2560.txt) is an interactive way to enable applications to check for the revocation of an identified certificate. In order to test for OCSP validity, we require the certificate that we are going to test, the certificate chain of such certificate, and the OCSP responder address.

### Getting the server certificate

Get the server certificate that we want to check for OCSP validity:

    openssl s_client -connect www.akamai.com:443 < /dev/null 2>&1 |  sed -n '/-----BEGIN/,/-----END/p' > certificate.pem

Certificate will be store on the `certificate.pem` file.

### Getting the intermediate certificates

Getting the multiple intermediary certificates for an entity.

    openssl s_client -showcerts -connect www.akamai.com:443 > chain.pem

These certificates will be store in `chain.pem` file.

Inside the `chain.pem` you'll find the following information:

    CONNECTED(00000003)
    ---
    Certificate chain
    0 s:C = US, ST = Massachusetts, L = Cambridge, O = "Akamai Technologies, Inc.", CN = www.akamai.com
    i:C = US, O = DigiCert Inc, CN = DigiCert TLS RSA SHA256 2020 CA1
    a:PKEY: id-ecPublicKey, 256 (bit); sigalg: RSA-SHA256
    v:NotBefore: Apr 25 00:00:00 2023 GMT; NotAfter: Apr 25 23:59:59 2024 GMT
    -----BEGIN CERTIFICATE-----
    MIIGAjCCBOqgAwIBAgIQBkN1zR+sS+PC+kWvOJwagjANBgkqhkiG9w0BAQsFADBP
    MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMSkwJwYDVQQDEyBE
    aWdpQ2VydCBUTFMgUlNBIFNIQTI1NiAyMDIwIENBMTAeFw0yMzA0MjUwMDAwMDBa
    Fw0yNDA0MjUyMzU5NTlaMHYxCzAJBgNVBAYTAlVTMRYwFAYDVQQIEw1NYXNzYWNo
    dXNldHRzMRIwEAYDVQQHEwlDYW1icmlkZ2UxIjAgBgNVBAoTGUFrYW1haSBUZWNo
    bm9sb2dpZXMsIEluYy4xFzAVBgNVBAMTDnd3dy5ha2FtYWkuY29tMFkwEwYHKoZI
    zj0CAQYIKoZIzj0DAQcDQgAEazkJqPize87Z0TvOKqKpmfyMOOZotoW2EL4YUz2+
    OEcbZ3SY+aULa2c8/Y15S9ZDa+dssAUZ0r4Zy4x/W7Jx5aOCA3wwggN4MB8GA1Ud
    IwQYMBaAFLdrouqoqoSMeeq02g+YssWVdrn0MB0GA1UdDgQWBBQxd8ph2AKQD7PD
    VrXvZgwe+GZs4DAlBgNVHREEHjAcgg53d3cuYWthbWFpLmNvbYIKYWthbWFpLmNv
    bTAOBgNVHQ8BAf8EBAMCB4AwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMC
    MIGPBgNVHR8EgYcwgYQwQKA+oDyGOmh0dHA6Ly9jcmwzLmRpZ2ljZXJ0LmNvbS9E
    aWdpQ2VydFRMU1JTQVNIQTI1NjIwMjBDQTEtNC5jcmwwQKA+oDyGOmh0dHA6Ly9j
    cmw0LmRpZ2ljZXJ0LmNvbS9EaWdpQ2VydFRMU1JTQVNIQTI1NjIwMjBDQTEtNC5j
    cmwwPgYDVR0gBDcwNTAzBgZngQwBAgIwKTAnBggrBgEFBQcCARYbaHR0cDovL3d3
    dy5kaWdpY2VydC5jb20vQ1BTMH8GCCsGAQUFBwEBBHMwcTAkBggrBgEFBQcwAYYY
    aHR0cDovL29jc3AuZGlnaWNlcnQuY29tMEkGCCsGAQUFBzAChj1odHRwOi8vY2Fj
    ZXJ0cy5kaWdpY2VydC5jb20vRGlnaUNlcnRUTFNSU0FTSEEyNTYyMDIwQ0ExLTEu
    Y3J0MAkGA1UdEwQCMAAwggGABgorBgEEAdZ5AgQCBIIBcASCAWwBagB3AO7N0GTV
    2xrOxVy3nbTNE6Iyh0Z8vOzew1FIWUZxH7WbAAABh7W705QAAAQDAEgwRgIhAPVk
    7yDDPj3tAQc4cvqb9A0B0HjlyhdRFVP/H7/BRNmpAiEAycsOC/0dh53xb/G/BTL6
    GJPQgjc/MtM8D/eWAbCNUKoAdwBIsONr2qZHNA/lagL6nTDrHFIBy1bdLIHZu7+r
    OdiEcwAAAYe1u9PtAAAEAwBIMEYCIQDfupV1HkMJ8PWrMU/1qx6LrJ5UlmKesvqc
    7+87ct4ejwIhAOmSrN2AduhQODpqlLc0cvGzeGXKjIonGkoVRCkv3egyAHYA2ra/
    az+1tiKfm8K7XGvocJFxbLtRhIU0vaQ9MEjX+6sAAAGHtbvTrgAABAMARzBFAiEA
    se+cqNPobp9xgg7xBXa8hGm2Pxdwe+U043YEzlVujPQCIBhYLChVIaCMEDPC9VRv
    q7WJLHNa7WNrqBY2niGEPCFeMA0GCSqGSIb3DQEBCwUAA4IBAQCou6fmQrBrvt7x
    E6rJzQ2OArQWWB7G1UX/NOfkZh3NolCA/b36QNJETbHk2OCGKOwjBdaTsu1ekjI2
    LbzASgM8A0VvsnIIgLawZOzZmKO+OtRzBzNQYm9fkMqoDdY/OqmGrEJ+JwXrg2qz
    8+o5l8Nl7uDf6C3PBpu9w+rWV0arBsSHNWu0Vz82K5031WteXoZdoAJBJJTgjmSE
    UVmhXLWZ0G/JOQlpBk903/JNAtRyReELNxWJCms+OIa3EmelAJ4wbDjHxMqxH2Y2
    +d0jhh/LzTOp8sl2BHlzpMavTX0808PQtO0ywZRCYpPBCPFs4HDDrGwVCVLyevJx
    qaCk2c5Z
    -----END CERTIFICATE-----
    1 s:C = US, O = DigiCert Inc, CN = DigiCert TLS RSA SHA256 2020 CA1
    i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
    a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
    v:NotBefore: Apr 14 00:00:00 2021 GMT; NotAfter: Apr 13 23:59:59 2031 GMT
    -----BEGIN CERTIFICATE-----
    MIIEvjCCA6agAwIBAgIQBtjZBNVYQ0b2ii+nVCJ+xDANBgkqhkiG9w0BAQsFADBh
    MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
    d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
    QTAeFw0yMTA0MTQwMDAwMDBaFw0zMTA0MTMyMzU5NTlaME8xCzAJBgNVBAYTAlVT
    MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxKTAnBgNVBAMTIERpZ2lDZXJ0IFRMUyBS
    U0EgU0hBMjU2IDIwMjAgQ0ExMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
    AQEAwUuzZUdwvN1PWNvsnO3DZuUfMRNUrUpmRh8sCuxkB+Uu3Ny5CiDt3+PE0J6a
    qXodgojlEVbbHp9YwlHnLDQNLtKS4VbL8Xlfs7uHyiUDe5pSQWYQYE9XE0nw6Ddn
    g9/n00tnTCJRpt8OmRDtV1F0JuJ9x8piLhMbfyOIJVNvwTRYAIuE//i+p1hJInuW
    raKImxW8oHzf6VGo1bDtN+I2tIJLYrVJmuzHZ9bjPvXj1hJeRPG/cUJ9WIQDgLGB
    Afr5yjK7tI4nhyfFK3TUqNaX3sNk+crOU6JWvHgXjkkDKa77SU+kFbnO8lwZV21r
    eacroicgE7XQPUDTITAHk+qZ9QIDAQABo4IBgjCCAX4wEgYDVR0TAQH/BAgwBgEB
    /wIBADAdBgNVHQ4EFgQUt2ui6qiqhIx56rTaD5iyxZV2ufQwHwYDVR0jBBgwFoAU
    A95QNVbRTLtm8KPiGxvDl7I90VUwDgYDVR0PAQH/BAQDAgGGMB0GA1UdJQQWMBQG
    CCsGAQUFBwMBBggrBgEFBQcDAjB2BggrBgEFBQcBAQRqMGgwJAYIKwYBBQUHMAGG
    GGh0dHA6Ly9vY3NwLmRpZ2ljZXJ0LmNvbTBABggrBgEFBQcwAoY0aHR0cDovL2Nh
    Y2VydHMuZGlnaWNlcnQuY29tL0RpZ2lDZXJ0R2xvYmFsUm9vdENBLmNydDBCBgNV
    HR8EOzA5MDegNaAzhjFodHRwOi8vY3JsMy5kaWdpY2VydC5jb20vRGlnaUNlcnRH
    bG9iYWxSb290Q0EuY3JsMD0GA1UdIAQ2MDQwCwYJYIZIAYb9bAIBMAcGBWeBDAEB
    MAgGBmeBDAECATAIBgZngQwBAgIwCAYGZ4EMAQIDMA0GCSqGSIb3DQEBCwUAA4IB
    AQCAMs5eC91uWg0Kr+HWhMvAjvqFcO3aXbMM9yt1QP6FCvrzMXi3cEsaiVi6gL3z
    ax3pfs8LulicWdSQ0/1s/dCYbbdxglvPbQtaCdB73sRD2Cqk3p5BJl+7j5nL3a7h
    qG+fh/50tx8bIKuxT8b1Z11dmzzp/2n3YWzW2fP9NsarA4h20ksudYbj/NhVfSbC
    EXffPgK2fPOre3qGNm+499iTcc+G33Mw+nur7SpZyEKEOxEXGlLzyQ4UfaJbcme6
    ce1XR2bFuAJKZTRei9AqPCCcUZlM51Ke92sRKw2Sfh3oius2FkOH6ipjv3U/697E
    A7sKPPcw7+uvTPyLNhBzPvOk
    -----END CERTIFICATE-----
    ---
    Server certificate
    subject=C = US, ST = Massachusetts, L = Cambridge, O = "Akamai Technologies, Inc.", CN = www.akamai.com
    issuer=C = US, O = DigiCert Inc, CN = DigiCert TLS RSA SHA256 2020 CA1
    ---
    No client certificate CA names sent
    Peer signing digest: SHA256
    Peer signature type: ECDSA
    Server Temp Key: X25519, 253 bits
    ---
    SSL handshake has read 3151 bytes and written 526 bytes
    Verification: OK
    ---
    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
    Server public key is 256 bit
    Secure Renegotiation IS NOT supported
    Compression: NONE
    Expansion: NONE
    No ALPN negotiated
    Early data was not sent
    Verify return code: 0 (ok)
    ---

Edit the file:

    nano chain.pem

Delete the first certificate from the file, so that the file remains with only this certificate:

    -----BEGIN CERTIFICATE-----
    MIIEvjCCA6agAwIBAgIQBtjZBNVYQ0b2ii+nVCJ+xDANBgkqhkiG9w0BAQsFADBh
    MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
    d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
    QTAeFw0yMTA0MTQwMDAwMDBaFw0zMTA0MTMyMzU5NTlaME8xCzAJBgNVBAYTAlVT
    MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxKTAnBgNVBAMTIERpZ2lDZXJ0IFRMUyBS
    U0EgU0hBMjU2IDIwMjAgQ0ExMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
    AQEAwUuzZUdwvN1PWNvsnO3DZuUfMRNUrUpmRh8sCuxkB+Uu3Ny5CiDt3+PE0J6a
    qXodgojlEVbbHp9YwlHnLDQNLtKS4VbL8Xlfs7uHyiUDe5pSQWYQYE9XE0nw6Ddn
    g9/n00tnTCJRpt8OmRDtV1F0JuJ9x8piLhMbfyOIJVNvwTRYAIuE//i+p1hJInuW
    raKImxW8oHzf6VGo1bDtN+I2tIJLYrVJmuzHZ9bjPvXj1hJeRPG/cUJ9WIQDgLGB
    Afr5yjK7tI4nhyfFK3TUqNaX3sNk+crOU6JWvHgXjkkDKa77SU+kFbnO8lwZV21r
    eacroicgE7XQPUDTITAHk+qZ9QIDAQABo4IBgjCCAX4wEgYDVR0TAQH/BAgwBgEB
    /wIBADAdBgNVHQ4EFgQUt2ui6qiqhIx56rTaD5iyxZV2ufQwHwYDVR0jBBgwFoAU
    A95QNVbRTLtm8KPiGxvDl7I90VUwDgYDVR0PAQH/BAQDAgGGMB0GA1UdJQQWMBQG
    CCsGAQUFBwMBBggrBgEFBQcDAjB2BggrBgEFBQcBAQRqMGgwJAYIKwYBBQUHMAGG
    GGh0dHA6Ly9vY3NwLmRpZ2ljZXJ0LmNvbTBABggrBgEFBQcwAoY0aHR0cDovL2Nh
    Y2VydHMuZGlnaWNlcnQuY29tL0RpZ2lDZXJ0R2xvYmFsUm9vdENBLmNydDBCBgNV
    HR8EOzA5MDegNaAzhjFodHRwOi8vY3JsMy5kaWdpY2VydC5jb20vRGlnaUNlcnRH
    bG9iYWxSb290Q0EuY3JsMD0GA1UdIAQ2MDQwCwYJYIZIAYb9bAIBMAcGBWeBDAEB
    MAgGBmeBDAECATAIBgZngQwBAgIwCAYGZ4EMAQIDMA0GCSqGSIb3DQEBCwUAA4IB
    AQCAMs5eC91uWg0Kr+HWhMvAjvqFcO3aXbMM9yt1QP6FCvrzMXi3cEsaiVi6gL3z
    ax3pfs8LulicWdSQ0/1s/dCYbbdxglvPbQtaCdB73sRD2Cqk3p5BJl+7j5nL3a7h
    qG+fh/50tx8bIKuxT8b1Z11dmzzp/2n3YWzW2fP9NsarA4h20ksudYbj/NhVfSbC
    EXffPgK2fPOre3qGNm+499iTcc+G33Mw+nur7SpZyEKEOxEXGlLzyQ4UfaJbcme6
    ce1XR2bFuAJKZTRei9AqPCCcUZlM51Ke92sRKw2Sfh3oius2FkOH6ipjv3U/697E
    A7sKPPcw7+uvTPyLNhBzPvOk
    -----END CERTIFICATE-----

**Please note** that this might change in other situations. **The idea is that you have only the chain of certificates without the final end-user certificate**.

### Get the OCSP responder for a certificate

Now we need to obtain the address of the OCSP responder, to know where to connect for querying. This might be done through manual inspection, or by using the command:

    openssl x509 -noout -ocsp_uri -in certificate.pem

### Make the OCSP request

Finally we may do the request to the OCSP responder, using the following command:

    openssl ocsp -issuer chain.pem -cert certificate.pem -text -url http://ocsp.digicert.com

And we obtain the following result.

    OCSP Request Data:
        Version: 1 (0x0)
        Requestor List:
            Certificate ID:
            Hash Algorithm: sha1
            Issuer Name Hash: E4E395A229D3D4C1C31FF0980C0B4EC0098AABD8
            Issuer Key Hash: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
            Serial Number: 064375CD1FAC4BE3C2FA45AF389C1A82
        Request Extensions:
            OCSP Nonce: 
                04102B577E2C2120BFD9AFE5B2724C267D72
    OCSP Response Data:
        OCSP Response Status: successful (0x0)
        Response Type: Basic OCSP Response
        Version: 1 (0x0)
        Responder Id: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
        Produced At: Oct 23 14:24:38 2023 GMT
        Responses:
        Certificate ID:
        Hash Algorithm: sha1
        Issuer Name Hash: E4E395A229D3D4C1C31FF0980C0B4EC0098AABD8
        Issuer Key Hash: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
        Serial Number: 064375CD1FAC4BE3C2FA45AF389C1A82
        Cert Status: good
        This Update: Oct 23 14:09:01 2023 GMT
        Next Update: Oct 30 13:09:01 2023 GMT

        Signature Algorithm: sha256WithRSAEncryption
        Signature Value:
            15:80:c5:88:fc:b4:64:b6:72:75:bb:46:f8:3c:a8:d9:01:81:
            d1:ba:34:0c:dd:6e:c0:0d:3c:60:7f:cb:8a:f2:68:fd:b4:9f:
            93:32:4b:c6:84:db:ba:9d:c0:95:ff:17:83:f4:2e:0b:55:f9:
            42:a6:a9:c5:37:1f:17:13:a8:fe:b0:dc:aa:68:00:9d:cf:fe:
            bb:ca:b0:d5:a8:5f:c9:a0:da:ec:f0:eb:4f:bd:f5:56:9b:01:
            ac:0e:7d:e9:54:f1:82:28:6f:c5:9f:d7:4c:2b:49:ed:87:7f:
            83:f3:09:fc:9b:39:c2:a3:3d:cf:75:b1:a9:43:9f:34:c7:ae:
            72:f9:8f:23:12:4f:54:cf:0d:4e:f2:c7:49:7c:e5:d4:e0:4c:
            92:b1:9d:13:f7:ea:bf:ac:d5:43:28:4a:b8:a3:1c:a0:80:9c:
            da:fa:66:3b:d0:29:25:fe:f8:30:41:4c:f6:e5:f0:4c:9f:c0:
            3c:b4:e3:38:5f:23:df:4d:ff:61:22:e3:c1:b7:2b:96:37:ca:
            48:bc:bd:36:ea:db:6c:07:11:65:fb:2f:d2:23:28:50:de:d5:
            ad:96:12:63:5d:08:11:3a:ef:65:30:0a:72:29:93:6f:fc:7f:
            7d:38:f1:ce:f2:52:fb:69:11:bc:2d:21:e5:04:e7:91:7d:e7:
            33:a8:40:a7
    WARNING: no nonce in response
    Response verify OK
    certificate.pem: good
            This Update: Oct 23 14:09:01 2023 GMT
            Next Update: Oct 30 13:09:01 2023 GMT


From this response we can confirm that the certificate status is `good`.

    Cert Status: good
