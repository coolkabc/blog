# OpenLDAP 安装 

## 系统环境 Ubuntu 16.04.6

* apt-get install slapd # OpenLDAP server
* apt-get install ldap-utils # OpenLDAP utilities

## 默认配置

默认黑盒，通过服务脚本/etc/init.d/slapd进行追踪和信息确认
> 这样做主要是避免环境差异导致的认知错误

```
 31 # Source the init script configuration
 32 if [ -f "/etc/default/slapd" ]; then
 33     . /etc/default/slapd
 34 fi
 35
 36 # Load the default location of the slapd config file
 37 if [ -z "$SLAPD_CONF" ]; then
 38     if [ -e /etc/ldap/slapd.d ]; then
 39         SLAPD_CONF=/etc/ldap/slapd.d
 40     else
 41         SLAPD_CONF=/etc/ldap/slapd.conf
 42     fi
 43 fi
```

基于以上信息：

* 如果存在/etc/default/slapd文件则执行加载
* 如果存在/etc/ldap/slapd.d文件则SLAPD_CONF=/etc/ldap/slapd.d，否则SLAPD_CONF=/etc/ldap/slapd.conf

接着往下看或直接搜索SLAPD_CONF

```
 53 # extend options depending on config type
 54 if [ -f "$SLAPD_CONF" ]; then
 55     SLAPD_OPTIONS="-f $SLAPD_CONF $SLAPD_OPTIONS"
 56 elif [ -d "$SLAPD_CONF" ] ; then
 57     SLAPD_OPTIONS="-F $SLAPD_CONF $SLAPD_OPTIONS"
 58 fi
```

信息补充：

* 会对SLAPD_CONF是文件还是目录进行判断。通过slapd -h也可以看出，-f参数指定的是配置文件，-F指定的是配置目录。

## 配置信息确认

```
test@ubuntu:/etc/ldap$ ls -l /etc/ldap
total 16
-rw-r--r-- 1 root     root      332 Nov 13 11:29 ldap.conf
drwxr-xr-x 2 root     root     4096 Nov 13 11:29 sasl2
drwxr-xr-x 2 root     root     4096 Mar 20 07:38 schema
drwxr-xr-x 3 openldap openldap 4096 Mar 20 07:38 slapd.d
```

存在slapd.d并且是目录，所以/etc/ldap/slapd.d是配置目录。

## 基于Git做到配置信息持续跟踪

这时候很多Baidu/Goole过相关OpenLDAP信息的人会有疑惑，缺少slapd.conf文件

但是如果通读过OpenLDAP文档的人，应该不会有此疑惑。在[`5. Configuring slapd`](http://www.openldap.org/doc/admin24/slapdconf2.html)章节有描述
```
OpenLDAP 2.3 and later have transitioned to using a 
dynamic runtime configuration engine, slapd-config(5). slapd-config(5)

The older style slapd.conf(5) file is still supported, 
but its use is deprecated and support for it will be withdrawn in a future OpenLDAP release.
```

至此，软件安装完毕，配置目录也已确认。建议通过git维护/etc/ldap目录

```
cd /etc/ldap
git init
git add *
git commit -m '/etc/ldap/ init'
```

