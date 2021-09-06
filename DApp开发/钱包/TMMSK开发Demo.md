

基于TMMSK钱包开发，以下提供开发Demo

GitHub 地址：[https://github.com/ThinkiumGroup/TMMSK-Demo.git](https://github.com/ThinkiumGroup/TMMSK-Demo.git)



### 查看浏览器是否正在运行 TMMSK插件

```react
function checkTmmsk() {
  if (typeof window !== 'undefined' && typeof window.thinkium !== 'undefined') {
    alert('已安装TMMSK');
  } else {
    alert('未安装TMMSK');
  }
}
```



### 初始化 window.thinkium

TMMSK 会注入 window.thinkium 实例到当前页面上

```react
function init(type = 0) {
  //let currentChainId = parseInt(window.window.thinkium.chainId, 16)
  let thinkium = window.thinkium
  //禁止自动刷新，metamask要求写的
  thinkium.autoRefreshOnNetworkChange = false
  //开始调用metamask
  thinkium.enable().then(function (accounts) {
    if(type == 0) {
      alert('链接成功，当前钱包地址为: ' + accounts[0].toLowerCase())
    }
    //初始化provider
    let provider = window['thinkium'] || window.web3.currentProvider
    //初始化Web3
    window.web3 = new Web3(provider)
    window.defaultAccount = accounts[0].toLowerCase()
  }).catch(function (error) {
    console.log('enable-error', error)
    alert('链接出错')
  })
}
```



### 获取账户地址

```react
async function getAddress() {
  // 授权获取账户
  const accounts = await window.web3.eth.getAccounts();
  const myAccount = accounts[0];
  alert('当前钱包地址是' + myAccount)
}
```



### 同链转账

```react
 async function transfer() {
    let transactionParameters = {
      from: '0x0d7a8575469d627563C8e90ea65408deCa98d38a',  //0x地址
      to: '0xc1ec694522bf577775c6e704f0ad479dba0ad436',    //0x地址
      value: '10000000000000',
      input: ''
    }

    let txHash = await window.thinkium.request({
      method: 'eth_sendTransaction',
      params: [transactionParameters],
    })
    alert('转账交易发送成功，hash为：' + txHash)
  }
```



### 获取余额

```react
async function getBalance() {
    // 授权获取账户
    const accounts = await window.web3.eth.getAccounts();
    const myAccount = accounts[0];
    console.log(myAccount, 1);
    // 返回指定地址账户的余额
    const balance = await window.web3.eth.getBalance(myAccount) 
    // window.thinkium.request({ method: 'eth_getBalance', params: [myAccount, "latest"], "id": 2 });
    alert('当前账户余额是' + new BigNumber(balance).div("1e+18"))
  }
```



### 查看交易

```react
async function checkTransaction() {
  // let txHash = '0x8cfe9b79e326b4def2db02a261626724ae1db0d8f88bdd910cb333a1223e2d1d';
  let txHash = '0x74f92bb0a085e00eb095afe33dbd663635d88a5eff92ff47c385b251077c9a10';
  let result = await window.web3.eth.getTransactionReceipt(txHash); // 成功
  console.log('--result', result)
}
```



### 合约调用

```react
function callContract() {
    let contractAddress = '0x36fde683c46483a2ffd9aeb7b77a511db077c19c';
    let myContract = new window.web3.eth.Contract(HelloWorldAbi, contractAddress);

    myContract.methods.set('111').send({ from: window.defaultAccount })
      .on('transactionHash', function (transactionHash) {
        console.log('transactionHash',transactionHash)
      })
      .on('confirmation', function (confirmationNumber, receipt) {
        console.log('confirmation', { confirmationNumber: confirmationNumber, receipt: receipt })
      })
      .on('receipt', function (receipt) {
        console.log('receipt', { receipt: receipt })
      })
      .on('error', function (error, receipt) {
        console.log('error',{ error: error, receipt: receipt })
      })

     myContract.methods.get().call().then((res) => {
      console.log('---resultGet', res)
    })
  }
```

