

## PBR - Policy Based Routing 策略路由

### 同一网卡分配多个网段的IP

在某Server的网卡上分配两个不同网段的IP地址: 1个业务链路、1个管理链路

实验在已有业务IP的在网卡eth5上添加管理链路IP及配置策略路由

```
ip addr add 172.16.31.2/25 dev eth5
```


创建一个路由表test（数值越小优先级越高）

```
echo "10 test" >> /etc/iproute2/rt_tables
```

为test添加路由

```
ip route add 172.16.31.0/25 dev eth5 src 172.16.31.2 table test
ip route add default via 172.16.31.1 dev eth5 table test
```
or
```
ip route add default via 172.16.31.1 dev eth5 src 172.16.31.2 table test
```

创建规则（什么样的数据包使用test表）

```
ip rule add from 172.16.31.2/25 table test
ip rule add to 172.16.31.2/25 table test
```
