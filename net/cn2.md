# CN2

CN2 - Chinatelecom Next Carrier Network，简称CNCN（CN2）

中国电信（China Telecom）有三张骨干网，分别为：

* ChinaNet（传统163骨干网）- AS 4134
* 美洲电信骨干网 - AS 36678
* CN2骨干网 - AS 4809

其中CN2骨干网细分为：

* GT（Global Transit） CN2
  * 中端产品
  * 202.97.x.x 和 59.43.x.x 并存
  * 流量高峰期可能会拥堵
* GIA（Global Internet Access）CN2
  * 高端产品
  * 去/回 全程59.43.x.x

一些信息可通过whois查询：

```
whois 59.43.0.0/16
whois as4809
```
