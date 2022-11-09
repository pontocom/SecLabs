# OCSP - Online Certificate Status Protocol <!-- omit in toc -->

## Index <!-- omit in toc -->

- [OCSP - Online Certificate Status Protocol](#ocsp---online-certificate-status-protocol)
  - [Getting the server certificate](#getting-the-server-certificate)
  - [Getting the intermediate certificates](#getting-the-intermediate-certificates)
  - [Get the OCSP responder for a certificate](#get-the-ocsp-responder-for-a-certificate)
  - [Make the OCSP request](#make-the-ocsp-request)


## OCSP - Online Certificate Status Protocol

[OCSP](https://www.ietf.org/rfc/rfc2560.txt) is an interactive way to enable applications to check for the revokation of an identified certificate. In order to test for OCSP validity, we require the certificate that we are going to test, the certificate chain of such certificate, and the OCSP responder address.

### Getting the server certificate

Get the server certificate that we want to check for OCSP validity:

    openssl s_client -connect www.akamai.com:443 < /dev/null 2>&1 |  sed -n '/-----BEGIN/,/-----END/p' > certificate.pem

Certificate will be store on the `certificate.pem` file.

### Getting the intermediate certificates

Getting the multiple intermediary certificates for an entity.

    openssl s_client -showcerts -connect www.akamai.com:443 < /dev/null 2>&1 |  sed -n '/-----BEGIN/,/-----END/p' > chain.pem

These certificates will be store in `chain.pem` file.

### Get the OCSP responder for a certificate

Now we need to obatin the address of the OCSP responder, to know where to connect for querying. This might be done through manual inspection, or by using the command:

    openssl x509 -noout -ocsp_uri -in certificate.pem

### Make the OCSP request

Finnaly we may do the request to the OCSP responder, using the following command:

    openssl ocsp -issuer chain.pem -cert certificate.pem -text -url http://ocsp.digicert.com

And we obtain the following result.

    OCSP Request Data:
        Version: 1 (0x0)
        Requestor List:
            Certificate ID:
            Hash Algorithm: sha1
            Issuer Name Hash: E4E395A229D3D4C1C31FF0980C0B4EC0098AABD8
            Issuer Key Hash: FCEA9148BBCE3C6AA8902FBDD1DE95038C40809C
            Serial Number: 0A1E4B89556FDE711D7BB2E2DFFF804F
        Request Extensions:
            OCSP Nonce:
                041007AEEFEBA331089CA9113F7E4646D525
    OCSP Response Data:
        OCSP Response Status: successful (0x0)
        Response Type: Basic OCSP Response
        Version: 1 (0x0)
        Responder Id: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
        Produced At: Nov  2 20:31:11 2022 GMT
        Responses:
        Certificate ID:
        Hash Algorithm: sha1
        Issuer Name Hash: E4E395A229D3D4C1C31FF0980C0B4EC0098AABD8
        Issuer Key Hash: B76BA2EAA8AA848C79EAB4DA0F98B2C59576B9F4
        Serial Number: 0A1E4B89556FDE711D7BB2E2DFFF804F
        Cert Status: good
        This Update: Nov  2 20:15:01 2022 GMT
        Next Update: Nov  9 19:30:01 2022 GMT

        Signature Algorithm: sha256WithRSAEncryption
            9f:e5:42:ae:ea:a5:89:98:97:4d:7c:9b:77:e2:94:2d:63:46:
            3a:66:79:cb:3a:cd:f6:24:c9:56:39:97:e3:8d:a8:1d:42:c0:
            60:3b:12:ab:3c:b5:7a:3e:7c:ef:1e:0b:c9:97:aa:c2:42:79:
            59:fe:b4:b5:80:58:e7:d9:f5:3c:39:bd:1d:78:77:8a:b2:26:
            be:93:fc:bf:cb:92:9d:97:93:31:ca:dc:2b:a1:33:c3:e1:12:
            45:5f:82:2f:24:00:8f:99:a3:8b:5c:61:84:be:eb:4f:90:8a:
            48:31:43:f0:7d:93:8d:6d:16:6f:a6:42:c5:00:65:b9:26:e7:
            3d:8e:65:3d:0b:3e:2a:67:a4:fa:d7:6c:4b:f4:1c:6d:da:e0:
            17:8e:78:74:f9:4a:fe:4e:a1:6e:c5:c7:20:1e:cb:72:69:64:
            bf:f5:3b:6f:35:34:a7:5f:f3:54:af:3a:c2:42:f2:e8:a0:a5:
            cc:ad:84:9e:5a:cd:c6:2d:1b:47:7c:40:01:08:56:f9:80:5e:
            29:80:ac:0f:71:3b:a8:c8:5a:f6:e7:a0:dd:76:1e:e3:8a:93:
            d4:3d:57:7e:f3:30:97:e3:9d:d3:8a:fe:0b:6b:23:af:4b:8b:
            3e:4c:bf:e8:dc:b8:d2:61:3c:de:68:fa:ce:5c:d9:46:a2:f7:
            81:f5:51:e1
    WARNING: no nonce in response
    Response Verify Failure
    140704274896064:error:27FFF076:OCSP routines:CRYPTO_internal:signer certificate not found:/AppleInternal/Library/BuildRoots/810eba08-405a-11ed-86e9-6af958a02716/Library/Caches/com.apple.xbs/Sources/libressl/libressl-3.3/crypto/ocsp/ocsp_vfy.c:89:
    certificate.pem: ERROR: No Status found.