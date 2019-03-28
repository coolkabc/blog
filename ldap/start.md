# 常见名词解释或规范

* Schema：模式 定义对象类
* objectClass：对象类
  * 结构型（structural）：如person
  * 辅助型（auxiliary）：如extensibleObject
  * 抽象型（abstract）：如top，不能直接使用
* Entry：条目。目录树中一个具体的对象。对象类和属性组合成条目
* DN: Distinguished Name 全局唯一名字 例：uid=babs,ou=People,dc=example,dc=com.
* RDN: Relative DN 相对名字 例：uid=babs
* DC: Domain Component 域组件？例：dc=test,dc=com
* O: organizations 组织 例如：dc=example
* OU: Organizational Units 组织单元 例：ou=People
* CN: Common Name 用户名或服务器名
* C：Country 国家
* LDIF：LDAP Data Interchanged Format 轻量级目录访问协议数据交换格式。存储LDAP配置信息及目录内容的标准文本文件格式。
* OLC: On-Line Configuration

* DIT: Directory Information Tree 目录信息树
```
LDAP directory servers present data arranged in tree-like hierarchies in which 
each entry may have zero or more subordinate entries. 
This structure is called the Directory Information Tree, or DIT. 
```

* Root DSE: Root Directory Special Entry 根特殊条目，其DN是零长度字符串

```
All LDAP servers must expose a special entry, called the root DSE, 
whose DN is the zero-length string. 
This entry will be described in detail below, 
but one of the operational attributes that it exposes is called namingContexts, 
which provides a list of all of the DNs that act as naming contexts for the DITs
that may be held in the server. 
```

[参考链接dit-and-the-ldap-root-dse](https://ldap.com/dit-and-the-ldap-root-dse/)

