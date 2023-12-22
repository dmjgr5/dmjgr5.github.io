---
layout: default
title: Ubuntu apache2 에서의 SSL 적용하기
parent: Network & Security
nav_order: 15
---


# Ubuntu 에서 apache2 설치하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .note}
Ubuntu 에서는 apache2 에서의 SSL 적용 방법에 대해 정리해 본다.

## OpenSSL 설치하기

apt install 을 이용하여 openssl 을 설치한다.

```sh
root@user1-500R5K-501R5K-500R5Q:~# sudo apt install openssl
Reading package lists... Done
Building dependency tree       
Reading state information... Done
openssl is already the newest version (1.1.1f-1ubuntu2.20).
openssl set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 20 not upgraded.
root@user1-500R5K-501R5K-500R5Q:~# 
```

## Private Key(*.key) 생성하기 (openssl genrsa)

`openssl` 커맨드로 아래와 같이 개인키를 생성한다. `Enter pass phrase for server.key:` 부분에 개인키 암호를 입력하면 `-out` 에 입력한 key 파일이 생성됨을 확인할 수 있다. 


```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# openssl genrsa -des3 -out server.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
.......................................................................................+++++
..................................+++++
e is 65537 (0x010001)
Enter pass phrase for server.key:
Verifying - Enter pass phrase for server.key:
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls
server.key
root@user1-500R5K-501R5K-500R5Q:~/ssltest# cat server.key 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,FCD3A5456ED72869

PN0jOfBzKKzBwRkltcVfVGLSkoMYQgCMGqqWo+3RcUjvWD1EA50eugB3G53hxQDl
xgtYdJ8dzQ+QznQF/08rZk5e+hlixqaC/IFu955tINbSqriMUW4+6qKnzB6pviJz
xK7LUVyLpbbD6kE75dCt9f0GtVt0byR9CA1IfUD8K2vmDlv8j4aIsb2hKAG74RR3
MROmleiRPIP89Rbc2qTI9sM6Rmhs3DEnXeA/EOunTyEWPi4ectr+gadyxM0D2yMN
CsipfwCLNVIVWcYmHCMZwXM5Z0MmmDduHpvBuMYHkDyJK6F6Vu8WuUiIvT+01GT6
mq9BGfCvUoTCuql7ZuaJui6rtOiUAE/Vo9ouzLsKYSL++zLxYiv1b7iLyuxm2HMf
0z+im5WP60WMDyzxNEB4UMoiW+V+iPLA4jyGB/uH9bxUZmh7ndMra2at3RopU0zk
mVNlmCFDRDKeDwql200ahxWI/ioqTiA0Go08SeivwTSZFNUtUpSdy9wyDha+VrZI
qx8QZBD+K+d/62LLr7Uqgh6T0mlchTIX8W/TpCdDNPDN9u6VRd+3FUGE5FHp9sfD
053oTVsp6lQwOHm447I+d02g895U4sIHG4DGxC7oPmYy/QdQDLw8RzO93BY51pzQ
DpnlMjsSvf6FPrcb3UlE4xYFBa6xRHnCNpa1rW/2ZlT6sgNzuwzCMBN7I6bZfJ/2
HdPOSITuhv25oYgUvcEIJ7p1q6N1ZPNdv8lO56r4rBBYxiYDSt22U9FdT3BpzsPA
uUPWTgoW5pFAsIZIuNAHZ28391AHTpr4L1IrEQUOo9CSAan4TMfZzHeq5kcYh3LG
MTv1VyV10MAQAgBJnyEQaMvLXy+djjN6RpXCmdi1xj/guSnsmVhU/yavG8X/ubt7
WLyUEGgfpcnyzJdS38/rldu472A3iMbOzGVpnmgadB7bL6hBmcce9cI1JrpQ4LCh
KqVz2FlV3QWFx7Rd6TT9XrvXm4iSOMXy+njid1I4N6LYOprB6VXP0Y/XEt4sZWg9
VVYdYpZvFSBOsQjwYugP+mT8gs2R1InrAUhR3RpoI5FtM/RI7JVBodubQRjs0rUA
Al8R/z/sGuKMa/5p2KarASgSmwZ9Iiq6uldQrzwn35lFOCaEJIQmkyme9DFAxiFP
aq5KBdO76p+7fUC4s566utE4g7Oz3+mQ+kvzx7eI1lQKnCj0m+1cHW/BQmj8AdCm
B8V5xL+1+pLzyF/T6UFqLH1FU53OiFDGDaHTLJ6v9rWkY6OvIbnZNChD7mu4Ow1d
ng98Q4RxL+23vKYxVhDsOiIu+6it3Pol/8imGQO8Qc8f64MD2QSC9HYQuJJ/BTkg
4Mqa1g7NeW1V8acfYiBpadSQxhAmh65bjQuJyX7vivMs9f928wEl8D8qdNsASEKI
2EKL5a2/lNlv0zkcfNZOF3RUsNrPjEx4kDi1WWnIjFUtwI95ZYFK3uaqgCs6IlGp
zzJEzUX8sjBCHZvFpdsuP4nfxFBQxwyiJ1mgbWXaJq73jUqwVefP1vndzzM9dFRY
6aYh+cEzcYD3RYaPFQnkJ0UQAl3fWzmOt1B4JA2wqpHDTL0l3oi8TA==
-----END RSA PRIVATE KEY-----
```

## CSR(Certificate Sinning Request) 인증요청서(*.csr) 생성하기 (openssl req)

생성했던 개인키를 이용하여 CSR 인증서를 생성한다. `Enter pass phrase for server.key:` 에는 개인키 암호를 입력하고 그 외에는 적절한 값을 입력한다. `-out` 에 설정한 파일이 생성됨을 확인할 수 있다.


```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# openssl req -new  -key server.key -out server.csr
Enter pass phrase for server.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:kr
State or Province Name (full name) [Some-State]:seoul
Locality Name (eg, city) []:seoul
Organization Name (eg, company) [Internet Widgits Pty Ltd]:test
Organizational Unit Name (eg, section) []:test
Common Name (e.g. server FQDN or YOUR name) []:test
Email Address []:test@test.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls
server.csr  server.key
root@user1-500R5K-501R5K-500R5Q:~/ssltest# cat server.csr 
-----BEGIN CERTIFICATE REQUEST-----
MIICvTCCAaUCAQAweDELMAkGA1UEBhMCa3IxDjAMBgNVBAgMBXNlb3VsMQ4wDAYD
VQQHDAVzZW91bDENMAsGA1UECgwEdGVzdDENMAsGA1UECwwEdGVzdDENMAsGA1UE
AwwEdGVzdDEcMBoGCSqGSIb3DQEJARYNdGVzdEB0ZXN0LmNvbTCCASIwDQYJKoZI
hvcNAQEBBQADggEPADCCAQoCggEBALcKV1HVs6/7PBkaw+tEN2H6kMRLbqnleQo8
YQunSBeH2K50AGQRLDkQ8TFig4YmHjxwK0/o1OLD1nkkSKI4fGV4guU4mbpR9951
TJmAEmBLu7JCtOgUFla0pRO9fppJaLEs/n8PhqSjaOz1vrgFQEpsjVMe9uKxsKPA
Oh5dFVjwM06AmuRbqLn2k8udcgNOikZC+KfSpw7alzUWIgL2tFloK2O2crQ+rXP2
oWug71J38twdyJX2ifg8eTj96pvVzkGhLSjLqfD4ImJYsn3ySeIjUm4tKCXHYUZT
/iloGup7HbhlW4NR04goX/c3nchfTMX8T5Zgf1gmDPzeAidP27MCAwEAAaAAMA0G
CSqGSIb3DQEBCwUAA4IBAQB6IKef/29ePeKWjRypbF8QrrT7xtaUHhF+fkTCCtg1
jXqikT8GMqMK8+ZT7pqaYyl5UHfccQqJ/aY5jG/jkRq3kczl+LS2N/LfUZdrjGvL
6LQwnyIJ091EbG6oQ1Dhg58mBym9AEHMLQRrzKZpIEh8tfuWBpXXLMQ1G3t+Pdz2
LaHDtGlmIOCmrvC7u2xKvRj0CgA4ppaRIE9his4iLktyL9j3RT8k94dmg1ZZCf11
jJnG4BkZoavb3c5qRM8j0bfvLdt17ESCAS7/DiUvTUdgytSiJWOVyoGXMHWkyPxe
phdtUhVG6QnUCGcGsf3v+yOZXl0MIiQr4V9g86p4cGbA
-----END CERTIFICATE REQUEST-----
```

## 자체서명 인증서(*.crt) 생성 (openssl x509)

이전에 생성했던 개인키와 인증요청서를 이용하여 자체서명인증서를 생성한다.

```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
Signature ok
subject=C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
Getting Private key
Enter pass phrase for server.key:
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls
server.crt  server.csr  server.key
root@user1-500R5K-501R5K-500R5Q:~/ssltest# cat server.crt
-----BEGIN CERTIFICATE-----
MIIDdzCCAl8CFGm6Z/Eb6y1CHy7i/cqneSE/6iBnMA0GCSqGSIb3DQEBCwUAMHgx
CzAJBgNVBAYTAmtyMQ4wDAYDVQQIDAVzZW91bDEOMAwGA1UEBwwFc2VvdWwxDTAL
BgNVBAoMBHRlc3QxDTALBgNVBAsMBHRlc3QxDTALBgNVBAMMBHRlc3QxHDAaBgkq
hkiG9w0BCQEWDXRlc3RAdGVzdC5jb20wHhcNMjMxMjIyMTI1NjEzWhcNMjQxMjIx
MTI1NjEzWjB4MQswCQYDVQQGEwJrcjEOMAwGA1UECAwFc2VvdWwxDjAMBgNVBAcM
BXNlb3VsMQ0wCwYDVQQKDAR0ZXN0MQ0wCwYDVQQLDAR0ZXN0MQ0wCwYDVQQDDAR0
ZXN0MRwwGgYJKoZIhvcNAQkBFg10ZXN0QHRlc3QuY29tMIIBIjANBgkqhkiG9w0B
AQEFAAOCAQ8AMIIBCgKCAQEAtwpXUdWzr/s8GRrD60Q3YfqQxEtuqeV5CjxhC6dI
F4fYrnQAZBEsORDxMWKDhiYePHArT+jU4sPWeSRIojh8ZXiC5TiZulH33nVMmYAS
YEu7skK06BQWVrSlE71+mklosSz+fw+GpKNo7PW+uAVASmyNUx724rGwo8A6Hl0V
WPAzToCa5FuoufaTy51yA06KRkL4p9KnDtqXNRYiAva0WWgrY7ZytD6tc/aha6Dv
Unfy3B3IlfaJ+Dx5OP3qm9XOQaEtKMup8PgiYliyffJJ4iNSbi0oJcdhRlP+KWga
6nsduGVbg1HTiChf9zedyF9MxfxPlmB/WCYM/N4CJ0/bswIDAQABMA0GCSqGSIb3
DQEBCwUAA4IBAQCZnu4LE5cYyWTOCgQo3mShYyQIqoCK7O0kiZgQovgzF2F8cd/c
zHwJYxjiTkey84Mz6DXcZFCeyIs5XOJlYASAENmFB/e3LDRD1uKGFZofbmJPf5eN
1H4c/NrJaWV0RKtV7FrFp1Q1FxSdKgiS4ELWTNlLJrWOPfyh+8HVxChF8J7IgDS5
aIwJv8r4iRaSzCzcmqMfgnKcg2OWw46UwPDq5tYYJlAi1lNFQWFJRUsRoEHzMKwE
8Aek5ZH8/xGfpkN697u80C4LHmxMhdPhCzB8r/CJanPgsTUJ2GqIi0B1nPy6u+91
1zvLH//uOKLvG9msF9LXKKbrS14fswFC4wR9
-----END CERTIFICATE-----
```

## 인증서 확인하기 (openssl x509)

`-text` 옵션을 사용하며 인증서 상세 정보를 확인할 수 있다.

```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# openssl x509 -in server.crt -text
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number:
            69:ba:67:f1:1b:eb:2d:42:1f:2e:e2:fd:ca:a7:79:21:3f:ea:20:67
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
        Validity
            Not Before: Dec 22 12:56:13 2023 GMT
            Not After : Dec 21 12:56:13 2024 GMT
        Subject: C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:b7:0a:57:51:d5:b3:af:fb:3c:19:1a:c3:eb:44:
                    37:61:fa:90:c4:4b:6e:a9:e5:79:0a:3c:61:0b:a7:
                    48:17:87:d8:ae:74:00:64:11:2c:39:10:f1:31:62:
                    83:86:26:1e:3c:70:2b:4f:e8:d4:e2:c3:d6:79:24:
                    48:a2:38:7c:65:78:82:e5:38:99:ba:51:f7:de:75:
                    4c:99:80:12:60:4b:bb:b2:42:b4:e8:14:16:56:b4:
                    a5:13:bd:7e:9a:49:68:b1:2c:fe:7f:0f:86:a4:a3:
                    68:ec:f5:be:b8:05:40:4a:6c:8d:53:1e:f6:e2:b1:
                    b0:a3:c0:3a:1e:5d:15:58:f0:33:4e:80:9a:e4:5b:
                    a8:b9:f6:93:cb:9d:72:03:4e:8a:46:42:f8:a7:d2:
                    a7:0e:da:97:35:16:22:02:f6:b4:59:68:2b:63:b6:
                    72:b4:3e:ad:73:f6:a1:6b:a0:ef:52:77:f2:dc:1d:
                    c8:95:f6:89:f8:3c:79:38:fd:ea:9b:d5:ce:41:a1:
                    2d:28:cb:a9:f0:f8:22:62:58:b2:7d:f2:49:e2:23:
                    52:6e:2d:28:25:c7:61:46:53:fe:29:68:1a:ea:7b:
                    1d:b8:65:5b:83:51:d3:88:28:5f:f7:37:9d:c8:5f:
                    4c:c5:fc:4f:96:60:7f:58:26:0c:fc:de:02:27:4f:
                    db:b3
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha256WithRSAEncryption
         99:9e:ee:0b:13:97:18:c9:64:ce:0a:04:28:de:64:a1:63:24:
         08:aa:80:8a:ec:ed:24:89:98:10:a2:f8:33:17:61:7c:71:df:
         dc:cc:7c:09:63:18:e2:4e:47:b2:f3:83:33:e8:35:dc:64:50:
         9e:c8:8b:39:5c:e2:65:60:04:80:10:d9:85:07:f7:b7:2c:34:
         43:d6:e2:86:15:9a:1f:6e:62:4f:7f:97:8d:d4:7e:1c:fc:da:
         c9:69:65:74:44:ab:55:ec:5a:c5:a7:54:35:17:14:9d:2a:08:
         92:e0:42:d6:4c:d9:4b:26:b5:8e:3d:fc:a1:fb:c1:d5:c4:28:
         45:f0:9e:c8:80:34:b9:68:8c:09:bf:ca:f8:89:16:92:cc:2c:
         dc:9a:a3:1f:82:72:9c:83:63:96:c3:8e:94:c0:f0:ea:e6:d6:
         18:26:50:22:d6:53:45:41:61:49:45:4b:11:a0:41:f3:30:ac:
         04:f0:07:a4:e5:91:fc:ff:11:9f:a6:43:7a:f7:bb:bc:d0:2e:
         0b:1e:6c:4c:85:d3:e1:0b:30:7c:af:f0:89:6a:73:e0:b1:35:
         09:d8:6a:88:8b:40:75:9c:fc:ba:bb:ef:75:d7:3b:cb:1f:ff:
         ee:38:a2:ef:1b:d9:ac:17:d2:d7:28:a6:eb:4b:5e:1f:b3:01:
         42:e3:04:7d
-----BEGIN CERTIFICATE-----
MIIDdzCCAl8CFGm6Z/Eb6y1CHy7i/cqneSE/6iBnMA0GCSqGSIb3DQEBCwUAMHgx
CzAJBgNVBAYTAmtyMQ4wDAYDVQQIDAVzZW91bDEOMAwGA1UEBwwFc2VvdWwxDTAL
BgNVBAoMBHRlc3QxDTALBgNVBAsMBHRlc3QxDTALBgNVBAMMBHRlc3QxHDAaBgkq
hkiG9w0BCQEWDXRlc3RAdGVzdC5jb20wHhcNMjMxMjIyMTI1NjEzWhcNMjQxMjIx
MTI1NjEzWjB4MQswCQYDVQQGEwJrcjEOMAwGA1UECAwFc2VvdWwxDjAMBgNVBAcM
BXNlb3VsMQ0wCwYDVQQKDAR0ZXN0MQ0wCwYDVQQLDAR0ZXN0MQ0wCwYDVQQDDAR0
ZXN0MRwwGgYJKoZIhvcNAQkBFg10ZXN0QHRlc3QuY29tMIIBIjANBgkqhkiG9w0B
AQEFAAOCAQ8AMIIBCgKCAQEAtwpXUdWzr/s8GRrD60Q3YfqQxEtuqeV5CjxhC6dI
F4fYrnQAZBEsORDxMWKDhiYePHArT+jU4sPWeSRIojh8ZXiC5TiZulH33nVMmYAS
YEu7skK06BQWVrSlE71+mklosSz+fw+GpKNo7PW+uAVASmyNUx724rGwo8A6Hl0V
WPAzToCa5FuoufaTy51yA06KRkL4p9KnDtqXNRYiAva0WWgrY7ZytD6tc/aha6Dv
Unfy3B3IlfaJ+Dx5OP3qm9XOQaEtKMup8PgiYliyffJJ4iNSbi0oJcdhRlP+KWga
6nsduGVbg1HTiChf9zedyF9MxfxPlmB/WCYM/N4CJ0/bswIDAQABMA0GCSqGSIb3
DQEBCwUAA4IBAQCZnu4LE5cYyWTOCgQo3mShYyQIqoCK7O0kiZgQovgzF2F8cd/c
zHwJYxjiTkey84Mz6DXcZFCeyIs5XOJlYASAENmFB/e3LDRD1uKGFZofbmJPf5eN
1H4c/NrJaWV0RKtV7FrFp1Q1FxSdKgiS4ELWTNlLJrWOPfyh+8HVxChF8J7IgDS5
aIwJv8r4iRaSzCzcmqMfgnKcg2OWw46UwPDq5tYYJlAi1lNFQWFJRUsRoEHzMKwE
8Aek5ZH8/xGfpkN697u80C4LHmxMhdPhCzB8r/CJanPgsTUJ2GqIi0B1nPy6u+91
1zvLH//uOKLvG9msF9LXKKbrS14fswFC4wR9
-----END CERTIFICATE-----
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 

```


## Apache2 SSL 적용하기

`a2enmod ssl` 명령어로 SSL 모듈을 enable 한다.

```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# a2enmod ssl
Considering dependency setenvif for ssl:
Module setenvif already enabled
Considering dependency mime for ssl:
Module mime already enabled
Considering dependency socache_shmcb for ssl:
Enabling module socache_shmcb.
Enabling module ssl.
See /usr/share/doc/apache2/README.Debian.gz on how to configure SSL and create self-signed certificates.
To activate the new configuration, you need to run:
  systemctl restart apache2
```

SSL 을 위한 임의의 디렉토리를 생성하고 생성한 CA 인증서와 자체 서명인증서를 저장한다.

```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# mkdir /etc/apache2/ssl
root@user1-500R5K-501R5K-500R5Q:~/ssltest# cp server* /etc/apache2/ssl/
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls -al /etc/apache2/ssl/
total 20
drwxr-xr-x 2 root root 4096 12월 22 22:09 .
drwxr-xr-x 9 root root 4096 12월 22 22:09 ..
-rw-r--r-- 1 root root 1261 12월 22 22:09 server.crt
-rw-r--r-- 1 root root 1025 12월 22 22:09 server.csr
-rw------- 1 root root 1743 12월 22 22:09 server.key
```

또한 소유권 및 권한 변경을 진행한다. 여기서는 root 를 기준으로 설정한다.

```sh
$ sudo chown -R root:root /etc/apache2/ssl
$ sudo chmod 700 /etc/apahce2/ssl
$ sudo chmod 600 /etc/apache2/ssl/*.*
```

```sh
root@user1-500R5K-501R5K-500R5Q:~/ssltest# chown -R root:root /etc/apache2/ssl/
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls -al /etc/apache2/ssl/
total 20
drwxr-xr-x 2 root root 4096 12월 22 22:09 .
drwxr-xr-x 9 root root 4096 12월 22 22:09 ..
-rw-r--r-- 1 root root 1261 12월 22 22:09 server.crt
-rw-r--r-- 1 root root 1025 12월 22 22:09 server.csr
-rw------- 1 root root 1743 12월 22 22:09 server.key
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 
root@user1-500R5K-501R5K-500R5Q:~/ssltest# chmod 700 /etc/apache2/ssl
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls -al /etc/apache2/ssl/
total 20
drwx------ 2 root root 4096 12월 22 22:09 .
drwxr-xr-x 9 root root 4096 12월 22 22:09 ..
-rw-r--r-- 1 root root 1261 12월 22 22:09 server.crt
-rw-r--r-- 1 root root 1025 12월 22 22:09 server.csr
-rw------- 1 root root 1743 12월 22 22:09 server.key
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls -al /etc/apache2/
total 100
drwxr-xr-x   9 root root  4096 12월 22 22:09 .
drwxr-xr-x 150 root root 12288 12월 22 21:20 ..
-rw-r--r--   1 root root  7224 10월 26 22:54 apache2.conf
drwxr-xr-x   2 root root  4096 12월 22 21:20 conf-available
drwxr-xr-x   2 root root  4096 12월 22 21:20 conf-enabled
-rw-r--r--   1 root root  1782  2월 24  2021 envvars
-rw-r--r--   1 root root 31063  2월 24  2021 magic
drwxr-xr-x   2 root root 12288 12월 22 21:20 mods-available
drwxr-xr-x   2 root root  4096 12월 22 22:06 mods-enabled
-rw-r--r--   1 root root   320  2월 24  2021 ports.conf
drwxr-xr-x   2 root root  4096 12월 22 21:20 sites-available
drwxr-xr-x   2 root root  4096 12월 22 21:20 sites-enabled
drwx------   2 root root  4096 12월 22 22:09 ssl
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 
root@user1-500R5K-501R5K-500R5Q:~/ssltest# chmod 600 /etc/apache2/ssl/*.*
root@user1-500R5K-501R5K-500R5Q:~/ssltest# ls -al /etc/apache2/ssl/
total 20
drwx------ 2 root root 4096 12월 22 22:09 .
drwxr-xr-x 9 root root 4096 12월 22 22:09 ..
-rw------- 1 root root 1261 12월 22 22:09 server.crt
-rw------- 1 root root 1025 12월 22 22:09 server.csr
-rw------- 1 root root 1743 12월 22 22:09 server.key
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 
root@user1-500R5K-501R5K-500R5Q:~/ssltest# 
```

## VirtualHost 설정하기

`/etc/apache2/site-avaiale` 디렉토리로 이동하여 default conf 파일을 복사하여 수정한다.

```sh
root@user1-500R5K-501R5K-500R5Q:~# cd /etc/apache2/sites-available/
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# ls
000-default.conf  default-ssl.conf
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# cp default-ssl.conf example-ssl.conf
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# ls
000-default.conf  default-ssl.conf  example-ssl.conf
```


아래 3개 항목을 인증서 위치를 기준으로 수정한다.

```sh
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# vim example-ssl.conf 


    SSLEngine on
    SSLCertificateFile      /etc/apache2/ssl/server.crt
    SSLCertificateKeyFile /etc/apache2/ssl/server.key
```

설정 변경한 conf 파일을 enable 한다.

```sh
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# ls
000-default.conf  default-ssl.conf  example-ssl.conf
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# a2ensite example-ssl.conf 
Enabling site example-ssl.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# 
```

이후 apache 서비스를 restart 한다. 이 때 인증서 생성 시 설정했던 암호를 입력한다.


```sh
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# service apache2 restart
Enter passphrase for SSL/TLS keys for 127.0.0.1:443 (RSA): ****                    
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# service apache2 status
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabl>
     Active: active (running) since Fri 2023-12-22 22:21:03 KST; 8s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 5762 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
   Main PID: 5773 (apache2)
      Tasks: 55 (limit: 9323)
     Memory: 5.8M
     CGroup: /system.slice/apache2.service
             ├─5773 /usr/sbin/apache2 -k start
             ├─5774 /usr/sbin/apache2 -k start
             └─5775 /usr/sbin/apache2 -k start

12월 22 22:20:55 user1-500R5K-501R5K-500R5Q systemd[1]: Starting The Apache HTTP Serve>
12월 22 22:20:55 user1-500R5K-501R5K-500R5Q apachectl[5765]: AH00558: apache2: Could n>
12월 22 22:21:03 user1-500R5K-501R5K-500R5Q systemd[1]: Started The Apache HTTP Server.
lines 1-16/16 (END)

```

## SSL 적용 확인하기 (openssl s_client)

아래와 같이 명령어를 이용하여 생성했던 인증서 정보와 동일함을 확인할 수 있다. 확인 시 중요한 부분은 하단 부분의 `Verify return code: 18 (self signed certificate)` 임을 알 수 있다.


```sh
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# openssl s_client -connect localhost:443
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
verify error:num=18:self signed certificate
verify return:1
depth=0 C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
verify return:1
---
Certificate chain
 0 s:C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
   i:C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIDdzCCAl8CFGm6Z/Eb6y1CHy7i/cqneSE/6iBnMA0GCSqGSIb3DQEBCwUAMHgx
CzAJBgNVBAYTAmtyMQ4wDAYDVQQIDAVzZW91bDEOMAwGA1UEBwwFc2VvdWwxDTAL
BgNVBAoMBHRlc3QxDTALBgNVBAsMBHRlc3QxDTALBgNVBAMMBHRlc3QxHDAaBgkq
hkiG9w0BCQEWDXRlc3RAdGVzdC5jb20wHhcNMjMxMjIyMTI1NjEzWhcNMjQxMjIx
MTI1NjEzWjB4MQswCQYDVQQGEwJrcjEOMAwGA1UECAwFc2VvdWwxDjAMBgNVBAcM
BXNlb3VsMQ0wCwYDVQQKDAR0ZXN0MQ0wCwYDVQQLDAR0ZXN0MQ0wCwYDVQQDDAR0
ZXN0MRwwGgYJKoZIhvcNAQkBFg10ZXN0QHRlc3QuY29tMIIBIjANBgkqhkiG9w0B
AQEFAAOCAQ8AMIIBCgKCAQEAtwpXUdWzr/s8GRrD60Q3YfqQxEtuqeV5CjxhC6dI
F4fYrnQAZBEsORDxMWKDhiYePHArT+jU4sPWeSRIojh8ZXiC5TiZulH33nVMmYAS
YEu7skK06BQWVrSlE71+mklosSz+fw+GpKNo7PW+uAVASmyNUx724rGwo8A6Hl0V
WPAzToCa5FuoufaTy51yA06KRkL4p9KnDtqXNRYiAva0WWgrY7ZytD6tc/aha6Dv
Unfy3B3IlfaJ+Dx5OP3qm9XOQaEtKMup8PgiYliyffJJ4iNSbi0oJcdhRlP+KWga
6nsduGVbg1HTiChf9zedyF9MxfxPlmB/WCYM/N4CJ0/bswIDAQABMA0GCSqGSIb3
DQEBCwUAA4IBAQCZnu4LE5cYyWTOCgQo3mShYyQIqoCK7O0kiZgQovgzF2F8cd/c
zHwJYxjiTkey84Mz6DXcZFCeyIs5XOJlYASAENmFB/e3LDRD1uKGFZofbmJPf5eN
1H4c/NrJaWV0RKtV7FrFp1Q1FxSdKgiS4ELWTNlLJrWOPfyh+8HVxChF8J7IgDS5
aIwJv8r4iRaSzCzcmqMfgnKcg2OWw46UwPDq5tYYJlAi1lNFQWFJRUsRoEHzMKwE
8Aek5ZH8/xGfpkN697u80C4LHmxMhdPhCzB8r/CJanPgsTUJ2GqIi0B1nPy6u+91
1zvLH//uOKLvG9msF9LXKKbrS14fswFC4wR9
-----END CERTIFICATE-----
subject=C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com

issuer=C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com

---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 1447 bytes and written 363 bytes
Verification error: self signed certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 18 (self signed certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 006CB978115911FC20746754CF1834E945E8CC7188970B5B7F16684A6226BAFC
    Session-ID-ctx: 
    Resumption PSK: 023BA699D9420D33CF65745A204A8A11036D73E49551F4A97A0E758D634E5E1F338EDF6FEF8DC506C62F51F2B2B8BDD6
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - a0 78 0e 9f a2 d0 01 71-65 3b 08 89 ef 0b bf c6   .x.....qe;......
    0010 - 38 7a a8 6a cd 26 24 20-57 0d d6 97 b8 fc 56 a0   8z.j.&$ W.....V.
    0020 - 01 a7 6a c3 4f e8 be d4-ee 0c c5 75 68 af 41 9d   ..j.O......uh.A.
    0030 - 9b 5d 9c f3 76 85 3b 3d-36 33 7e 1f af cc ff 0e   .]..v.;=63~.....
    0040 - 20 32 46 29 ea c8 57 19-cf 87 1a 26 7d cc 3c e6    2F)..W....&}.<.
    0050 - 45 8e 91 1a 06 f2 66 0e-65 31 ee 0d 1e b9 f7 11   E.....f.e1......
    0060 - 58 dc c8 0e a1 02 7c ec-5b 0d 00 08 6e 90 7d 28   X.....|.[...n.}(
    0070 - 9a 10 31 ae 11 17 20 02-b6 1d f8 2f f0 ec 72 00   ..1... ..../..r.
    0080 - 6f 91 cf 94 a4 09 bc 8e-18 7b 77 cf b1 33 97 54   o........{w..3.T
    0090 - b1 4f b7 b2 df 91 4c 61-62 95 07 3e 80 8b 0d 8e   .O....Lab..>....
    00a0 - 89 13 2a 6a e6 de 9e 99-87 29 c7 0f ea 41 2f ae   ..*j.....)...A/.
    00b0 - dc 51 85 82 68 eb 48 82-c2 29 a7 89 de 6f e1 95   .Q..h.H..)...o..
    00c0 - 44 e5 f4 5e 4b a5 c2 d7-5d d1 fe 94 3e ff d0 ed   D..^K...]...>...
    00d0 - c9 c3 f0 b3 24 93 db 41-78 af d8 e9 a5 ab df 2b   ....$..Ax......+

    Start Time: 1703251583
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    *Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 3662E9A94DD269C167C1BCCEDD33DF8F56ACB5A40F2E885220F9F83641D36C24
    Session-ID-ctx: 
    Resumption PSK: C2AC5E0067CF6E11DE2D45C40C7851D270F67AFAC82927603A801C4C746F2EEB471BB6F221CDAEBF260A57B7BCBFA816
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - a0 78 0e 9f a2 d0 01 71-65 3b 08 89 ef 0b bf c6   .x.....qe;......
    0010 - 45 23 2f e6 e9 d2 6a 81-e2 e7 a1 ce c9 00 29 0e   E#/...j.......).
    0020 - 71 a3 b5 e1 cb 32 d1 e6-69 46 54 74 64 21 e1 64   q....2..iFTtd!.d
    0030 - 14 a9 5a 62 0d 73 ca ac-ad 2a 29 7a 46 75 01 eb   ..Zb.s...*)zFu..
    0040 - 54 58 f4 51 79 a2 45 9c-db 4c de 4d 92 67 27 d7   TX.Qy.E..L.M.g'.
    0050 - ea 51 23 66 fd da ab d9-72 7c ed 84 f7 17 36 14   .Q#f....r|....6.
    0060 - 7e 9a 0c df ed a5 41 e8-5f df 28 63 7c 31 4a e0   ~.....A._.(c|1J.
    0070 - 20 a4 a7 b5 95 31 b7 14-7d ca e1 34 05 eb f4 97    ....1..}..4....
    0080 - ad b4 b5 63 7f 72 3a 96-b3 de a0 62 72 d5 3b a8   ...c.r:....br.;.
    0090 - 40 33 f2 a7 7e dd a1 94-30 db 6e e0 4d 98 67 5d   @3..~...0.n.M.g]
    00a0 - e6 23 3f 1a b6 67 b9 cc-d7 16 a2 1e 77 e0 fb b4   .#?..g......w...
    00b0 - 96 2a 00 df be 3f 56 9d-ff 24 57 b5 89 9e 43 76   .*...?V..$W...Cv
    00c0 - 06 e1 a7 00 2f 37 57 7d-c7 be dd 20 31 61 e6 4e   ..../7W}... 1a.N
    00d0 - 5c 22 7f d9 90 af f3 41-51 7d fc 52 53 41 19 2b   \".....AQ}.RSA.+

    Start Time: 1703251583
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK

```

또한 `-dates` 옵션으로 시작일, 만료일을 확인할 수 있다.


```sh
root@user1-500R5K-501R5K-500R5Q:/etc/apache2/sites-available# openssl s_client -connect localhost:443 | openssl x509 -noout -dates
Can't use SSL_get_servername
depth=0 C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
verify error:num=18:self signed certificate
verify return:1
depth=0 C = kr, ST = seoul, L = seoul, O = test, OU = test, CN = test, emailAddress = test@test.com
verify return:1
notBefore=Dec 22 12:56:13 2023 GMT
notAfter=Dec 21 12:56:13 2024 GMT
```