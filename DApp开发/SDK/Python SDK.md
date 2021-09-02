### GitHub

地址：[https://github.com/ThinkiumGroup/web3.py](https://github.com/ThinkiumGroup/web3.py)



### 环境

使用 **python3**

全局声明

```python
 thk = web3.thk
```



### 1.web3.thk.getAccount（获取账户余额）

请求参数:

| 参数名 |  类型 |  是否必须 |  含义 |
| :---: | :---: | :---: | :---: |
| address | string | true |  账户地址 |

响应参数:

| **参数名 ** | **类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| address | string | true | 账户地址 |
| nonce | int | true | 交易的发起者在之前进行过的交易数量 |
| balance | bigint | true | 账户余额 |
| storageRoot | string | false | 合约存储数据的hash(没有合约返回null) |
| codeHash | string | false | 合约代码的hash(没有合约返回null) |

示例:
```python
response = web3.thk.getAccount('0x0000000000000000000000000000000000000000')
```

返回：

```json
{
   "address": "0x0000000000000000000000000000000000000000",
   "nonce": 0, 
   "balance": 0,
   "storageRoot": null,
   "codeHash": null
}
```


### 2.web3.thk.sendTx() （执行一笔交易）
请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| transaction | dict | true | 交易对象 |

transaction:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |
| fromChainId | string | true | from地址的链id |
| toChainId | string | true | to地址的链id |
| from | string | true | 交易发起账户地址 |
| to | string | true | 交易接受账户地址 |
| nonce | string | true | 交易的发起者在之前进行过的交易数量 |
| value | string | true | 转账金额 |
| input | string | true | 调用合约时的参数 |
| sig | string | true | 交易签名 |
| pub | string | true | 公钥 |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| TXhash | string | true | 交易hash |

示例:
```python
transaction = (
    	"chainId": "2",
       "fromChainId": "2",
  			"toChainId": "2",
        "from": "0x2c7536e3605d9c16a7a3d7b1898e529396a65c23",
        "nonce": "1",
        "to": "0x0000000000000000000000000000000000000000",
        "input": '',
        "value": "1111111110"
)
con_sign_tx = web3.thk.signTransaction(con_tx, privarte_key)
response = web3.thk.sendTx(con_sign_tx)
```

返回：

```json
{
    "TXhash": "0x22024c2e429196ac76d0e557ac0cf6141f5b500c56fde845582b837c9dab236b"
}
```



### 3.web3.thk.getTxByHash（通过交易hash获取交易详情）

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |
| hash | string | true | 交易hash |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| Transaction | dict | true | 交易详情 |
| root | string | true | 交易hash |
| status | int | true | 交易状态: 1:成功, 0:失败 |
| logs | array[dict] | false | 这个交易产生的日志对象数组 |
| transactionHash | string | true | 交易hash |
| contractAddress | string | true | 合约账户地址 |
| out | string | true | 调用返回结果数据 |

Transaction:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainID | string | true | 链id |
| from | string | true | 交易发起账户地址 |
| to | string | true | 交易接受账户地址 |
| nonce | string | true | 交易的发起者在之前进行过的交易数量 |
| val | string | true | 转账金额 |
| input | string | true | 调用合约时的参数 |

示例:

```javascript
var response = web3.thk.getTxByHash('2', '0x29d7eef512137c55f67a7012e814e5add45ae8b81a9ceb8e754c38e8aa5dee4d');
```

返回：

```json
{
    "Transaction": {
        "chainID": 2,
        "from": "0x0000000000000000000000000000000000000000",
        "to": null,
        "nonce": 0,
        "value": 0,
        "input": "0x6080604052600160005534801561001557600080fd5b50600260008190555060a18061002c6000396000f300608060405260043610603f576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168063d46300fd146044575b600080fd5b348015604f57600080fd5b506056606c565b6040518082815260200191505060405180910390f35b600080549050905600a165627a7a72305820c52125523008034b3491540aa03fc856951b8da206b011ac05a0c6b52f61b3c00029"
    },
    "root": null,
    "status": 1,
    "logs": null,
    "transactionHash": "0x24d06cf16cd9aad66a144ad2b1b2e475d936656027cd70eae792459926b4a8c1",
    "contractAddress": "0x0e50cea0402d2a396b0db1c5d08155bd219cc52e",
    "out": "0x"
}
```


### 4.web3.thk.getStats（获取链信息）

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| currentheight | bigint | true | 当前块高 |
| txcount | int | true | 总交易数 |
| tps | int | true | 每秒交易数 |
| tpsLastEpoch | int | true | 上一时期交易数 |
| lives | int | true | 链的已存活时间 |
| accountcount | int | true | 账户数 |
| epochlength | int | true | 当前时期包含多少块 |
| epochduration | int | true | 当前时期运行时间 |
| lastepochduration | int | true | 上一时期的运行时间 |
| currentcomm | array | true | 当前这条链的委员会成员 |

示例:
```python
response = web3.thk.getStats('2')
```

返回：

```json
{
    "currentheight": 5290,
    "txcount": 5295,
    "tps": 0,
    "tpsLastEpoch": 0,
    "lives": 10714,
    "accountcount": 6,
    "epochlength": 80,
    "epochduration": 162,
    "lastepochduration": 162,
    "currentcomm": [
        "0x96dc94580e0eadd78691807f6eac9759b9964daa8b46da4378902b040e0eb102cb48413308d2131e9e5557321f30ba9287794f689854e6d2e63928a082e79286",
        "0x4ce2edd98452036c804f3f2eeef157672be2ccf647369eb42eb49ab9f428821f9990efde3cf7f16e4c64616c10b673077f4278c6dd2fc6021da8ad0085a522a2"
    ]
}
```


### 5.web3.thk.getTransactions（获取指定账户在对应链上一定高度范围内的交易信息）

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| address | string | true | 账户地址 |
| chainId | string | true | 链id |
| startHeight | string | true | 查询的起始块高 |
| endHeight | string | true | 查询的截止块高 |

响应参数 交易的array:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainid | string | true | 链id |
| height | int | true | 查询的起始块高 |
| from | string | true | 交易发起账户地址 |
| to | string | true | 交易接受账户地址 |
| nonce | int | true | 交易的发起者在之前进行过的交易数量 |
| value | int | true | 转账金额 |
| timestamp | int | true | 交易的时间戳 |

示例：


```python
response = web3.thk.getTransactions('2', '50', '70');
```

返回：

```json
[
      {
          "chainid": 2,
          "height": 118,
          "from": "0x0000000000000000000000000000000000000000",
          "to": null,
          "nonce": 0,
          "value": 0,
          "timestamp": 1547708801
      },
      {
          "chainid": 2,
          "height": 3831,
          "from": "0x0000000000000000000000000000000000000000",
          "to": null,
          "nonce": 1,
          "value": 0,
          "timestamp": 1547716293
      }
]
```


### 6.web3.thk.callTransaction（查询交易）

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| transaction | dict | true | 交易对象 |

transaction:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |
| fromChainId | string | true | from地址的链id |
| toChainId | string | true | to地址的链id |
| from | string | true | 交易发起账户地址 |
| to | string | true | 交易接受账户地址 |
| nonce | string | true | 交易的发起者在之前进行过的交易数量 |
| value | string | true | 转账金额 |
| input | string | true | 调用合约时的参数 |
| sig | string | true | 交易签名 |
| pub | string | true | 公钥 |


响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| Transaction | dict | true | 交易详情 |
| root | string | true | 交易hash |
| status | int | true | 交易状态: 1:成功, 0:失败 |
| logs | array[dict] | false | 这个交易产生的日志对象数组 |
| transactionHash | string | true | 交易hash |
| contractAddress | string | true | 合约账户地址 |
| out | string | true | 调用返回结果数据 |

Transaction:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainID | string | true | 链id |
| from | string | true | 交易发起账户地址 |
| to | string | true | 交易接受账户地址 |
| nonce | string | true | 交易的发起者在之前进行过的交易数量 |
| val | string | true | 转账金额 |
| input | string | true | 调用合约时的参数 |

示例:

```python
response = web3.thk.callTransaction('2', '0x0000000000000000000000000000000000000000', '0x0e50cea0402d2a396b0db1c5d08155bd219cc52e','22','0', '0xe98b7f4d0000000000000000000000000000000000000000000000000000000000000001');
```

返回：

```json
{
    "Transaction": {
        "chainID": 2,
        "from": "0x0000000000000000000000000000000000000000",
        "to": "0x0e50cea0402d2a396b0db1c5d08155bd219cc52e",
        "nonce": 2,
        "value": 0,
        "input": "0xe98b7f4d0000000000000000000000000000000000000000000000000000000000000001"
    },
    "root": null,
    "status": 0,
    "logs": null,
    "transactionHash": "0x9936cab441360985fc9e27904f0767c1c39fe8e0edb83709a0cdad52470a4592",
    "contractAddress": "0x0000000000000000000000000000000000000000",
    "out": "0x"
}
```



### 7.web3.thk.getBlockHeader（获取指定块高信息）

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |
| height | string | true | 查询块的块高 |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| hash | string | true | 此块的hash |
| previoushash | string | true | 父块的hash |
| chainid | int | true | 链id |
| height | int | true | 查询块的块高 |
| mergeroot | string | true | 合并其他链转账数据hash |
| deltaroot | string | true | 跨链转账数据hash |
| stateroot | string | true | 状态hash |
| txcount | int | true | 交易总数 |
| timestamp | int | true | 时间戳 |

请求示例:

```python
response = web3.thk.getBlockHeader('2', '30');
```

返回：

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



### 8.web3.thk.getBlockTxs（获取块内交易）

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |
| height | string | true | 查询块的块高 |
| page | string | true | 页码 |
| size | string | true | 页的大小 |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| elections | dict | true | 交易详情 |
| accountchanges | array | true | 交易信息 |

accountchanges:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainid | string | true | 链id |
| height | int | true | 查询的起始块高 |
| from | string | true | 交易发起账户地址 |
| to | string | true | 交易接受账户地址 |
| nonce | int | true | 交易的发起者在之前进行过的交易数量 |
| value | int | true | 转账金额 |
| timestamp | int | true | 交易的时间戳 |

请求示例:
```javascript
response = web3.thk.getBlockTxs('2', '30','1','10');
```

返回：

```javascript
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


### 9.web3.thk.compileContract（编译合约）
请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| chainId | string | true | 链id |
| contract | string | true | 合约代码 |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| code | string | true | 编译后的合约代码 |
| info | dict | true | 合约信息 |

请求示例:

```python
response = web3.thk.compileContract('2', 'pragma solidity >= 0.4.0;contract test {uint storedData; function set(uint x) public { storedData = x;} function get() public view returns (uint) { return storedData;}}');
```

返回：

```javascript
{
    "test": {
        "code": "0x608060405234801561001057600080fd5b5060be8061001f6000396000f3fe6080604052348015600f57600080fd5b5060043610604e577c0100000000000000000000000000000000000000000000000000000000600035046360fe47b1811460535780636d4ce63c14606f575b600080fd5b606d60048036036020811015606757600080fd5b50356087565b005b6075608c565b60408051918252519081900360200190f35b600055565b6000549056fea165627a7a723058205f13a3c1870823036833f92b7ac23a38f4bb1d9b737c36f0ea70ded514af2a6c0029",
        "info": {
            "source": "pragma solidity >= 0.4.0;contract test {uint storedData; function set(uint x) public { storedData = x;} function get() public view returns (uint) { return storedData;}}",
            "language": "Solidity",
            "languageVersion": "0.5.2",
            "compilerVersion": "0.5.2",
            "compilerOptions": "--combined-json bin,abi,userdoc,devdoc,metadata --optimize",
            "abiDefinition": [
                {
                    "constant": false,
                    "inputs": [
                        {
                            "name": "x",
                            "type": "uint256"
                        }
                    ],
                    "name": "set",
                    "outputs": [],
                    "payable": false,
                    "stateMutability": "nonpayable",
                    "type": "function"
                },
                {
                    "constant": true,
                    "inputs": [],
                    "name": "get",
                    "outputs": [
                        {
                            "name": "",
                            "type": "uint256"
                        }
                    ],
                    "payable": false,
                    "stateMutability": "view",
                    "type": "function"
                }
            ],
            "userDoc": {
                "methods": {}
            },
            "developerDoc": {
                "methods": {}
            },
            "metadata": "{\"compiler\":{\"version\":\"0.5.2+commit.1df8f40c\"},\"language\":\"Solidity\",\"output\":{\"abi\":[{\"constant\":false,\"inputs\":[{\"name\":\"x\",\"type\":\"uint256\"}],\"name\":\"set\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"get\",\"outputs\":[{\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}],\"devdoc\":{\"methods\":{}},\"userdoc\":{\"methods\":{}}},\"settings\":{\"compilationTarget\":{\"<stdin>\":\"test\"},\"evmVersion\":\"byzantium\",\"libraries\":{},\"optimizer\":{\"enabled\":true,\"runs\":200},\"remappings\":[]},\"sources\":{\"<stdin>\":{\"keccak256\":\"0xa906fc7673818a545ec91bd707cee4d4549c5bf8ae684ddfcee70b0417fd07df\",\"urls\":[\"bzzr://fa15822d315f9ccb19e8e3f658c14e843995e90d9ba8a142ed144edd26eb017b\"]}},\"version\":1}"
        }
    }
}
```

### 10.web3.thk.saveContract （保存合约）
注: 合约hash地址获取->(编译合约(CompileContract) -> 发布合约(SendTx)-> 获取合约地址(GetTxByHash) )

请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| contractaddr | string | true | 合约地址 |
| contract | dict | true | 合约代码 |

响应参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| msg | string | true | 保存结果 |

请求示例:

```python
response = web3.thk.saveContract(contractaddr, contract)
```

返回：

```json
{
    "msg": "save success"
}
```
### 11.web3.thk.getContract （获取已保存的合约）
请求参数:

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| contractaddr | string | true | 合约地址 |

响应参数: 

| **参数名称** | **参数类型** | **是否必须** | **含义** |
| :---: | :---: | :---: | :---: |
| contract | dict | true | 合约代码 |

请求示例:

```python
response = web3.thk.GetContract(contractaddr)
```

返回：

```json
{{
    "test": {
        "code": "0x608060405234801561001057600080fd5b5060be8061001f6000396000f3fe6080604052348015600f57600080fd5b5060043610604e577c0100000000000000000000000000000000000000000000000000000000600035046360fe47b1811460535780636d4ce63c14606f575b600080fd5b606d60048036036020811015606757600080fd5b50356087565b005b6075608c565b60408051918252519081900360200190f35b600055565b6000549056fea165627a7a723058205f13a3c1870823036833f92b7ac23a38f4bb1d9b737c36f0ea70ded514af2a6c0029",
        "info": {
            "source": "pragma solidity >= 0.4.0;contract test {uint storedData; function set(uint x) public { storedData = x;} function get() public view returns (uint) { return storedData;}}",
            "language": "Solidity",
            "languageVersion": "0.5.2",
            "compilerVersion": "0.5.2",
            "compilerOptions": "--combined-json bin,abi,userdoc,devdoc,metadata --optimize",
            "abiDefinition": [
                {
                    "constant": false,
                    "inputs": [
                        {
                            "name": "x",
                            "type": "uint256"
                        }
                    ],
                    "name": "set",
                    "outputs": [],
                    "payable": false,
                    "stateMutability": "nonpayable",
                    "type": "function"
                },
                {
                    "constant": true,
                    "inputs": [],
                    "name": "get",
                    "outputs": [
                        {
                            "name": "",
                            "type": "uint256"
                        }
                    ],
                    "payable": false,
                    "stateMutability": "view",
                    "type": "function"
                }
            ],
            "userDoc": {
                "methods": {}
            },
            "developerDoc": {
                "methods": {}
            },
            "metadata": "{\"compiler\":{\"version\":\"0.5.2+commit.1df8f40c\"},\"language\":\"Solidity\",\"output\":{\"abi\":[{\"constant\":false,\"inputs\":[{\"name\":\"x\",\"type\":\"uint256\"}],\"name\":\"set\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"get\",\"outputs\":[{\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}],\"devdoc\":{\"methods\":{}},\"userdoc\":{\"methods\":{}}},\"settings\":{\"compilationTarget\":{\"<stdin>\":\"test\"},\"evmVersion\":\"byzantium\",\"libraries\":{},\"optimizer\":{\"enabled\":true,\"runs\":200},\"remappings\":[]},\"sources\":{\"<stdin>\":{\"keccak256\":\"0xa906fc7673818a545ec91bd707cee4d4549c5bf8ae684ddfcee70b0417fd07df\",\"urls\":[\"bzzr://fa15822d315f9ccb19e8e3f658c14e843995e90d9ba8a142ed144edd26eb017b\"]}},\"version\":1}"
        }
    }
}
```