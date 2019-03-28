# 证书相关信息速查

## 链接

[OpenSSL PKI Tutorial](https://pki-tutorial.readthedocs.io/en/latest/)

## 常见简写

* PKCS - Public Key Cryptography Standards
* PKI - Public Key Infrastructure
* CA - Certification Authority
* PEM - Privacy-enhanced Electronic Mail, Base64编码, "-----BEGIN CERTIFICATE-----" and "-----END CERTIFICATE-----"
* CSR - Certificate Signing Request
* CRL - Certificate Revocation Lists

## 证书验证

```
maos@dev:~/ssl$ openssl verify -CAfile certs/ca.crt certs/server1.crt
certs/server1.crt: OK
```

## 私钥去掉密码

```
maos@dev:~/ssl$ openssl rsa -in private/server-key.pem -out private/server-key-insecure.pem
Enter pass phrase for private/server-key.pem:
writing RSA key
```

## 公钥加密,私钥解密

公钥可以公开给第三方

私钥中提取公钥

```
maos@dev:~/ssl$ openssl rsa -in private/server-key.pem -pubout -out public/server-public.pem
Enter pass phrase for private/server-key.pem:
writing RSA key
```

公钥对文件加密

```
maos@dev:~/ssl$ openssl rsautl -encrypt -inkey public/server-public.pem -pubin -in file.txt -out file.ssl
maos@dev:~/ssl$ ls
ca.srl  certs  file.ssl  file.txt  private  public  README
maos@dev:~/ssl$ cat file.txt
hello world
maos@dev:~/ssl$ cat file.ssl
70
  ~$    *7PpN]6!KdC쑸M\MSWpcc<fc.t[gt9_ϜtVE     v~BI?p.F|sӭZM+{nmaos@dev:~/ssl$
```

私钥对文件解密

```
maos@dev:~/ssl$ openssl rsautl -decrypt -inkey private/server-key.pem -in file.ssl -out decrypted.txt
Enter pass phrase for private/server-key.pem:
maos@dev:~/ssl$ cat decrypted.txt
hello world
```
