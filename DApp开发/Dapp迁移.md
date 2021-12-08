## 零成本：其它链Dapp迁移至Thinkium链流程



![1638876337897](https://thinkium-wiki.s3.ap-northeast-1.amazonaws.com/migrate/zh-1.png)

**Dapp的运行分为四个部分**

 

1.MetaMask（私钥管理的浏览器插件）

（安装链接：https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn )

2.Dapp前端页面（业务逻辑的具体实现）

3.Dapp后端服务（业务依赖的具体服务）

4.Dapp合约（发布到区块链上的智能合约，内含不可更改的业务逻辑）

 

 

**假设已有Dapp部署在以太坊或币安智能链上，并正常运行。那么如果想要迁移此Dapp至Thinkium链。需要做如下关键4步。**

 

**1.（图中第1步）部署Dapp智能合约至Thinkium链(70001链，70002链或70103链)**

 

**2.（图中第2步，第6步）配置Dapp合约地址至前端页面或后台服务**

如果部署者的地址，在Thinkium链上发布合约时的nonce值，与这个地址在其他链发布合约时的nonce值相同，意味着按照，部署者地址+nonce值的地址算法计算方式，计算出来的发布在Thinkium链上的合约地址与其他链的合约地址相同，那么可忽略配置合约地址到项目里这一步，不需修改

 

 

**3.（图中第7步）配置Thinkium 的rpc和链id至后台服务，用于后续签名发交易至Thinkium链。参照下方rpc表格。**

 

**4.（图中第3步）添加Thinkium 的rpc 至 MetmaMask**

4.1 如果已安装了MetaMask，可忽略。

未安装可通过下方链接，添加chrome插件。注意科学上网。

下载链接 ：https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn

 

4.2 登录打开MetaMask钱包，点击上方网络后，弹出如下图所示，点击添加网络。

![1638876425764](https://thinkium-wiki.s3.ap-northeast-1.amazonaws.com/migrate/zh-2.png)

4.3 添加网络，打开后，如下所示，然后按照下方列表及示例，添加网络。因为Thinkium是多链结构，具体依据您是把合约部署在哪条链上，来决定添加哪些网络。

![1638876449265](https://thinkium-wiki.s3.ap-northeast-1.amazonaws.com/migrate/zh-3.png)

| 网络名称                   | Rpc url                          | 链id  | Symbol | 区块链浏览器                       |
| -------------------------- | -------------------------------- | ----- | ------ | ---------------------------------- |
| Thinkium Mainnet Chain 1   | https://proxy1.thinkiumrpc.net   | 70001 | TKM    | https://chain1.thinkiumscan.net/   |
| Thinkium Mainnet Chain 2   | https://proxy2.thinkiumrpc.net   | 70002 | TKM    | https://chain2.thinkiumscan.net/   |
| Thinkium Mainnet Chain 103 | https://proxy103.thinkiumrpc.net | 70103 | TKM    | https://chain103.thinkiumscan.net/ |

 

 

至此，可以通过在Dapp网页前端访问Thinkium链上的Dapp应用了。

 

 