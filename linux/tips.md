## Linux Tips

#### 查看库信息

* objdump
* file
* strings
* readelf

#### OpenVPN + OpenSSH

Client(2.2.2.2) ---> Server(1.1.1.1)

配置ssh 端口转发

```
ssh -N -f -L 57:1.1.1.1:57 root@1.1.1.1
```

解读: -L 连接本地57端口转发到1.1.1.1的57端口

即Local 57 -> root@1.1.1.1 -> 1.1.1.1:57

当然1.1.1.1:57也可以是其它可连接机器
```
-N Do not execute a remote command.  This is useful for just forwarding ports.
-f Requests ssh to go to background just before command execution.
-L [bind_address:]port:host:hostport 也可以是socket转发;bind_address 只能是回环地址除非开启GatewayPorts; man ssh
```

OpenVPN配置

连接本地57端口即可
```
remote 127.0.0.1 57 tcp-client
```

拓展: 站在Server角度执行
```
ssh -N -f -R 57:127.0.0.1:57 root@2.2.2.2
```
解读: 在远端(Client port 57)监听57端口,远端57端口的连接都会转发到本地(Server)57端口

即 57(Client) --> root@2.2.2.2. --> 127.0.0.1:57(Server)

当然127.0.0.1:57也可以是其它可连接机器
```
-R [bind_address:]port:host:hostport  也可以是socket转发;bind_address 只能是回环地址除非开启GatewayPorts; man ssh
```

#### Mac 连 console

```
screen /dev/cu.usbserial 9600
```

