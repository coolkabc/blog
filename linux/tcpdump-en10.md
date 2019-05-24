## tcpdump: listening on eth0, link-type EN10MB (Ethernet)

相当一部分人当看到以下信息时时会怀疑监听的网卡变为`10MB`(包括我自己)

![tcpdump-en10](/imgs/tcpdump-en10.jpeg)

可以参考以下两篇文章:
- [维基百科 - 以太网](https://zh.wikipedia.org/wiki/以太网)
- [TCPDUMP - LINK-LAYER HEADER TYPES](https://www.tcpdump.org/linktypes.html)

综上:
* 以太网是目前的应用最广泛的局域网技术,经历了10Mbps、1000Mbps、1Gbps等阶段.`IEEE 802.3`对此做了一系列规范和定义.
* 显示link-type EN10MB是历史原因导致(the 10MB in the DLT_ name is historica)
