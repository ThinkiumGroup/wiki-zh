### Gas费用

Gas 对Thinkium来说至关重要，它是Thinkium运行的基础之一，每一笔发生在Thinkium上的交易均需要支付gas费用。

### 什么是Gas

Gas 是指衡量在Thinkum网络上执行特定操作所需的计算工作量的单位。

由于每笔Thinkium交易都需要计算资源来执行，因此每笔交易都需要收费。Gas 是指在Thinkium上成功进行交易所需的费用。

Gas 费用以Thinkium的本地货币TKM 支付。Gas 价格以 gwei 表示，它本身就是 TKM 的面额 - 每个 gwei 等于 0.000000001 TKM (10 -9 TKM)。例如，与其说你的 gas 花费 0.000000001 TKM，你可以说你的 gas 花费 1 gwei。“gwei”这个词本身的意思是“giga-wei”，它等于1,000,000,000 wei。Wei 本身是 TKM 的最小单位。

### Gas费用计算

普通转账：固定收取0.01 TKM

合约交易：根据合约实际消耗的步骤计算

跨链转账：根据跨链系统合约实际消耗的步骤计算（约0.08 TKM）

### 什么是GAS LIMIT？

Gas 限制是指您愿意在交易中消耗的最大 Gas 量。涉及[智能合约的](https://ethereum.org/en/developers/docs/smart-contracts/)更复杂的交易需要更多的计算工作，因此与简单支付相比，它们需要更高的 gas 限额。

如果您指定的 gas 太少，例如，简单 TKM 转账的 gas 低于0.01 TKM（例如0.005），则会消耗0.005 TKM来尝试运行转账，但它不会完成。然后链会恢复任何更改，但由于矿工已经完成了价值 0.005 TKM 的工作，因此消耗了 gas。

