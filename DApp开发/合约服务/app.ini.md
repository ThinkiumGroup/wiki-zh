创建 app.ini 配置文件



```
[http]
port = ":8101"


[rpc]
;rpc 地址，以下是测试地址，上线要换成线上地址proxy.thinkiumrpc.net
rpc = "test.thinkiumrpc.net"

[encrypt]
;key 必须为 16, 24, 32 位
;接口中传入私钥的对称加密密码,接口中businessId参数传1,即代表使用的是key1
key1="yitawzdk3ueqarfq"
key2="azrtiabarda3ufgq"


[mysql]
;hash查交易时返回的结果都需要通过该库的contract表进行反解,在合约发布时会插入到contract表中
;可以不配置，不配置数据保存在嵌入式数据库sqkite
url = "username:password@tcp(ip:port)/dbName?charset=utf8&parseTime=true&loc=Local"
```



需将以上五处变量替换为自己的配置

**username**: 数据库读写权限账户名

**password** ：useranme对应的密码

**ip**: 数据库所在机器ip

**port**: mysql服务端口

**dbName**: 数据库名称

