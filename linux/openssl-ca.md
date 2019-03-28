# OpenSSL - 自建CA

## 生成根证书私钥

pem - Privacy Enbanced Mail

```
maos@dev:~/ssl$ openssl genrsa -aes256 -out private/cakey.pem 1024
Generating RSA private key, 1024 bit long modulus
...............++++++
...........................++++++
e is 65537 (0x10001)
Enter pass phrase for private/cakey.pem:
Verifying - Enter pass phrase for private/cakey.pem:
```

* genrsa - 使用rsa算法产生私钥
* -aes256 - 使用256位的aes算法对私钥加密
* -out - 私钥输出路径
* 1024 - 私钥长度

## 生成根证书的申请文件

csr - certificate signing request

```
maos@dev:~/ssl$ openssl req -new -key private/cakey.pem -out private/ca.csr
Enter pass phrase for private/cakey.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Beijing
Locality Name (eg, city) []:Beijing
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Test
Organizational Unit Name (eg, section) []:Tech
Common Name (e.g. server FQDN or YOUR name) []:ca
Email Address []:ca@test.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:123456
An optional company name []:Test LT
```
* req - 执行证书签发命令
* -new - 新的签发申请
* -key - 私钥路径
* -out - csr文件输出路径

## 自签发根证书

csr文件一般生成后发给CA机构进行签发，这里进行自签发

```
maos@dev:~/ssl$ openssl x509 -req -days 365 -sha1 -extensions v3_ca -signkey private/cakey.pem -in private/ca.csr -out certs/ca.crt
Signature ok
subject=/C=CN/ST=Beijing/L=Beijing/O=Test/OU=Tech/CN=ca/emailAddress=ca@test.com
Getting Private key
Enter pass phrase for private/cakey.pem:
```

* x509 - 生成x509格式证书
* -req - 输入的是一个请求文件（csr）
* -days - 证书有效期
* -sha1 - 摘要算法
* -extensions - 添加openssl.cnf配置文件中的va_ca扩展
* -signkey - 自签发时用，签发证书的私钥
* -in - 输入的csr文件
* -out - 输出的证书文件

证书信息查看或确认

```
maos@dev:~/ssl$ openssl x509 -in certs/ca.crt -noout -text
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 13716219366033723470 (0xbe59ccd77dc6104e)
    Signature Algorithm: sha1WithRSAEncryption
        Issuer: C=CN, ST=Beijing, L=Beijing, O=Test, OU=Tech, CN=ca/emailAddress=ca@test.com
        Validity
            Not Before: Mar 28 08:00:27 2019 GMT
            Not After : Mar 27 08:00:27 2020 GMT
        Subject: C=CN, ST=Beijing, L=Beijing, O=Test, OU=Tech, CN=ca/emailAddress=ca@test.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (1024 bit)
                Modulus:
                    00:af:2b:56:1e:ee:68:1b:74:27:e8:d0:e3:b1:c2:
                    ee:0b:91:4b:7a:97:2e:cf:b4:12:15:5d:54:3d:b0:
                    e1:4a:22:f8:46:11:98:c2:74:ae:47:0d:88:7a:82:
                    16:77:3e:0a:af:72:be:e0:46:58:a2:4f:a8:24:41:
                    12:18:79:45:b4:33:2c:6b:9a:46:6f:0b:15:a7:67:
                    04:cd:42:5c:43:7f:bc:e3:ae:ac:fc:af:35:e4:0a:
                    c7:9f:89:50:0a:1e:23:63:c7:c5:66:c3:26:7f:0f:
                    52:b0:e2:28:a2:b2:6e:ca:80:e2:42:05:c6:63:d5:
                    4b:41:64:70:0f:bb:34:bc:83
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha1WithRSAEncryption
         80:9e:80:68:e4:84:a7:af:60:fc:95:38:b2:67:fb:90:74:d6:
         b4:6f:9a:b5:a2:71:4e:15:8f:c4:0a:e0:51:5a:1c:df:f9:37:
         07:73:73:b3:93:05:de:03:6d:64:11:a7:0e:20:3f:ee:09:bb:
         e9:dd:40:6c:9f:39:fa:7c:4b:83:7b:2b:7a:c3:b3:d8:c6:c9:
         ed:02:ae:36:2c:8d:9f:79:61:fd:6e:fd:dc:d4:a2:db:a3:53:
         9b:e0:51:e0:29:2a:2a:c9:38:f4:2d:b9:54:4a:bc:3d:c8:e7:
         7b:d6:7d:48:d0:00:67:34:da:14:c1:3d:6d:27:21:11:db:05:
         fe:00
```

## 用根证书签发Server证书

生成服务端私钥

```
maos@dev:~/ssl$ openssl genrsa -aes256 -out private/server-key.pem 1024
Generating RSA private key, 1024 bit long modulus
.............++++++
..............................++++++
e is 65537 (0x10001)
Enter pass phrase for private/server-key.pem:
Verifying - Enter pass phrase for private/server-key.pem:
```

生成证书请求文件（csr）

```
maos@dev:~/ssl$ openssl req -new -key private/server-key.pem -out private/server.csr
Enter pass phrase for private/server-key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Beijing
Locality Name (eg, city) []:Beijing
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Test
Organizational Unit Name (eg, section) []:Tech
Common Name (e.g. server FQDN or YOUR name) []:server.test.com
Email Address []:server@test.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:123456
An optional company name []:Test LD
```

使用根证书签发服务端证书

```
maos@dev:~/ssl$ openssl x509 -req -days 365 -sha1 -extensions v3_req -CA certs/ca.crt -CAkey private/cakey.pem -CAserial ca.srl -CAcreateserial -in private/server.csr -out certs/server.crt
Signature ok
subject=/C=CN/ST=Beijing/L=Beijing/O=Test/OU=Tech/CN=server.test.com/emailAddress=server@test.com
Getting CA Private Key
Enter pass phrase for private/cakey.pem:
```

* -CA - CA证书路径
* -CAkey - CA证书私钥路径
* -CAserial - 证书序列号路径
* -CAcreateserial - 证书序列号文件不存在的话则创建

需注意-extensions v3_req v3_ca参数的不同

v3_ca

```
basicConstraints = CA:true
```

v3_req

```
basicConstraints = CA:FALSE
```

## 用根证书签发Client证书

生成客户端私钥

```
maos@dev:~/ssl$ openssl genrsa -aes256 -out private/client-key.pem
Generating RSA private key, 2048 bit long modulus
...............................................+++
...........................................................................+++
e is 65537 (0x10001)
Enter pass phrase for private/client-key.pem:
Verifying - Enter pass phrase for private/client-key.pem:
```

生成证书请求文件（csr）

```
maos@dev:~/ssl$ openssl req -new -key private/client-key.pem -out private/client.csr
Enter pass phrase for private/client-key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Beijing
Locality Name (eg, city) []:Beijing
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Test
Organizational Unit Name (eg, section) []:Tech
Common Name (e.g. server FQDN or YOUR name) []:client.test.com
Email Address []:client@test.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:123456
An optional company name []:Test LD
```

使用根证书签发客户端证书

```
maos@dev:~/ssl$ openssl x509 -req -days 365 -sha1 -extensions v3_req -CA certs/ca.crt -CAkey private/cakey.pem -CAserial ca.srl -in private/client.csr -out certs/client.crt
Signature ok
subject=/C=CN/ST=Beijing/L=Beijing/O=Test/OU=Tech/CN=client.test.com/emailAddress=client@test.com
Getting CA Private Key
Enter pass phrase for private/cakey.pem:
```

参考：[OpenSSL生成根证书CA及签发子证书](https://my.oschina.net/itblog/blog/651434)
