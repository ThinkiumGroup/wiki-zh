### GitHub

地址：[https://github.com/ThinkiumGroup/web3j](https://github.com/ThinkiumGroup/web3j)



### 开发环境

jdk 版本：1.8

在执行所有的链上交互接口前，要获取与链的连接实例 web3j。httpService 参数为 rpc地址(注意要使用 v2版本)

```java
protected static final Web3j web3j = Web3j.load(new HttpService("http://test.thinkiumrpc.net/v2"));
```


## base-chain-id
使用前需指定base-chain-id，代码如下

```java
Transaction.chainIdBase = 60000L; // 测试环境
Transaction.chainIdBase = 70000L; // 生产环境
```


### 1. 获取账户情况(web3.GetAccount)

请求参数：

| 参数名 | 类型 | 是否必须| 含义 |
| :------:| :------: | :------: | :------: |
| chainId | int | true | 链id |
| address | string | true | 账户地址 |

响应参数：
| 参数名 | 类型 | 是否必须| 含义 |
| :------:| :------: | :------: | :------: |
| address | string | true | 账户地址 |
| nonce | int | true | 交易的发起者在之前进行过的交易数量 |
| balance| bigint | true | 账户余额 |
| storageRoot| string | false | 合约存储数据的hash(没有合约返回null) |
|codeHash| string | false | 合约代码的hash(没有合约返回null) |

请求示例：
```java
String chainId = "2";
String address = "0x2c7536e3605d9c16a7a3d7b1898e529396a65c23";
Map map = web3.GetAccount(chainId,address);
System.out.println(map);
```
返回结果：

```json
{
  "address": "0x2c7536e3605d9c16a7a3d7b1898e529396a65c23",
  "balance": 1e+27,
  "codeHash": null,
  "nonce": 8,
  "storageRoot": null
}
```



### 2. 执行一笔交易(web3.SendTX)

请求参数：
           
|   参数名    |  类型  | 是否必须 |                含义                |
| :---------: | :----: | :------: | :--------------------------------: |
|   chainId   | string |   true   |                链id                |
| fromChainId | string |   true   |       交易发起账户地址的链id       |
|  toChainId  | string |   true   |       交易接受账户地址的链id       |
|    from     | string |   true   |          交易发起账户地址          |
|     to      | string |   true   |          交易接受账户地址          |
|    nonce    | string |   true   | 交易的发起者在之前进行过的交易数量 |
|    value    | string |   true   |              转账金额              |
|    input    | string |   true   |          调用合约时的参数          |
|     sig     | string |   true   |              交易签名              |
|     pub     | string |   true   |                公钥                |

响应参数:
           
| 参数名 |  类型  | 是否必须 |   含义   |
| :----: | :----: | :------: | :------: |
| TXhash | string |   true   | 交易hash |


请求示例:
```java
String chainId22="2";
String fromChainId="2";
String toChainId="2";
//使用签名方法
String sig="0x3c0c75b4dea8c8335475d462bd12dae9e746e3532c6a6b2791cafca565c6610a429fb7260e2f3c64b8e6eb090ee123db700ed2c5f0a4d9a314152f721f0a847101";
String pub="0x044e3b81af9c2234cad09d679ce6035ed1392347ce64ce405f5dcd36228a25de6e47fd35c4215d1edf53e6f83de344615ce719bdb0fd878f6ed76f06dd277956de";
String from="0x2c7536e3605d9c16a7a3d7b1898e529396a65c23";
String to="0x6ea0fefc17c877c7a4b0f139728ed39dc134a967";
String nonce="33";
String value="2333";
String input="";
int ExpireHeight=0;
Map mapresult=web3.SendTx(chainId22,fromChainId,toChainId,sig,pub,from,to,nonce,value,input,ExpireHeight);
System.out.println(mapresult);
```
返回结果：

```json
{
	"TXhash": "0x22024c2e429196ac76d0e557ac0cf6141f5b500c56fde845582b837c9dab236b"
}
```



### 3. 通过交易hash获取交易详情(web3.GetTransactionByHash)

请求参数：
| 参数名  |  类型  | 是否必须 |   含义   |
| :-----: | :----: | :------: | :------: |
| chainId | string |   true   |   链id   |
|  hash   | string |   true   | 交易hash |


响应参数:
           
|     参数名      |    类型     | 是否必须 |                    含义                     |
| :-------------: | :---------: | :------: | :-----------------------------------------: |
|   Transaction   |    dict     |   true   |                  交易详情                   |
|      root       |   string    |   true   | 保存了创建该receipt对象时，“账户”的当时状态 |
|     status      |     int     |   true   |          交易状态: 1:成功, 0:失败           |
|      logs       | array[dict] |  false   |         这个交易产生的日志对象数组          |
| transactionHash |   string    |   true   |                  交易hash                   |
| contractAddress |   string    |   true   |                合约账户地址                 |
|       out       |   string    |   true   |              调用返回结果数据               |

Transaction:
           
| 参数名  |  类型  | 是否必须 |                含义                |
| :-----: | :----: | :------: | :--------------------------------: |
| chainID |  int   |   true   |                链id                |
|  from   | string |   true   |          交易发起账户地址          |
|   to    | string |   true   |          交易接受账户地址          |
|  nonce  | string |   true   | 交易的发起者在之前进行过的交易数量 |
|   val   | string |   true   |              转账金额              |
|  input  | string |   true   |          调用合约时的参数          |


请求示例:
```java
chainId="3";
String hash = "0xb298a034848ea3fccf421824c9bc42d1525994843fcda67fd01ca66f16128ebe";
Map maptx = web3.GetTransactionByHash(chainId,hash);
System.out.println(maptx);
```
返回结果：

```json
{
  "Transaction": {
    "chainID": 3,
    "from": "0x2c7536e3605d9c16a7a3d7b1898e529396a65c23",
    "input": "0x95000000022c7536e3605d9c16a7a3d7b1898e529396a65c230000000000000042000000034fa1c4e6182b6b7f3bca273390cf587b50b473110000000000045644010102a304471cc04daacb7b7c7aedc99641e5ca5698acae3026669f3939f6757f239afc6bf6aefc94941093a1a0df8e3f6a5bf468075826c85d00b1c83ff3b1268f6e8fcbf4b1b090fc02a06021c200008080940d934080c20028808100017a92957e842499e3eb05f3e89683be8d14dd8e753f3b02355dbf39eccfb3d6b20001019403934080c2ffff8081000495f5abd6dfe47cfa3bcfdf88a636d92f7047dbc6b2fabb33da5c71c21e193989451a4767cd1265d629f7c4a6dc298704c2f9d9342c603d11220ca99dffff70ea0c32794c7415e80e33bf93bf6fcdd8ddd2260197fab5690601451612729506523ec792c1c2439978bd9cb5c23d3dc06649cfccaef1d5ef9dd5c507d1cc9c2a6a0001039424930080c20000c08f919b95ead8dbcf25cb16eb3b6d7bc79ce9e6af2823e377e44f1d725f2f056e810005fcb6c9e006ebd8699e4b0aa00a1391e2b3aca513e7fca5fb3c4098deed27a593bb8ef4e4dce46ba0743ecaf6e7fe2f916324512d1725cea4e15cebb6e821bcfb1c41807fcb3b674579cd535c46f2822bb70ee83200ad68576b064c21d2e01690eca078605c1b0ad6ff4323f7c23307585d3dddd504f96e7a7f722f9802d2a1b74fca1f5de7c524d6658648ee28c95512adc78dcd0f440b2fd6600cc51ea2326e000110",
    "nonce": 17,
    "to": "0x0000000000000000000000000000000000030000",
    "value": 0
  },
  "blockHeight": 280293,
  "contractAddress": "0x0000000000000000000000000000000000000000",
  "logs": null,
  "out": "0x3d8e3f6a5bf468075826c85d00b1c83ff3b1268f6e8fcbf4b1b090fc02a06021",
  "root": null,
  "status": 1,
  "transactionHash": "0x265954c4863005eba23c57d3fb321626979e3c34d84e3b8ae967719f7b3192e4"
}
```



### 4. 获取链信息(web3.getStats)

请求参数
| 参数名  |  类型  | 是否必须 | 含义 |
| :-----: | :----: | :------: | :--: |
| chainId | string |   true   | 链id |


响应参数:
           
|      参数名       |  类型  | 是否必须 |          含义          |
| :---------------: | :----: | :------: | :--------------------: |
|   currentheight   | bigint |   true   |        当前块高        |
|      txcount      |  int   |   true   |        总交易数        |
|        tps        |  int   |   true   |       每秒交易数       |
|   tpsLastEpoch    |  int   |   true   |     上一时期交易数     |
|       lives       |  int   |   true   |     链的已存活时间     |
|   accountcount    |  int   |   true   |         账户数         |
|    epochlength    |  int   |   true   |   当前时期包含多少块   |
|   epochduration   |  int   |   true   |    当前时期运行时间    |
| lastepochduration |  int   |   true   |   上一时期的运行时间   |
|    currentcomm    | array  |   true   | 当前这条链的委员会成员 |

请求示例:
```java
ChainId="2";
Map mapstas=web3.GetStats(chainId);
System.out.println(mapstas);
```
返回结果：

```json
{
  "accountcount": 0,
  "currentcomm": [
  "0x6e4a2f626a7d6dd082b713acdd3be49872097ab79f664f807095f665a50c68803688d1a242c8580ce6e8355468f666d1d43e3bdb0deebd0df17f97e1ea397355",
  "0xd1f889690f8c75bbada89a4c8893b8bf6fe29be3b5c3d8a2d772024a340d59d375f39ed88498666a57da10af885ad63a414f8a10153fb739eb1ebfcef57cc883",
  "0xa9d8dd87c9ece1787cbb16ba179df6d1d4b29580c03ae1da6e4634b154f8e2feb052fc1c07ca1e362f9b3e2b5f43822df9dd6578e782701de83afeca91a7b453"
  ],
  "currentheight": 280413,
  "epochduration": 202,
  "epochlength": 80,
  "lastepochduration": 202,
  "lives": 64362,
  "tps": 0,
  "tpsLastEpoch": 0,
  "txcount": 10
}
```



### 5. 获取指定账户在对应链上一定高度范围内的交易信息(web3.GetTransactions)

请求参数
|   参数名    |  类型  | 是否必须 |      含义      |
| :---------: | :----: | :------: | :------------: |
|   chainId   | string |   true   |      链id      |
|   address   | string |   true   |     链地址     |
| startHeight | string |   true   | 查询的起始块高 |
|  endHeight  | string |   true   | 查询的截止块高 |


响应参数:
|  参数名   |  类型  | 是否必须 |                含义                |
| :-------: | :----: | :------: | :--------------------------------: |
|  chainId  |  int   |   true   |                链id                |
|   from    | string |   true   |          交易发起账户地址          |
|    to     | string |   true   |          交易接受账户地址          |
|   nonce   |  int   |   true   | 交易的发起者在之前进行过的交易数量 |
|   value   |  int   |   true   |              转账金额              |
| timestamp |  int   |   true   |            交易的时间戳            |
|   input   | string |   true   |          调用合约时的参数          |
|   hash    | string |   true   |              交易hash              |


请求示例:
```java
String  chainIdtxs="2";
String  address_gettx="0x2c7536e3605d9c16a7a3d7b1898e529396a65c23";
String  startHeight="50";
String  endHeight="100";
JSONArray arr=web3.GetTransactions(chainIdtxs,address_gettx,startHeight,endHeight);
System.out.println(arr);
```

返回结果：

```json
[
    {
        "chainId": 2,
        "from": "0x2c7536e3605d9c16a7a3d7b1898e529396a65c23",
        "to": "0x4b69a38eed4c24fb85ca79146d7c07392136e178",
        "nonce": 0,
        "value": 100000,
        "input": "0x",
        "hash": "0x473de49860222d0eb72a94544d31b87ecf5079f51f7dc619a61828ff8e903a0e",
        "timestamp": 1563332880
    }
]
```



### 6. 调用交易（web3.CallTransaction）

请求参数
|   参数名    |  类型  | 是否必须 |                含义                |
| :---------: | :----: | :------: | :--------------------------------: |
|   chainId   | string |   true   |                链id                |
| fromChainId | string |   true   |       交易发起账户地址的链id       |
|  toChainId  | string |   true   |       交易接受账户地的链id址       |
|    from     | string |   true   |          交易发起账户地址          |
|     to      | string |   true   |          交易接受账户地址          |
|    nonce    |  int   |   true   | 交易的发起者在之前进行过的交易数量 |
|    value    |  int   |   true   |              转账金额              |
|    input    | string |   true   |          调用合约时的参数          |


响应参数:

|     参数名      |    类型     | 是否必须 |                    含义                     |
| :-------------: | :---------: | :------: | :-----------------------------------------: |
|     chainId     |     int     |   true   |                    链id                     |
|      from       |   string    |   true   |              交易发起账户地址               |
|       to        |   string    |   true   |              交易接受账户地址               |
|      nonce      |     int     |   true   |     交易的发起者在之前进行过的交易数量      |
|      value      |     int     |   true   |                  转账金额                   |
|      input      |   string    |   true   |              调用合约时的参数               |
|      hash       |   string    |   true   |                  交易hash                   |
|    timestamp    |     int     |   true   |                   时间戳                    |
|      root       |   string    |   true   | 保存了创建该receipt对象时，“账户”的当时状态 |
|     status      |     int     |   true   |          交易状态: 1:成功, 0:失败           |
|      logs       | array[dict] |   true   |         这个交易产生的日志对象数组          |
| transactionHash |   string    |   true   |                  交易hash                   |
| contractAddress |   string    |   true   |                合约账户地址                 |
|       out       |   string    |   true   |              调用返回结果数据               |
|   blockHeight   |     int     |   true   |                    块高                     |

请求示例:
```java
Transaction info_call=new Transaction();
info_call.setChainId("2");
info_call.setFrom("0x0000000000000000000000000000000000000000");
info_call.setTo("0x0e50cea0402d2a396b0db1c5d08155bd219cc52e");
info_call.setNonce("15");
info_call.setValue("0");
info_call.setInput("0xdfc02018");
info_call.setFromChainId("2");
info_call.setToChainId("2");
Map calltrResult=web3.CallTransaction(info_call);
System.out.println(calltrResult);
```
返回结果：

```json
{
  "Transaction":{
    "input":"0xdfc02018",
    "chainId":2,
    "from":"0x0000000000000000000000000000000000000000",
    "to":"0x0e50cea0402d2a396b0db1c5d08155bd219cc52e",
    "nonce":0,
    "value":0,
    "hash":"",
    "timestamp":0
  },
  "blockHeight":0,
  "root":"",
  "contractAddress":"0x0000000000000000000000000000000000000000",
  "logs":"",
  "transactionHash":"0xb5a31778f88df4816701e73d1cd67df8b836881766aeb93f3ba8c36bb37f7808",
  "status":1,
  "out":"0x"
}
```



### 7. 获取指定块高信息(web3.GetBlockHeader)

请求参数
| 参数名  |  类型  | 是否必须 |     含义     |
| :-----: | :----: | :------: | :----------: |
| chainId | string |   true   |     链id     |
| height  | string |   true   | 查询块的块高 |

响应参数:
|    参数名    |  类型  | 是否必须 |          含义          |
| :----------: | :----: | :------: | :--------------------: |
|     hash     | string |   true   |       此块的hash       |
| previoushash | string |   true   |       父块的hash       |
|   chainid    |  int   |   true   |          链id          |
|    height    |  int   |   true   |      查询块的块高      |
|  mergeroot   | string |   true   | 合并其他链转账数据hash |
|  deltaroot   | string |   true   |    跨链转账数据hash    |
|  stateroot   | string |   true   |        状态hash        |
|   txcount    |  int   |   true   |        交易总数        |
|  timestamp   |  int   |   true   |         时间戳         |

请求示例:
```java
String  height="30";
Map mapHeightr=web3.GetBlockHeader(chainId2,height);
System.out.println(mapHeightr);
```
返回结果：

```json
{
  "hash": "0x71603186004fd46d32cda0780c4f4cf77ce13b396b1b8132b2c632173441b9d2",
  "previoushash": "0xd0f6e9c89eb6be655632911e3743b5a994423c3526653dc55b62ebea3ff56c43",
  "chainid": 2,
  "height": 30,
  "mergeroot": "0xdddfde85423a0d7da064c1b5a8cc1ff18d4a209027ef95ecceae0e6ed8f7c1af",
  "deltaroot": "0xdddfde85423a0d7da064c1b5a8cc1ff18d4a209027ef95ecceae0e6ed8f7c1af",
  "stateroot": "0x0b672749b02da6bf8f3aa50238140ce7fae5af3e926d4eb06d4cfb707a90702e",
  "txcount": 1,
  "timestamp": 1547777358
}
```



### 8. 获取指定块的交易(web3.getBlockTxs)

请求参数：
| 参数名  |  类型  | 是否必须 |     含义     |
| :-----: | :----: | :------: | :----------: |
| chainId | string |   true   |     链id     |
| height  | string |   true   | 查询块的块高 |
|  page   | string |   true   |     页码     |
|  size   | string |   true   |   页的大小   |

响应参数：
|     参数名     | 类型  | 是否必须 |   含义   |
| :------------: | :---: | :------: | :------: |
|   elections    | dict  |   true   | 交易详情 |
| accountchanges | array |   true   | 交易信息 |

accountchanges:         
|  参数名   |  类型  | 是否必须 |                含义                |
| :-------: | :----: | :------: | :--------------------------------: |
|  chainId  | string |   true   |                链id                |
|  height   |  int   |   true   |           查询的起始块高           |
|   from    | string |   true   |          交易发起账户地址          |
|    to     | string |   true   |          交易接受账户地址          |
|   nonce   |  int   |   true   | 交易的发起者在之前进行过的交易数量 |
|   value   |  int   |   true   |              转账金额              |
| timestamp |  int   |   true   |            交易的时间戳            |


请求示例:
```java
String  height84="84";
String  page="1";
String  size="10";
Map maptxs=web3.GetBlockTxs(chainId2,height84,page,size);
System.out.println(maptxs);
```
响应参数：

```json
{
  "elections": null,
  "accountchanges": [
    {
      "chainid": 2,
      "height": 30,
      "from": "0x4fa1c4e6182b6b7f3bca273390cf587b50b47311",
      "to": "0x4fa1c4e6182b6b7f3bca273390cf587b50b47311",
      "nonce": 30,
      "value": 1,
      "timestamp": 1547777358
    }
  ]
}
```



### 9 合约调用

//先生成：abi和bin
使用命令：solc Greeter.sol  --bin --abi --optimize -o ./output/
Solc 工具下载地址: https://github.com/ethereum/solidity/releases/tag/v0.4.25
Output 为.abi 和.bin 生成目录

#### 9.1 发布合约(Contract.Deploy)

请求参数：
|   参数名    |    类型     | 是否必须 |       含义        |
| :---------: | :---------: | :------: | :---------------: |
| Transaction | Transaction |   true   |   交易实体Model   |
| binContent  |   string    |   true   | 生成的bin文件内容 |
| parameters  | List:Type  |   true   |    传入的参数     |

响应参数：
| 参数名 |  类型  | 是否必须 |   含义   |
| :----: | :----: | :------: | :------: |
| TXhash | string |   true   | 交易Hash |

请求示例:
```java
//发布合约
Map result= Contract.Deploy(info,binContent, Collections.emptyList());
System.out.println("sendtx result :" +result);
Thread.sleep(5000);    //延时5秒
```
响应参数：

```json
{
	"TXhash":"0xbcf864ce51279dbe23866c216a22e4cda4bde5464fb35f68ee9efca0c040e2e5"
}
```

#### 9.2 需要共识且修改数据状态的合约调用(Contract.Send)

请求参数：
|   参数名    |    类型     | 是否必须 |     含义      |
| :---------: | :---------: | :------: | :-----------: |
| Transaction | Transaction |   true   | 交易实体Model |
|  function   |  Function   |   true   |     函数      |

响应参数：
| 参数名 |  类型  | 是否必须 |   含义   |
| :----: | :----: | :------: | :------: |
| TXhash | string |   true   | 交易Hash |

请求示例:
```java
Map resultSend= Contract.Send(info,function1);
System.out.println("resultSend : "+resultSend);
```
响应参数：

```json
{
	"TXhash":"0xbcf864ce51279dbe23866c216a22e4cda4bde5464fb35f68ee9efca0c040e2e5"
}
```


#### 9.3 从本地节点获取数据的合约调用(Contract.Call)

请求参数：
|   参数名    |    类型     | 是否必须 |     含义      |
| :---------: | :---------: | :------: | :-----------: |
| Transaction | Transaction |   true   | 交易实体Model |
|  function   |  Function   |   true   |     函数      |

响应参数：
|     参数名      |    类型     | 是否必须 |                      含义                       |
| :-------------: | :---------: | :------: | :---------------------------------------------: |
|   Transaction   | Transaction |   true   |                  交易实体Model                  |
|   blockHeight   |     int     |   true   |                      块高                       |
|      root       |   string    |   true   | 保存了创建该 Receipt 对象时,“ 帐户 ” 的当时状态 |
| contractAddress |   string    |   true   |                    合约地址                     |
|      logs       |   string    |   true   |                      日志                       |
| transactionHash |   string    |   true   |                    交易Hash                     |
|     status      |     int     |   true   |                    是否成功                     |
|       out       |   string    |   true   |                调用返回结果数据                 |


Transaction:
|  参数名   |  类型  | 是否必须 |                含义                |
| :-------: | :----: | :------: | :--------------------------------: |
|  chainId  | string |   true   |                链id                |
|   from    | string |   true   |          交易发起账户地址          |
|    to     | string |   true   |          交易接受账户地址          |
|   nonce   |  int   |   true   | 交易的发起者在之前进行过的交易数量 |
|   value   |  int   |   true   |              转账金额              |
| timestamp |  int   |   true   |            交易的时间戳            |
|   input   | string |   true   |          调用合约时的参数          |
|   hash    | string |   true   |              交易hash              |

请求示例:
```java
Map resultCall = Contract.Call(info,function);
System.out.println(resultCall);
```
响应参数：

```json
{
  "Transaction":{
    "input":"0xcfae3217",
    "chainId":2,
    "from":"0x2c7536e3605d9c16a7a3d7b1898e529396a65c23",
    "to":"0xd39df28bd62f487a2272ce9df499167de16373bb",
    "nonce":345,
    "value":0,
    "hash":"",
    "timestamp":0
  },
  "blockHeight":0,
  "root":"",
  "contractAddress":"0x0000000000000000000000000000000000000000",
  "logs":"",
  "transactionHash":"0x06990881f0b8af53d4a8bdf386531c55373ceb043456c536250b3ea4f5160b33",
  "status":1,  		 	"out":"0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000a4772656574696e67732100000000000000000000000000000000000000000000"
}
```





### 10.web3.Ping

请求参数
| 参数名  |  类型  | 是否必须 |  含义   |
| :-----: | :----: | :------: | :-----: |
| address | string |   true   | ip+端口 |

响应参数:
|    参数名     |  类型  | 是否必须 |      含义      |
| :-----------: | :----: | :------: | :------------: |
|    nodeId     | string |   true   |     节点id     |
|    version    | string |   true   |      版本      |
|  isDataNode   |  bool  |   true   | 是否是数据节点 |
|  dataNodeOf   |  int   |   true   |    数据节点    |
|  lastMsgTime  | int64  |   true   | 上一个信息时间 |
| lastEventTime | int64  |   true   | 上一个事件时间 |
| lastBlockTime | int64  |   true   |  上一个块时间  |
|   overflow    |  bool  |   true   |      溢出      |
|  lastBlocks   |  map   |   true   |   最后一个块   |
|    opTypes    |  map   |   true   |      类型      |


请求示例:
```java
String ipAddress="192.168.1.7:22007";
Map mapPing=web3.Ping(ipAddress);
System.out.println(mapPing);
```
响应参数：

```json
{
    "nodeId": "0x84385cc16d8e0a47909ee998d51370e5f56d7c85716e045c99760bedb180346da7d00b575ba23b76ffcd0969ae84e1e6b6943ec408f40b44825128577d8a895d",
    "version": "V1.0.0",
    "isDataNode": false,
    "dataNodeOf": 0,
    "lastMsgTime": 1563351861,
    "lastEventTime": 1563351861,
    "lastBlockTime": 1563351860,
    "overflow": false,
    "lastBlocks": {
        "0": 6889,
        "1": 6899,
        "2": 4176,
        "3": 5323,
        "4": 3299,
        "5": 4993,
        "6": 1199
    },
    "opTypes": {
        "0": [
            "SPEC"
        ],
        "2": [
            "SPEC"
        ],
        "3": [
            "COMM"
        ],
        "5": [
            "COMM"
        ]
    }
}
```





### 11. 生成支票的证明(web3.RpcMakeVccProof)


请求参数

|    参数名    |  类型  | 是否必须 |                含义                |
| :----------: | :----: | :------: | :--------------------------------: |
|   chainId    | string |   true   |                链id                |
| fromChainId  | string |   true   |       交易发起账户地址的链id       |
|  toChainId   | string |   true   |       交易接受账户地址的链id       |
|     from     | string |   true   |          交易发起账户地址          |
|      to      | string |   true   |          交易接受账户地址          |
|    nonce     | string |   true   | 交易的发起者在之前进行过的交易数量 |
|    value     | string |   true   |              转账金额              |
| ExpireHeight |  int   |   true   |              过期高度              |

响应参数:
| 参数名 |  类型  | 是否必须 |      含义      |
| :----: | :----: | :------: | :------------: |
| input  | string |   true   | 生成的支票证明 |


请求示例:
```java
RpcMakeVccProof rpcInfo=new RpcMakeVccProof();
rpcInfo.setChainId("3");
rpcInfo.setFrom("0x2c7536e3605d9c16a7a3d7b1898e529396a65c23");
rpcInfo.setTo("0x4fa1c4e6182b6b7f3bca273390cf587b50b47311");
rpcInfo.setFromChainId("2");
rpcInfo.setToChainId("3");
rpcInfo.setValue("1");
rpcInfo.setExpireheight("284228");
rpcInfo.setNonce("10");
Map mapVccResult=web3.RpcMakeVccProoff(rpcInfo);
System.out.println(mapVccResult);
```
响应参数：

```json
{
    "input": "0x95000000022c7536e3605d9c16a7a3d7b1898e529396a65c23000000000000000a000000034fa1c4e6182b6b7f3bca273390cf587b50b473110000000000045644010102a227d7c068bc00e1a0cb6566a508a7044b8e5d11ea3f8b359a59c2406b80c6bfb23766f092941093a1a0c05cad87667a882bb66fcc3dbcdd048a82529855fd3c8304999fe7651293d450edc2000080809424930080c20000c07f4b4267a5fe52834d41b3ed6405a1e72e96eb1aec43f6f4c69c42c92bf9661a810005847b691757b57ef3808b5050c3b35e524c23e27203998b6b0665a2529f08ad605e8333471bbdde23213cc02b3d04c688f1e3d4e0e707ad1c9cf2bfad5b2fa770eadc5fc028bf8f8760f5fa69c3621d18a37000082fadf62b870e633cdf47a7c3eca078605c1b0ad6ff4323f7c23307585d3dddd504f96e7a7f722f9802d2a1b766e78588af6a85bab7314a91829366343792f6ff584f719bba43bc3ebc1423a0000110"
}
```





### 12. 生成取消支票的证明(web3.MakeCCCExistenceProof)

请求参数
|    参数名    |  类型  | 是否必须 |                含义                |
| :----------: | :----: | :------: | :--------------------------------: |
|   chainId    | string |   true   |                链id                |
| fromChainId  | string |   true   |       交易发起账户地址的链id       |
|  toChainId   | string |   true   |       交易接受账户地址的链id       |
|     from     | string |   true   |          交易发起账户地址          |
|      to      | string |   true   |          交易接受账户地址          |
|    nonce     | string |   true   | 交易的发起者在之前进行过的交易数量 |
|    value     | string |   true   |              转账金额              |
| ExpireHeight |  int   |   true   |              过期高度              |


响应参数:
|  参数名   |  类型  | 是否必须 |       含义       |
| :-------: | :----: | :------: | :--------------: |
|   input   | string |   true   | 调用合约时的参数 |
| existence |  bool  |   true   |   是否存过支票   |


请求示例:
```java
MakeCCCExistenceProof parInfo=new MakeCCCExistenceProof();
parInfo.setChainId("3");
parInfo.setFrom("0x2c7536e3605d9c16a7a3d7b1898e529396a65c23");
parInfo.setTo("0x4fa1c4e6182b6b7f3bca273390cf587b50b47311");
parInfo.setFromChainId("3");
parInfo.setToChainId("3");
parInfo.setValue("1");
parInfo.setExpireheight("33772");
parInfo.setNonce("9");
Map mapccc=web3.MakeCCCExistenceProof(parInfo);
System.out.println(mapccc);
```
响应参数：

```json
{
    "existence": false,
    "input": "0x96000000032c7536e3605d9c16a7a3d7b1898e529396a65c230000000000000009000000034fa1c4e6182b6b7f3bca273390cf587b50b4731100000000000083ec010103a2272cc0bc1dd5c595436856c7d7a901a8c4059b1ba44c0bd784817b014302a6c8c4fcb19194a1fe930080c200008080919425930080c20000c013866c071f3ec74b6a5c734f28b4ff2b82c9a274fc439af4c98fa0d979beed5f8100054a3c7dd49fdfc74ef359c98a4c7cd9d4acccbbca7e3febddbcccbb186fcc66764cafef79863477a77e0a3ebc512568f3f49e77c893f3dc635b53ad75b88de8e57f0ee271f516ce7640ff71573df432f30fd39ffaf19eba2bdef8197cbe5ca585eca078605c1b0ad6ff4323f7c23307585d3dddd504f96e7a7f722f9802d2a1b7427ea90f8d2785126ec64ed6f65c7bdc1ec5daf4af7306f8989a38d59ec43948000111"
}
```



### 13.获取链结构（web3.GetChainInfo）

请求参数
|  参数名  | 类型  | 是否必须 |            含义            |
| :------: | :---: | :------: | :------------------------: |
| chainIds | []int |   true   | 链id（备注：传空代表所有） |


响应参数:
| 参数名 |    类型     | 是否必须 |    含义    |
| :----: | :---------: | :------: | :--------: |
|   []   | []chainInfo |   true   | 链信息数组 |

chainInfo：
|  参数名   |    类型    | 是否必须 |    含义    |
| :-------: | :--------: | :------: | :--------: |
|  chainId  |    int     |   true   |    链id    |
| datanodes | []dataNode |   true   | 数据节点群 |
|   mode    |    int     |   true   |    模式    |
|  parent   |    int     |   true   |     父     |

dataNode：
|    参数名    |  类型  | 是否必须 |     含义     |
| :----------: | :----: | :------: | :----------: |
|  dataNodeId  |  int   |   true   |  数据节点id  |
|  dataNodeIp  | string |   true   |  数据节点ip  |
| dataNodePort |  int   |   true   | 数据节点端口 |

请求示例:
```java
JSONArray result_ChainInfo=web3.GetChainInfo();
System.out.println(result_ChainInfo);
```
响应参数：

```json
[
  {
    "chainId": 0,
    "datanodes": [
      {
      "dataNodeId": "0x5e17128ba224a96d6e84be0c7f899febea26c55c78940610d78a0d22dbd0ab03cc3233491de0b5eb770dbf850b509bd191723df4fc40520bcbab565d46543d6e",
      "dataNodeIp": "192.168.1.13",
      "dataNodePort": 22010
      }
    ],
    "mode": 5,
    "parent": 1048576
  },
  {
    "chainId": 1,
    "datanodes": [
      {
        "dataNodeId": "0x96dc94580e0eadd78691807f6eac9759b9964daa8b46da4378902b040e0eb102cb48413308d2131e9e5557321f30ba9287794f689854e6d2e63928a082e79286",
        "dataNodeIp": "192.168.1.13",
        "dataNodePort": 22014
      }
    ],
    "mode": 6,
    "parent": 0
  },
  {
    "chainId": 2,
    "datanodes": [
      {
        "dataNodeId": "0xa93b150f11c422d8700554859281be8e34a91a859e0e021af186002c7e4a2661ea2467a63b417030d68e2fdddeb4342943dff13225da77124abf912fd092f71f",
        "dataNodeIp": "192.168.1.13",
        "dataNodePort": 22018
      }
    ],
    "mode": 6,
    "parent": 0
  },
  {
    "chainId": 3,
    "datanodes": [
      {
        "dataNodeId": "0x783f4b2490461ecfd8ee8d3451e434de06bacb0ffff56de53a33fe545589094fa0b929eeaa62dc5203d1e831ccdd37d206d0b85b193921efb223bf0cb2f37b4c",
        "dataNodeIp": "192.168.1.13",
        "dataNodePort": 22022
      }
    ],
    "mode": 7,
    "parent": 1
  },
  {
    "chainId": 4,
    "datanodes": [
      {
        "dataNodeId": "0x44c98ab831f3ca4553e491bba06753e959ceb55d43e18bc76539572feb1e0dbaf2fbfc19f571d6544e82be1c7c39760f6a023d4be4dcb9473dd580c731d03926",
        "dataNodeIp": "192.168.1.13",
        "dataNodePort": 22026
      }
    ],
    "mode": 7,
    "parent": 1
  }
]
```





### 14.获取委员会成员信息（web3.GetCommittee）

请求参数
| 参数名  |  类型  | 是否必须 |   含义   |
| :-----: | :----: | :------: | :------: |
| chainId | string |   true   |   链id   |
|  epoch  | string |   true   | 参选轮次 |


响应参数:
|    参数名     |   类型   | 是否必须 |    含义    |
| :-----------: | :------: | :------: | :--------: |
|    chainId    |   int    |   true   |    链id    |
| MemberDetails | []string |   true   | 委员会详情 |
|     Epoch     |   int    |   true   |  参选轮次  |


请求示例:
```java
String epoch="1";
String chainId3="3";
JSONArray result_CommitteeInfo=web3.GetCommittee(chainId3,epoch);
System.out.println(result_CommitteeInfo);
```



响应参数:

```json
[       "0xfbcd643db7f1812d71f7fb66a9b6c7b877d4b44e707b5788ca041003e7f671c85f88ee79d0642fbd1294b2140e3ef5414b3bb125b92dab11696788a66f949a91",
          "0x79d260d785858034d64641b9a5867bd08e3f6cae9a3f18e09dfd1a238808b97093d74a8f1a15d9cbab68b005383a2637cce73110f3dfb5faea0a7c371f0976b9",
          "0x7f17a336b3005ef9fed2baabcdc08b31cfcd72ea84acd34b4b468d4b527d0628978ab0e541c5fe1e04d0ad01a7d892ece8920f90e12b8a55c79933167786a196",
          "0x6b81e535a8f7b0a3ed746d46e6e1248ebf001650c078f27056a20f00a2c9d3c369865872ba01dcf95485b3dfcfd921a6239b6690166e47ec47ade0e6aea593c1"
]
```

