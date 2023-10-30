# Diffie-Hellman (DH)  <!-- omit in toc -->

## Index <!-- omit in toc -->

- [Using Diffie-Hellman (DH) for key exchange](#using-diffie-hellman-dh-for-key-exchange)
  - [Generate DH public key parameters](#generate-dh-public-key-parameters)
  - [Generate the DH private and public keys](#generate-the-dh-private-and-public-keys)
  - [Encrypt and decrypt with DH](#encrypt-and-decrypt-with-dh)


## Using Diffie-Hellman (DH) for key exchange

Diffie-Hellman is one of the oldest public-key cryptosystems. It is often called a **_key-agreement protocol_**, because it allows the establishment of a common secret key, that can be used to encrypt and decrypt messages.

### Generate DH public key parameters

Generate the DH public key parameters and save them to a file (this may take a while to complete depending on the key size - you need to be patient).

    openssl dhparam -out dhparams.pem 2048

Here are the parameters generated in base64.

    -----BEGIN DH PARAMETERS-----
    MIIBCAKCAQEA/J7CXLjNaOZoL9QNkhlPI8iGjuI1hVLHKCxdLP5LhLqqPfOiKLBE
    8wU8LgiAOiH7jMW4BfohuoCRG7E8xWnplt1Rn1SAiiZzdeKKY4t08fiCAmO3EUHj
    sxXPwV537IbbN2d2MRxIodCla9nIx/x906foXfhpr4mi3k+dfg5lv/2HOnjST0qR
    NccwjqH+k3ye03bhuB6D9pEL2prRrDxn13q4UGoh0Uwp/wcxHQk1sy6ouvaaEJYm
    OKemxWVZ19EPOpIVxvgVTa0e28z7zlDP/9+xN7fCu46Irv0SWVxa2JnAogP308rk
    8xAgeRJA4lRKOAgFb2dJZSNT65w19SeBqwIBAg==
    -----END DH PARAMETERS-----

To view the contents of the parameters of the file, please do:

    openssl pkeyparam -in dhparams.pem -text

And you'll have access to the parameters created.

    -----BEGIN DH PARAMETERS-----
    MIIBCAKCAQEA/J7CXLjNaOZoL9QNkhlPI8iGjuI1hVLHKCxdLP5LhLqqPfOiKLBE
    8wU8LgiAOiH7jMW4BfohuoCRG7E8xWnplt1Rn1SAiiZzdeKKY4t08fiCAmO3EUHj
    sxXPwV537IbbN2d2MRxIodCla9nIx/x906foXfhpr4mi3k+dfg5lv/2HOnjST0qR
    NccwjqH+k3ye03bhuB6D9pEL2prRrDxn13q4UGoh0Uwp/wcxHQk1sy6ouvaaEJYm
    OKemxWVZ19EPOpIVxvgVTa0e28z7zlDP/9+xN7fCu46Irv0SWVxa2JnAogP308rk
    8xAgeRJA4lRKOAgFb2dJZSNT65w19SeBqwIBAg==
    -----END DH PARAMETERS-----
    DH Parameters: (2048 bit)
        prime:
            00:fc:9e:c2:5c:b8:cd:68:e6:68:2f:d4:0d:92:19:
            4f:23:c8:86:8e:e2:35:85:52:c7:28:2c:5d:2c:fe:
            4b:84:ba:aa:3d:f3:a2:28:b0:44:f3:05:3c:2e:08:
            80:3a:21:fb:8c:c5:b8:05:fa:21:ba:80:91:1b:b1:
            3c:c5:69:e9:96:dd:51:9f:54:80:8a:26:73:75:e2:
            8a:63:8b:74:f1:f8:82:02:63:b7:11:41:e3:b3:15:
            cf:c1:5e:77:ec:86:db:37:67:76:31:1c:48:a1:d0:
            a5:6b:d9:c8:c7:fc:7d:d3:a7:e8:5d:f8:69:af:89:
            a2:de:4f:9d:7e:0e:65:bf:fd:87:3a:78:d2:4f:4a:
            91:35:c7:30:8e:a1:fe:93:7c:9e:d3:76:e1:b8:1e:
            83:f6:91:0b:da:9a:d1:ac:3c:67:d7:7a:b8:50:6a:
            21:d1:4c:29:ff:07:31:1d:09:35:b3:2e:a8:ba:f6:
            9a:10:96:26:38:a7:a6:c5:65:59:d7:d1:0f:3a:92:
            15:c6:f8:15:4d:ad:1e:db:cc:fb:ce:50:cf:ff:df:
            b1:37:b7:c2:bb:8e:88:ae:fd:12:59:5c:5a:d8:99:
            c0:a2:03:f7:d3:ca:e4:f3:10:20:79:12:40:e2:54:
            4a:38:08:05:6f:67:49:65:23:53:eb:9c:35:f5:27:
            81:ab
        generator: 2 (0x2)

### Generate the DH private and public keys

Using the DH public parameters that were created in the previous step, it is now necessary to create the **public** and **private** keys of both entities that need to communicate (again **Alice** and **Bob**).

So, both **Alice** and **Bob**, need to have the `dhparams.pem` file created in the previous step.

Now, **Alice** can do the following to create its private key:

    openssl genpkey -paramfile dhparams.pem -out alice_private.dh

Let’s look at the content of **Alice** private key:

    openssl pkey -in alice_private.dh -text -noout

This is the contents of the private key:

    DH Private-Key: (2048 bit)
        private-key:
            68:fd:1e:8b:01:a1:6b:b1:80:2d:a5:9d:19:f5:74:
            5c:5a:9f:d2:ea:a0:64:69:3e:99:e4:e0:85:fc:e5:
            d0:cc:f5:f9:76:61:07:22:4c:1f:2b:7b:f2:9a:6b:
            0c:dc:11:1c:70:7c:29:ff:e9:13:48:7b:01:b2:11:
            e9:13:92:a1:a9:59:22:2c:de:66:55:d5:c2:12:93:
            cf:d4:ed:d8:46:45:2b:86:32:c8:06:ff:33:58:43:
            02:a5:df:f1:8d:19:a9:d6:26:c2:04:71:54:65:55:
            67:d3:8f:e2:48:ba:e7:58:f2:58:18:f3:56:5a:ca:
            b2:ae:3a:08:a1:d2:fc:6e:95:31:7e:d3:94:cf:72:
            66:2c:58:5e:3f:16:b2:3b:a3:0a:d4:00:b7:e4:e3:
            34:5f:91:e6:60:75:3c:7f:ae:a9:37:37:2b:e4:e2:
            f4:82:0a:28:5a:18:b2:56:f8:26:81:76:8e:1f:07:
            9d:f0:de:a6:6c:34:34:44:ce:20:d2:fb:55:7c:07:
            28:f9:cd:02:81:7d:0d:bd:57:7b:13:a9:1d:fd:f6:
            15:39:d1:ce:03:ff:ef:5b:32:6d:5c:f5:2d:66:76:
            ae:70:5b:a8:f3:f0:34:fd:b6:cc:09:32:a7:02:78:
            e1:9c:0d:cd:c5:43:59:db:36:fc:ae:f3:61:4f:27:
            18
        public-key:
            75:e6:9b:4b:d2:6a:f3:6e:07:e2:0f:10:50:e9:fd:
            a2:4c:99:8b:88:59:85:e3:5a:a4:f6:82:72:c3:55:
            bc:94:b3:6c:08:d9:30:e8:15:33:42:cd:bb:7d:3b:
            8f:0f:82:b8:94:b9:de:60:34:cd:f0:61:00:3f:01:
            6a:19:04:f9:04:63:9d:06:94:5c:22:22:62:86:b8:
            21:5b:c4:2c:be:00:6a:ad:e3:a7:13:57:31:04:9c:
            a1:df:01:20:11:af:83:2e:2e:cd:9c:7b:06:2f:a4:
            ae:c5:31:01:8a:39:b5:e1:80:53:b0:fe:0c:00:6d:
            76:0c:9c:69:09:0c:44:98:c8:b5:7e:d1:ed:58:db:
            39:05:b8:c0:71:20:64:1f:71:3b:b3:69:5b:9b:3c:
            11:3d:ce:db:dc:2f:72:2d:ee:4d:75:8c:e5:07:f1:
            ad:e1:08:a9:62:25:ba:67:13:83:00:85:97:d2:74:
            be:ea:1e:cd:11:4e:bb:4c:e1:01:99:3f:5a:86:69:
            5e:23:ec:6f:a8:ce:58:f7:3d:cf:50:7f:7d:08:0c:
            b7:74:47:f7:5e:76:89:fb:97:44:05:53:16:98:2a:
            d7:ca:38:c1:b2:a1:3d:6e:32:38:46:95:f2:fd:6e:
            6f:a9:be:47:78:68:04:37:52:b8:4f:b0:a7:f3:8d:
            9c
        prime:
            00:fc:9e:c2:5c:b8:cd:68:e6:68:2f:d4:0d:92:19:
            4f:23:c8:86:8e:e2:35:85:52:c7:28:2c:5d:2c:fe:
            4b:84:ba:aa:3d:f3:a2:28:b0:44:f3:05:3c:2e:08:
            80:3a:21:fb:8c:c5:b8:05:fa:21:ba:80:91:1b:b1:
            3c:c5:69:e9:96:dd:51:9f:54:80:8a:26:73:75:e2:
            8a:63:8b:74:f1:f8:82:02:63:b7:11:41:e3:b3:15:
            cf:c1:5e:77:ec:86:db:37:67:76:31:1c:48:a1:d0:
            a5:6b:d9:c8:c7:fc:7d:d3:a7:e8:5d:f8:69:af:89:
            a2:de:4f:9d:7e:0e:65:bf:fd:87:3a:78:d2:4f:4a:
            91:35:c7:30:8e:a1:fe:93:7c:9e:d3:76:e1:b8:1e:
            83:f6:91:0b:da:9a:d1:ac:3c:67:d7:7a:b8:50:6a:
            21:d1:4c:29:ff:07:31:1d:09:35:b3:2e:a8:ba:f6:
            9a:10:96:26:38:a7:a6:c5:65:59:d7:d1:0f:3a:92:
            15:c6:f8:15:4d:ad:1e:db:cc:fb:ce:50:cf:ff:df:
            b1:37:b7:c2:bb:8e:88:ae:fd:12:59:5c:5a:d8:99:
            c0:a2:03:f7:d3:ca:e4:f3:10:20:79:12:40:e2:54:
            4a:38:08:05:6f:67:49:65:23:53:eb:9c:35:f5:27:
            81:ab
        generator: 2 (0x2)

Now **Bob**, needs to do the same thing:

    openssl genpkey -paramfile dhparams.pem -out bob_private.dh

After this, **Bob** and **Alice** need to exchange their **public keys**. These need to **get extracted from their private key files** generated in the previous step. 

So, **Alice** does:

    openssl pkey -in alice_private.dh -pubout -out alice_public.dh

And **Bob**, does:

    openssl pkey -in bob_private.dh -pubout -out bob_public.dh

Now, **Alice** can send `alice_public.dh` to **Bob**, and **Bob** can send `bob_public.dh` to **Alice**.

Finally, both **Alice** and **Bob**, can compute a **common secret key** that can be used to exchange encrypted information. **Alice**, needs to derive its key:

    openssl pkeyutl -derive -inkey alice_private.dh -peerkey bob_public.dh -out alice_common_key.dh

And **Bob**, can do the same:

    openssl pkeyutl -derive -inkey bob_private.dh -peerkey alice_public.dh -out bob_common_key.dh

If we compare them both, they are equal.

    diff alice_common_key.dh bob_common_key.dh

And you can look at the content of the files... using `hexdump` utility.

    hexdump -C alice_common_key.dh

Here are the contents of the key:

    00000000  c7 5f cf 8d 1e 00 6f 54  7b cc 7d 0c 85 d0 99 15  |�_�...oT{�}..�..|
    00000010  78 47 bc 32 fa 09 67 73  15 b5 9e 42 e6 e0 13 3c  |xG�2�.gs.�.B��.<|
    00000020  d7 d3 1c 17 a1 ef 45 1f  4b 2f 65 b0 d0 87 f9 d0  |��..��E.K/e��.��|
    00000030  2e 99 fe 66 91 fe e3 c1  01 2a b0 e2 d5 e1 a7 4e  |..�f.���.*����N|
    00000040  b8 8c 97 e1 c4 ec 29 58  41 06 77 50 f4 95 23 fb  |�..���)XA.wP�.#�|
    00000050  13 29 01 43 22 05 8a db  38 1a f8 21 30 8c f8 f7  |.).C"..�8.�!0.��|
    00000060  22 25 7b 88 89 7a 05 0c  b5 fa 30 ce 61 2c c6 e4  |"%{..z..��0�a,��|
    00000070  24 ca 23 e2 78 05 13 b3  2e 13 a9 2c e4 19 d7 eb  |$�#�x..�..�,�.��|
    00000080  be 8a 61 b4 d1 20 df ed  9e a2 31 89 4b b0 d5 6b  |�.a�� ��.�1.K��k|
    00000090  ac d8 8a 01 46 1a cc b9  fc aa e0 68 6d 82 35 52  |��..F.̹��hm.5R|
    000000a0  45 c5 10 6d bd 07 ac a1  a9 4f 3f 23 12 e3 d8 4e  |E�.m�.���O?#.��N|
    000000b0  0e e9 18 04 68 da b4 93  5d e0 86 c0 ad 1f d6 fc  |.�..hڴ.]�.��.��|
    000000c0  d7 e1 70 64 77 96 6b 83  57 77 47 5a 60 1d 45 92  |��pdw.k.WwGZ`.E.|
    000000d0  70 46 67 4d 3a f8 64 d7  ad f9 db a4 0d 92 cf e6  |pFgM:�d׭�ۤ..��|
    000000e0  d0 43 86 be b6 2e 65 08  95 dc 7d d9 6d 0c 03 b9  |�C.��.e..�}�m..�|
    000000f0  cb 2a 46 5e 6a 78 36 2f  51 ff ba b5 7a 17 45 33  |�*F^jx6/Q���z.E3|
    00000100

### Encrypt and decrypt with DH

Now that we have a common key between **Alice** and **Bob**, this key can be used to do symmetric cryptography between the two parties communicating. Consider the `plain.txt` and `cipher.txt` below, two examples of files that are going to be encrypted or decrypted.

To encrypt data in **Alice**, we can use:

    openssl aes-256-ecb -base64 -kfile alice_common_key.dh -e -in plain.txt -out cipher.txt -pbkdf2

To decrypt data in the **Bob** side, we can do:

    openssl aes-256-ecb -base64 -kfile bob_common_key.dh -d -in cipher.txt -out clear.txt -pbkdf2

And this is a possible way to encrypt and decrypt data using a DH common key.