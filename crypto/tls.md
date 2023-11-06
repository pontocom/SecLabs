# Transport Layer Security <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Transport Layer Security/ Secure Sockets Layer (SSL/TLS)](#transport-layer-security-secure-sockets-layer-ssltls)
  - [Check a SSL/TLS connection to a server](#check-a-ssltls-connection-to-a-server)
  - [Get a server digital certificate](#get-a-server-digital-certificate)
  - [SSLscan](#sslscan)
  - [SSLyze](#sslyze)
  - [testssl.sh](#testsslsh)
  - [SSL/TLS online tools](#ssltls-online-tools)
- [References/Additional information](#referencesadditional-information)


## Transport Layer Security/ Secure Sockets Layer (SSL/TLS)

Secure Sockets Layer (SSL) is a widely used protocol for secure and authenticated connections between Web browsers and Web servers.

### Check a SSL/TLS connection to a server

OpenSSL can be used to check the parameters of an SSL connection to a particular server, for example:

    openssl s_client -connect www.microsoft.com:443

Resulting in:

    CONNECTED(00000005)
    depth=2 C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root G2
    verify return:1
    depth=1 C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06
    verify return:1
    depth=0 C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = www.microsoft.com
    verify return:1
    ---
    Certificate chain
    0 s:C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = www.microsoft.com
    i:C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06
    1 s:C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06
    i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root G2
    ---
    Server certificate
    -----BEGIN CERTIFICATE-----
    MIII1jCCBr6gAwIBAgITMwBZ+Lbaholwb/ob2QAAAFn4tjANBgkqhkiG9w0BAQwF
    ADBZMQswCQYDVQQGEwJVUzEeMBwGA1UEChMVTWljcm9zb2Z0IENvcnBvcmF0aW9u
    MSowKAYDVQQDEyFNaWNyb3NvZnQgQXp1cmUgVExTIElzc3VpbmcgQ0EgMDYwHhcN
    MjIxMDA0MjMyMzExWhcNMjMwOTI5MjMyMzExWjBoMQswCQYDVQQGEwJVUzELMAkG
    A1UECBMCV0ExEDAOBgNVBAcTB1JlZG1vbmQxHjAcBgNVBAoTFU1pY3Jvc29mdCBD
    b3Jwb3JhdGlvbjEaMBgGA1UEAxMRd3d3Lm1pY3Jvc29mdC5jb20wggEiMA0GCSqG
    SIb3DQEBAQUAA4IBDwAwggEKAoIBAQC3XyMTbW+b+oTRRkirpEjsH/JiF2Dsy8Sj
    KGuCT5SSCO6qfDatzbUFzvMn7TFFAyOEWQhjp5vpAu3zZTu9jeezuXXJx1+2Ey15
    VnZJV3Q78zwJJ/3yTbiXfzhqYWr31Nm3boFzKZWZGZDHgY7epAdYwlOdLQk4QRHm
    3aJNOJWCgc1prSd/XgP+q9sYOnAYVRv9tGtkznTX6QaS82jWjDH8zHi299K8YQpk
    1aophTBJAitrpmBFSHvDgwMpurpx0rjAvq1Q27mTAWG07LMd7J7nyZiFzUxmFDDq
    2ix220yPEmL+wlz1dSqwNaIZi7f67Q9aQ5GnE0A5IPtK0eYp1AbvAgMBAAGjggSG
    MIIEgjCCAX8GCisGAQQB1nkCBAIEggFvBIIBawFpAHcA6D7Q2j71BjUy51covIlr
    yQPTy9ERa+zraeF3fW0GvW4AAAGDpVinhgAABAMASDBGAiEAnc/ZJKQ47TEBYPPo
    qSeM5wRPGELQmAw0i9hYMHUxdukCIQCoQvNpRxMnNH9beub6RbBYSHAIUSbgq6wK
    OTdFbfFNdwB1ALNzdwfhhFD4Y4bWBancEQlKeS2xZwwLh9zwAw55NqWaAAABg6VY
    p8YAAAQDAEYwRAIgPYOx2FZTNMcINed7dn+d75khk9ZstsbyvwPW8V2xtisCICGg
    1wuFd+KukXW8HeDa3mkloREsifgzeTBMGpwVwToMAHcArfe++nz/EMiLnT2cHj4Y
    arRnKV3PsQwkyoWGNOvcgooAAAGDpVinVwAABAMASDBGAiEAttaYStY88Acj2OSc
    nvx1RdlmZRFau8/aMIK/IQEYBisCIQDN/1i/tTTawY67Q4qAFnj1Eh+0kIvwUkrO
    TEV58sFRVjAnBgkrBgEEAYI3FQoEGjAYMAoGCCsGAQUFBwMCMAoGCCsGAQUFBwMB
    MDwGCSsGAQQBgjcVBwQvMC0GJSsGAQQBgjcVCIe91xuB5+tGgoGdLo7QDIfw2h1d
    goTlaYLzpz4CAWQCASUwga4GCCsGAQUFBwEBBIGhMIGeMG0GCCsGAQUFBzAChmFo
    dHRwOi8vd3d3Lm1pY3Jvc29mdC5jb20vcGtpb3BzL2NlcnRzL01pY3Jvc29mdCUy
    MEF6dXJlJTIwVExTJTIwSXNzdWluZyUyMENBJTIwMDYlMjAtJTIweHNpZ24uY3J0
    MC0GCCsGAQUFBzABhiFodHRwOi8vb25lb2NzcC5taWNyb3NvZnQuY29tL29jc3Aw
    HQYDVR0OBBYEFGcNlofXWtjH6XjUmPrgobtxutaHMA4GA1UdDwEB/wQEAwIEsDCB
    mQYDVR0RBIGRMIGOghN3d3dxYS5taWNyb3NvZnQuY29tghF3d3cubWljcm9zb2Z0
    LmNvbYIYc3RhdGljdmlldy5taWNyb3NvZnQuY29tghFpLnMtbWljcm9zb2Z0LmNv
    bYINbWljcm9zb2Z0LmNvbYIRYy5zLW1pY3Jvc29mdC5jb22CFXByaXZhY3kubWlj
    cm9zb2Z0LmNvbTAMBgNVHRMBAf8EAjAAMGQGA1UdHwRdMFswWaBXoFWGU2h0dHA6
    Ly93d3cubWljcm9zb2Z0LmNvbS9wa2lvcHMvY3JsL01pY3Jvc29mdCUyMEF6dXJl
    JTIwVExTJTIwSXNzdWluZyUyMENBJTIwMDYuY3JsMGYGA1UdIARfMF0wUQYMKwYB
    BAGCN0yDfQEBMEEwPwYIKwYBBQUHAgEWM2h0dHA6Ly93d3cubWljcm9zb2Z0LmNv
    bS9wa2lvcHMvRG9jcy9SZXBvc2l0b3J5Lmh0bTAIBgZngQwBAgIwHwYDVR0jBBgw
    FoAU1cFnOsKjnfR3UltZEjgp5lVou6UwHQYDVR0lBBYwFAYIKwYBBQUHAwIGCCsG
    AQUFBwMBMA0GCSqGSIb3DQEBDAUAA4ICAQBxYv4ot1H2312MXlIvccktJlx1/zdq
    HvYlonJRgNH9Upyf69Mpd4bOvuPbV3GFf2kOYk2TrfMIsdgXVAsHhVJlGY/ybYkx
    GnYq3p16cTIDOO1sck352gM3y1n5BK21JMJL/pt1PYIgPnl3ED4DjaD1fucVT93B
    uvkW6N9er5YHQrXG6+gEPD4RZeXhtbIC7zMeBlpcbFG8pxp7vy2NmftNzIOaTkHw
    xT66cGEUgoPEt7ugcJr4nuq6OQ+m+wBZnv14GP+Rjcx6WXGeFIk4POG8XyEDKy22
    +EVODNuvHwWIBCKfxI8rZWf00CcLtVYupyUiAmoovM1TET/41sM2+sOxvTlNdxOL
    pnbVaeiDptMZKuQ0XkFshCJUZ/RhocrjsYNLANYbHoiaJT1lAqU7sTQgQocB34oY
    /Ojy975dBu3DnCebKaXEzjHtujLJ+1fMZ9uUbIP6/FwxL/UqvIpbpTnCoznvnpNE
    ii5x/Jh6Dg+ShpD2QyUE+1LvWhui9GNwm427cnRMHdxNGYHXAuFpGK8/ky3a849p
    xKqx4LNue0y/Rxd+s/EeJRmhF/mYcxzar7G/6T6TjHcjhIR7NTC3y7Q49L3LQdOx
    OtahCotJbc95k+AXudsfPlVLKsYJ6AF9NxN4K2miTmNqz+tmGiKoqbebOTHsd62i
    AstzF05URX9Y4w==
    -----END CERTIFICATE-----
    subject=C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = www.microsoft.com

    issuer=C = US, O = Microsoft Corporation, CN = Microsoft Azure TLS Issuing CA 06

    ---
    No client certificate CA names sent
    Peer signing digest: SHA256
    Peer signature type: RSA-PSS
    Server Temp Key: X25519, 253 bits
    ---
    SSL handshake has read 4368 bytes and written 399 bytes
    Verification: OK
    ---
    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
    Server public key is 2048 bit
    Secure Renegotiation IS NOT supported
    Compression: NONE
    Expansion: NONE
    No ALPN negotiated
    Early data was not sent
    Verify return code: 0 (ok)
    ---
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: 79E4C98601A3400F12807C54D26202A9145A2205ECE8A9ECCF35B82D5391C3C8
        Session-ID-ctx:
        Resumption PSK: BB1D8D7E0BD3B11DB702120DDB89809AA71CDC723FF9912D86142E38BA243E64C5E8FD2EA71A00FB954E5E3475703B1D
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 83100 (seconds)
        TLS session ticket:
        0000 - 00 00 35 6e 11 51 4f da-c0 0e 4e bb 71 b8 9b 41   ..5n.QO...N.q..A
        0010 - 5a be dc a8 31 fa b3 21-d9 a1 77 0f a9 5e 66 c5   Z...1..!..w..^f.
        0020 - 92 9f 82 00 e8 c8 48 cf-ff 16 8a 62 6d 94 af 59   ......H....bm..Y
        0030 - 5d 61 03 77 31 96 aa d2-5b 91 e4 92 29 8c 3d c2   ]a.w1...[...).=.
        0040 - d9 c6 79 eb 0b ce 2a 0d-fe 47 46 2a a9 7f eb a8   ..y...*..GF*....
        0050 - 7e be b6 a2 10 db 75 5c-12 77 3b 96 f5 cd 45 81   ~.....u\.w;...E.
        0060 - 5f ad e0 ff 43 a8 58 77-db 39 36 81 ab b2 6b 36   _...C.Xw.96...k6
        0070 - c4 c1 a9 0b 99 e1 5e a8-34 a1 b4 72 97 78 86 a7   ......^.4..r.x..
        0080 - 33 c7 ab ba 93 cf d7 a9-0c e4 54 4b 83 74 a0 5e   3.........TK.t.^
        0090 - ba e1 a2 99 f7 14 86 79-e4 bd 41 4a cf 8f 93 de   .......y..AJ....
        00a0 - c4 87 80 4e 18 ee 65 68-15 53 8f 2d 56 5c 53 72   ...N..eh.S.-V\Sr
        00b0 - 0b d3 40 05 9a ec 26 ff-2f e7 e0 24 e4 27 02 9c   ..@...&./..$.'..
        00c0 - 22 0b 84 36 39 ac 3a 15-7b 0b 3c 56 83 e2 82 43   "..69.:.{.<V...C
        00d0 - c9 77 41 28 1b cf 45 c7-bc ef 3d f2 9b c1 3b 1b   .wA(..E...=...;.

        Start Time: 1667493148
        Timeout   : 7200 (sec)
        Verify return code: 0 (ok)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: FDF5369ABC630434B2E11B78E29A2FC43FC0732A147D6D7EE1D370FF0D50595E
        Session-ID-ctx:
        Resumption PSK: D63C7DE07685AB98AF1C6CEEBAE63DE012A22290964DB41AC1D3B2817851C8F38677ECD4EE67E9405CE6A44F0F12688B
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 83100 (seconds)
        TLS session ticket:
        0000 - 00 00 35 6e 11 51 4f da-c0 0e 4e bb 71 b8 9b 41   ..5n.QO...N.q..A
        0010 - ad 56 82 f3 74 6d 6d 53-d5 a8 b5 a9 e6 c5 55 e7   .V..tmmS......U.
        0020 - c2 52 2e f2 d6 b5 cb 18-bd 23 82 63 df a6 8e 29   .R.......#.c...)
        0030 - f3 32 c4 51 e5 c6 e2 29-d9 2f 78 43 45 51 5d c0   .2.Q...)./xCEQ].
        0040 - be d8 8e 3b f5 b5 5b 2f-b8 ee ad ae fb bf 2b a7   ...;..[/......+.
        0050 - 42 42 b7 af 4a 23 09 13-f4 2e 1f 58 9a d6 46 90   BB..J#.....X..F.
        0060 - c4 76 f9 41 05 99 af 2e-77 85 d7 12 32 f0 21 97   .v.A....w...2.!.
        0070 - 79 92 8e 39 31 97 f3 56-9b 06 fd 43 be 97 16 aa   y..91..V...C....
        0080 - 66 49 d8 2e 44 54 8f fe-e5 56 e9 5c 70 ed 79 b7   fI..DT...V.\p.y.
        0090 - 93 4c 74 7b db d2 7f 97-76 84 a6 34 d0 67 0f e4   .Lt{....v..4.g..
        00a0 - 3f a3 86 66 14 86 ae 19-0b e8 3d bf 43 ad df 83   ?..f......=.C...
        00b0 - 29 95 b3 8c 52 9d 2d b4-95 7f e8 50 8f ea 05 04   )...R.-....P....
        00c0 - d5 42 b9 07 b9 10 d5 ae-03 ca 16 18 b7 c4 dc f1   .B..............
        00d0 - cc 8a 49 d7 4a 09 8d b2-e5 d8 3a 9a 98 ea b2 0c   ..I.J.....:.....

        Start Time: 1667493148
        Timeout   : 7200 (sec)
        Verify return code: 0 (ok)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    closed

### Get a server digital certificate

We can obtain a server's digital certificate by doing the following:

    echo | openssl s_client -connect www.google.com:443 2>&1 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > google.crt

After we have the certificate saved in a file (`google.crt`) we can view it.

    openssl x509 -noout -text -in ./google.crt 

Resulting:

    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                31:68:f6:7c:25:93:0f:e5
            Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=US, O=Google Inc, CN=Google Internet Authority G2
            Validity
                Not Before: Oct 28 17:50:22 2015 GMT
                Not After : Jan 26 00:00:00 2016 GMT
            Subject: C=US, ST=California, L=Mountain View, O=Google Inc, CN=www.google.com
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                RSA Public Key: (2048 bit)
                    Modulus (2048 bit):
                        00:b4:4a:88:99:2c:74:01:6a:d4:4a:17:8a:b6:22:
                        34:e1:b7:91:c2:63:22:68:13:3f:f4:31:57:cd:91:
                        d8:c9:b6:b5:f4:77:19:7a:21:67:88:f4:b3:3e:cd:
                        64:2f:0d:ca:bf:f7:20:0c:1b:03:db:27:3e:46:da:
                        82:0f:fe:81:41:85:40:ae:bc:fe:8d:a8:a5:a6:92:
                        54:90:e2:d1:74:c6:1e:a5:ce:3e:32:4f:04:b9:67:
                        d1:e2:59:a3:1b:7d:d9:68:15:b2:f0:90:a4:a5:30:
                        16:3e:5f:6a:d9:07:14:d1:86:05:9c:38:e0:73:65:
                        e4:d4:4a:94:b3:93:e5:b2:06:23:14:d4:f3:e7:cf:
                        35:b7:45:ec:e9:07:dd:e0:bc:cb:5b:23:88:3a:1e:
                        8e:7e:02:fa:b7:83:2e:8f:9a:5c:f5:50:10:f2:f0:
                        3d:9b:d5:af:29:19:b3:39:7f:31:69:cb:bc:a7:36:
                        54:87:a0:c0:2a:55:d3:91:57:3e:97:83:98:e3:47:
                        65:8b:e8:32:98:43:cd:c1:b1:8b:a7:55:1e:73:0e:
                        81:2f:b4:5d:9c:e1:c1:cf:a7:2e:6f:b0:30:60:5d:
                        61:a7:02:b7:bc:6b:e9:0d:b8:00:78:ca:9f:fa:70:
                        8d:1f:f1:2b:a4:f0:a6:02:72:f4:23:35:e0:78:1c:
                        19:3d
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Extended Key Usage: 
                    TLS Web Server Authentication, TLS Web Client Authentication
                X509v3 Subject Alternative Name: 
                    DNS:www.google.com
                Authority Information Access: 
                    CA Issuers - URI:http://pki.google.com/GIAG2.crt
                    OCSP - URI:http://clients1.google.com/ocsp

                X509v3 Subject Key Identifier: 
                    95:CA:1C:F5:FB:39:28:C9:1C:7D:D2:3C:0E:85:68:01:7E:98:7B:4C
                X509v3 Basic Constraints: critical
                    CA:FALSE
                X509v3 Authority Key Identifier: 
                    keyid:4A:DD:06:16:1B:BC:F6:68:B5:76:F5:81:B6:BB:62:1A:BA:5A:81:2F

                X509v3 Certificate Policies: 
                    Policy: 1.3.6.1.4.1.11129.2.5.1
                    Policy: 2.23.140.1.2.2

                X509v3 CRL Distribution Points: 
                    URI:http://pki.google.com/GIAG2.crl

        Signature Algorithm: sha256WithRSAEncryption
            36:57:20:af:df:78:82:4b:bf:83:98:01:06:db:c6:f1:c0:b8:
            6d:b3:8c:ba:38:f3:46:d6:0b:2e:7a:5e:01:42:ca:29:90:37:
            51:05:3c:e8:b3:f8:e8:42:91:0b:25:11:94:5d:f5:bc:eb:d3:
            f0:37:79:a3:c0:03:f9:f3:1e:d9:61:a7:2a:a1:81:12:db:29:
            2f:31:ee:8c:80:b2:e3:a0:5c:e4:03:97:93:31:94:44:23:fb:
            4a:48:e2:39:e1:0d:1b:b6:49:66:6d:7b:2d:fb:69:9d:00:2c:
            62:7c:dd:5c:cd:f1:4c:a0:35:cd:57:36:12:49:10:33:3e:7f:
            e7:55:ac:f5:a5:f8:0e:e9:cd:51:fc:1a:25:fe:41:8c:6f:1a:
            c1:f8:70:f9:f0:e2:b4:28:b1:ea:d9:49:f9:5e:1e:e3:51:4d:
            51:59:6e:0f:26:91:2c:a6:69:37:df:98:a8:95:dd:3e:bc:fd:
            9a:ee:4f:d4:bc:31:40:11:2c:e7:d1:2f:36:e6:26:7b:af:e6:
            6f:41:9e:f4:27:3d:0b:b8:11:f4:67:09:08:ef:40:de:0c:ad:
            fe:81:65:b6:4a:2d:de:02:78:73:43:c7:2c:06:18:b3:75:fd:
            54:dd:f7:c9:1d:ad:6c:b2:aa:70:56:7b:e3:9f:8e:e3:86:63:
            e6:b8:10:fc

### SSLscan

[SSLscan](https://github.com/rbsec/sslscan) is a tool that can be used to test the SSL/TLS protocol on a specific server and potencialy find some problems or even vulnerabilities.

Using it is quite simple:

    sslscan www.google.com

Which results in the fllowing output.

    Version: 2.0.15
    OpenSSL 3.0.7 1 Nov 2022

    Connected to 172.217.17.4

    Testing SSL server www.google.com on port 443 using SNI name www.google.com

    SSL/TLS Protocols:
    SSLv2     disabled
    SSLv3     disabled
    TLSv1.0   enabled
    TLSv1.1   enabled
    TLSv1.2   enabled
    TLSv1.3   enabled

    TLS Fallback SCSV:
    Server supports TLS Fallback SCSV

    TLS renegotiation:
    Secure session renegotiation supported

    TLS Compression:
    OpenSSL version does not support compression
    Rebuild with zlib1g-dev package for zlib support

    Heartbleed:
    TLSv1.3 not vulnerable to heartbleed
    TLSv1.2 not vulnerable to heartbleed
    TLSv1.1 not vulnerable to heartbleed
    TLSv1.0 not vulnerable to heartbleed

    Supported Server Cipher(s):
    Preferred TLSv1.3  128 bits  TLS_AES_128_GCM_SHA256        Curve 25519 DHE 253
    Accepted  TLSv1.3  256 bits  TLS_AES_256_GCM_SHA384        Curve 25519 DHE 253
    Accepted  TLSv1.3  256 bits  TLS_CHACHA20_POLY1305_SHA256  Curve 25519 DHE 253
    Preferred TLSv1.2  256 bits  ECDHE-ECDSA-CHACHA20-POLY1305 Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-ECDSA-AES128-GCM-SHA256 Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-ECDSA-AES256-GCM-SHA384 Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-ECDSA-AES128-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-ECDSA-AES256-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-RSA-CHACHA20-POLY1305   Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-RSA-AES128-GCM-SHA256   Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-GCM-SHA384   Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  ECDHE-RSA-AES128-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.2  128 bits  AES128-GCM-SHA256
    Accepted  TLSv1.2  256 bits  AES256-GCM-SHA384
    Accepted  TLSv1.2  128 bits  AES128-SHA
    Accepted  TLSv1.2  256 bits  AES256-SHA
    Accepted  TLSv1.2  112 bits  TLS_RSA_WITH_3DES_EDE_CBC_SHA
    Preferred TLSv1.1  128 bits  ECDHE-ECDSA-AES128-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.1  256 bits  ECDHE-ECDSA-AES256-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.1  128 bits  ECDHE-RSA-AES128-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.1  256 bits  ECDHE-RSA-AES256-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.1  128 bits  AES128-SHA
    Accepted  TLSv1.1  256 bits  AES256-SHA
    Accepted  TLSv1.1  112 bits  TLS_RSA_WITH_3DES_EDE_CBC_SHA
    Preferred TLSv1.0  128 bits  ECDHE-ECDSA-AES128-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.0  256 bits  ECDHE-ECDSA-AES256-SHA        Curve 25519 DHE 253
    Accepted  TLSv1.0  128 bits  ECDHE-RSA-AES128-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.0  256 bits  ECDHE-RSA-AES256-SHA          Curve 25519 DHE 253
    Accepted  TLSv1.0  128 bits  AES128-SHA
    Accepted  TLSv1.0  256 bits  AES256-SHA
    Accepted  TLSv1.0  112 bits  TLS_RSA_WITH_3DES_EDE_CBC_SHA

    Server Key Exchange Group(s):
    TLSv1.3  128 bits  secp256r1 (NIST P-256)
    TLSv1.3  128 bits  x25519
    TLSv1.2  128 bits  secp256r1 (NIST P-256)
    TLSv1.2  128 bits  x25519

    SSL Certificate:
    Signature Algorithm: sha256WithRSAEncryption
    ECC Curve Name:      prime256v1
    ECC Key Strength:    128

    Subject:  www.google.com
    Altnames: DNS:www.google.com
    Issuer:   GTS CA 1C3

    Not valid before: Sep 26 08:23:57 2022 GMT
    Not valid after:  Dec 19 08:23:56 2022 GMT

### SSLyze

[SSLyze](https://github.com/nabla-c0d3/sslyze) is also a tool that might be use to scan for SSL/TLS on a server. Using it is also very simple.

    sslyze www.google.com

Which results in the following.

    CHECKING CONNECTIVITY TO SERVER(S)
    ----------------------------------

    www.google.com:443        => 142.250.200.132


    SCAN RESULTS FOR WWW.GOOGLE.COM:443 - 142.250.200.132
    -----------------------------------------------------

    * Certificates Information:
        Hostname sent for SNI:             www.google.com
        Number of certificates detected:   2


        Certificate #0 ( _EllipticCurvePublicKey )
        SHA1 Fingerprint:                  8d451b91f4f3d1beaf7dfd745739a0a1ff9bb7b2
        Common Name:                       www.google.com
        Issuer:                            GTS CA 1C3
        Serial Number:                     172338986348961426169022814442718324929
        Not Before:                        2022-10-17
        Not After:                         2023-01-09
        Public Key Algorithm:              _EllipticCurvePublicKey
        Signature Algorithm:               sha256
        Key Size:                          256
        Curve:                             secp256r1
        DNS Subject Alternative Names:     ['www.google.com']

        Certificate #0 - Trust
        Hostname Validation:               OK - Certificate matches server hostname
        Android CA Store (13.0.0_r8):      OK - Certificate is trusted
        Apple CA Store (iOS 15.1, iPadOS 15.1, macOS 12.1, tvOS 15.1, and watchOS 8.1):OK - Certificate is trusted
        Java CA Store (jdk-13.0.2):        OK - Certificate is trusted
        Mozilla CA Store (2022-09-18):     OK - Certificate is trusted
        Windows CA Store (2022-08-15):     OK - Certificate is trusted
        Symantec 2018 Deprecation:         OK - Not a Symantec-issued certificate
        Received Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Verified Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Received Chain Contains Anchor:    OK - Anchor certificate not sent
        Received Chain Order:              OK - Order is valid
        Verified Chain contains SHA1:      OK - No SHA1-signed certificate in the verified certificate chain

        Certificate #0 - Extensions
        OCSP Must-Staple:                  NOT SUPPORTED - Extension not found
        Certificate Transparency:          WARNING - Only 2 SCTs included but Google recommends 3 or more

        Certificate #0 - OCSP Stapling
                                            NOT SUPPORTED - Server did not send back an OCSP response


        Certificate #1 ( _RSAPublicKey )
        SHA1 Fingerprint:                  bb181548dc5db04f8194e588b4f19f1cc4d12a0d
        Common Name:                       www.google.com
        Issuer:                            GTS CA 1C3
        Serial Number:                     265906118735752094790635674560926019293
        Not Before:                        2022-10-17
        Not After:                         2023-01-09
        Public Key Algorithm:              _RSAPublicKey
        Signature Algorithm:               sha256
        Key Size:                          2048
        Exponent:                          65537
        DNS Subject Alternative Names:     ['www.google.com']

        Certificate #1 - Trust
        Hostname Validation:               OK - Certificate matches server hostname
        Android CA Store (13.0.0_r8):      OK - Certificate is trusted
        Apple CA Store (iOS 15.1, iPadOS 15.1, macOS 12.1, tvOS 15.1, and watchOS 8.1):OK - Certificate is trusted
        Java CA Store (jdk-13.0.2):        OK - Certificate is trusted
        Mozilla CA Store (2022-09-18):     OK - Certificate is trusted
        Windows CA Store (2022-08-15):     OK - Certificate is trusted
        Symantec 2018 Deprecation:         OK - Not a Symantec-issued certificate
        Received Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Verified Chain:                    www.google.com --> GTS CA 1C3 --> GTS Root R1
        Received Chain Contains Anchor:    OK - Anchor certificate not sent
        Received Chain Order:              OK - Order is valid
        Verified Chain contains SHA1:      OK - No SHA1-signed certificate in the verified certificate chain

        Certificate #1 - Extensions
        OCSP Must-Staple:                  NOT SUPPORTED - Extension not found
        Certificate Transparency:          WARNING - Only 2 SCTs included but Google recommends 3 or more

        Certificate #1 - OCSP Stapling
                                            NOT SUPPORTED - Server did not send back an OCSP response

    * SSL 2.0 Cipher Suites:
        Attempted to connect using 7 cipher suites; the server rejected all cipher suites.

    * SSL 3.0 Cipher Suites:
        Attempted to connect using 80 cipher suites; the server rejected all cipher suites.

    * TLS 1.0 Cipher Suites:
        Attempted to connect using 80 cipher suites.

        The server accepted the following 5 cipher suites:
            TLS_RSA_WITH_AES_256_CBC_SHA                      256
            TLS_RSA_WITH_AES_128_CBC_SHA                      128
            TLS_RSA_WITH_3DES_EDE_CBC_SHA                     168
            TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA                256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA                128       ECDH: prime256v1 (256 bits)

        The group of cipher suites supported by the server has the following properties:
        Forward Secrecy                    OK - Supported
        Legacy RC4 Algorithm               OK - Not Supported


    * TLS 1.1 Cipher Suites:
        Attempted to connect using 80 cipher suites.

        The server accepted the following 5 cipher suites:
            TLS_RSA_WITH_AES_256_CBC_SHA                      256
            TLS_RSA_WITH_AES_128_CBC_SHA                      128
            TLS_RSA_WITH_3DES_EDE_CBC_SHA                     168
            TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA                256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA                128       ECDH: prime256v1 (256 bits)

        The group of cipher suites supported by the server has the following properties:
        Forward Secrecy                    OK - Supported
        Legacy RC4 Algorithm               OK - Not Supported


    * TLS 1.2 Cipher Suites:
        Attempted to connect using 156 cipher suites.

        The server accepted the following 11 cipher suites:
            TLS_RSA_WITH_AES_256_GCM_SHA384                   256
            TLS_RSA_WITH_AES_256_CBC_SHA                      256
            TLS_RSA_WITH_AES_128_GCM_SHA256                   128
            TLS_RSA_WITH_AES_128_CBC_SHA                      128
            TLS_RSA_WITH_3DES_EDE_CBC_SHA                     168
            TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256       256       ECDH: X25519 (253 bits)
            TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384             256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA                256       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256             128       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA                128       ECDH: prime256v1 (256 bits)
            TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256     256       ECDH: X25519 (253 bits)

        The group of cipher suites supported by the server has the following properties:
        Forward Secrecy                    OK - Supported
        Legacy RC4 Algorithm               OK - Not Supported


    * TLS 1.3 Cipher Suites:
        Attempted to connect using 5 cipher suites.

        The server accepted the following 3 cipher suites:
            TLS_CHACHA20_POLY1305_SHA256                      256       ECDH: X25519 (253 bits)
            TLS_AES_256_GCM_SHA384                            256       ECDH: X25519 (253 bits)
            TLS_AES_128_GCM_SHA256                            128       ECDH: X25519 (253 bits)


    * Deflate Compression:
                                            OK - Compression disabled

    * OpenSSL CCS Injection:
                                            OK - Not vulnerable to OpenSSL CCS injection

    * OpenSSL Heartbleed:
                                            OK - Not vulnerable to Heartbleed

    * ROBOT Attack:
                                            OK - Not vulnerable.

    * Session Renegotiation:
        Client Renegotiation DoS Attack:   OK - Not vulnerable
        Secure Renegotiation:              OK - Supported

    * Elliptic Curve Key Exchange:
        Supported curves:                  X25519, prime256v1
        Rejected curves:                   X448, prime192v1, secp160k1, secp160r1, secp160r2, secp192k1, secp224k1, secp224r1, secp256k1, secp384r1, secp521r1, sect163k1, sect163r1, sect163r2, sect193r1, sect193r2, sect233k1, sect233r1, sect239k1, sect283k1, sect283r1, sect409k1, sect409r1, sect571k1, sect571r1

    SCANS COMPLETED IN 4.242441 S
    -----------------------------

    COMPLIANCE AGAINST MOZILLA TLS CONFIGURATION
    --------------------------------------------

        Checking results against Mozilla's "intermediate" configuration. See https://ssl-config.mozilla.org/ for more details.

        www.google.com:443: FAILED - Not compliant.
            * tls_versions: TLS versions {'TLSv1', 'TLSv1.1'} are supported, but should be rejected.
            * ciphers: Cipher suites {'TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA', 'TLS_RSA_WITH_AES_256_CBC_SHA', 'TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA', 'TLS_RSA_WITH_AES_128_CBC_SHA', 'TLS_RSA_WITH_3DES_EDE_CBC_SHA', 'TLS_RSA_WITH_AES_256_GCM_SHA384', 'TLS_RSA_WITH_AES_128_GCM_SHA256'} are supported, but should be rejected.

### testssl.sh

Another interesting tool to test SSL/TLS is to use the [testssl.sh](https://github.com/drwetter/testssl.sh) tool. It is also adequate to find problems on SSL/TLS servers.

To use it simply execute:

    testssl.sh carlos.serrao.me

The obtained output is something like this (just a small portion):

    Testing vulnerabilities

    Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
    CCS (CVE-2014-0224)                       not vulnerable (OK)
    Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK), no session ticket extension
    ROBOT                                     Server does not support any cipher suites that use RSA key transport
    Secure Renegotiation (RFC 5746)           supported (OK)
    Secure Client-Initiated Renegotiation     not vulnerable (OK)
    CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
    BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                            Can be ignored for static pages or if no secrets in the page
    POODLE, SSL (CVE-2014-3566)               not vulnerable (OK), no SSLv3 support
    TLS_FALLBACK_SCSV (RFC 7507)              No fallback possible (OK), no protocol below TLS 1.2 offered
    SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
    FREAK (CVE-2015-0204)                     not vulnerable (OK)
    DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                            make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                            https://search.censys.io/search?resource=hosts&virtual_hosts=INCLUDE&q=C5D0C2C5E14B71CF7342F4BEAB81322EF9AD9B9BEEBB3D72ACB5E6360EA08C15
    LOGJAM (CVE-2015-4000), experimental      common prime with 2048 bits detected: RFC3526/Oakley Group 14 (2048 bits),
                                            but no DH EXPORT ciphers
    BEAST (CVE-2011-3389)                     not vulnerable (OK), no SSL3 or TLS1
    LUCKY13 (CVE-2013-0169), experimental     not vulnerable (OK)
    RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)

### SSL/TLS online tools

The following list presents some online tools for testing also SSL/TLS.

1. [SSL Server Test by Qualys](https://www.ssllabs.com/ssltest/)
2. [SSL Checker by SSL Shopper](https://www.sslshopper.com/ssl-checker.html)
3. [SSL Scanner by SSL Tools](http://www.ssltools.com/)
4. [SSL Certificate Checker by Digicert](https://www.digicert.com/help/)
5. [SSL Check by JitBit](https://www.jitbit.com/sslcheck/)
6. [SSL Checker by SSL Store](https://www.thesslstore.com/ssltools/ssl-checker.php)
7. [SSL Tester by Wormly](https://www.wormly.com/test_ssl)
8. [SSL Security Test by ImmuniWeb](https://www.immuniweb.com/ssl/)
9. [SSL Certificate Checker](https://www.sslchecker.com/sslchecker)
10. [SSLv3 Poodle Vulnerability Scanner](https://pentest-tools.com/network-vulnerability-scanning/ssl-poodle-scanner)

## References/Additional information

1. [https://openssl.org/docs/manmaster/apps/openssl.html](https://openssl.org/docs/manmaster/apps/openssl.html)
2. [http://www.g-loaded.eu/2005/11/10/be-your-own-ca/](http://www.g-loaded.eu/2005/11/10/be-your-own-ca/)
3. [https://jamielinux.com/docs/openssl-certificate-authority/index.html](https://jamielinux.com/docs/openssl-certificate-authority/index.html)
4. [http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html](http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html)
5. [https://www.openssl.org/docs/manmaster/apps/config.html](https://www.openssl.org/docs/manmaster/apps/config.html)
6. [https://www.mkssoftware.com/docs/man1/openssl_smime.1.asp](https://www.mkssoftware.com/docs/man1/openssl_smime.1.asp)
7. [https://www.madboa.com/geek/openssl/](https://www.madboa.com/geek/openssl/)
8. [https://www.misterpki.com/openssl-verify/](https://www.misterpki.com/openssl-verify/)
9. [https://www.misterpki.com/openssl-s-client/](https://www.misterpki.com/openssl-s-client/)

