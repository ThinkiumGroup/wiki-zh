### EVM虚拟机

Thinkium公链全面兼容以太坊EVM虚拟机，任何使用以太坊EVM编辑的合约等完全可以在Thinkium公链上运行。以下介绍引用自以太坊EVM



### 以太坊虚拟机EVM介绍

#### EVM 说明

EVM 作为具有1024 个项目深度的堆栈机器执行。每个项目都是一个 256 位字，选择它是为了便于使用 256 位密码术（例如 Keccak-256 哈希或 secp256k1 签名）。

在执行期间，EVM 维护一个瞬态*内存*（作为字寻址的字节数组），它不会在事务之间持续存在。

然而，合约确实包含一个 Merkle Patricia*存储*树（作为一个字可寻址的字数组），与相关帐户和全局状态的一部分相关联。

编译智能合同字节码执行作为数字EVM的操作码，其执行诸如标准栈操作`XOR`，`AND`，`ADD`，`SUB`，等。该EVM还实现了许多特定blockchain-栈操作的，如`ADDRESS`，`BALANCE`，`KECCAK256`，`BLOCKHASH`，等。

[![显示 EVM 操作需要 gas 的位置的图表](https://ethereum.org/static/9628ab90bfd02f64cf873446cbdc6c70/302a4/gas.png)

#### 从账本到状态机

“分布式账本”的类比通常用于描述像比特币这样的区块链，它使用基本的密码学工具实现了一种去中心化的货币。加密货币的行为类似于“正常”货币，因为规则规定了可以做什么和不可以做什么来修改分类帐。例如，一个比特币地址不能花费比它之前收到的更多的比特币。这些规则支持比特币和许多其他区块链上的所有交易。

虽然以太坊拥有自己的原生加密货币 (Ether)，它遵循几乎完全相同的直观规则，但它还支持更强大的功能：智能合约。对于这个更复杂的特征，需要一个更复杂的类比。以太坊不是分布式账本，而是分布式状态机。以太坊的状态是一个大数据结构，它不仅包含所有账户和余额，而且是一个*机器状态*，它可以根据一组预定义的规则在块之间变化，并且可以执行任意机器代码。EVM 定义了从块到块改变状态的具体规则。

![显示 EVM 组成的图表](https://ethereum.org/static/e8aca8381c7b3b40c44bf8882d4ab930/302a4/evm.png)

#### 以太坊状态转换函数

EVM 的行为就像一个数学函数：给定一个输入，它产生一个确定性的输出。因此，更正式地将以太坊描述为具有**状态转换功能**是非常有帮助的：

```
1Y(S, T)= S'
2
```

给定一个旧的有效状态`(S)`和一组新的有效交易`(T)`，以太坊状态转换函数`Y(S, T)`产生一个新的有效输出状态`S'`

#### 状态

在以太坊的上下文中，状态是一个巨大的数据结构，称为修改后的 Merkle Patricia Trie，它保持所有帐户通过哈希链接并可以简化为存储在区块链上的单个根哈希。

#### 交易

交易是来自账户的加密签名指令。有两种类型的交易：导致消息调用的交易和导致合约创建的交易。

合约创建会导致创建一个包含编译后的智能合约字节码的新合约账户。每当另一个帐户对该合约进行消息调用时，它就会执行其字节码。

