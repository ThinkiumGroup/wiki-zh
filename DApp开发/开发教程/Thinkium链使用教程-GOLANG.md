# Thinkium链web3开发教程-GOLANG

### 目标

 	1. 了解链的基础操作过程
 	2. 了解合约，并在链上做合约相关操作
 	3. 使用golang与链交互

注:本文测试rpc为https://test1.thinkiumrpc.net,还拥有其他可用test rpc依次为https://test2.thinkiumrpc.net、https://test103.thinkiumrpc.net ,读者可自行选择，本文源码克隆地址为https://github.com/ThinkiumGroup/web3go-demo.git

#### 基础环境和工具

1. 基础golang环境，这里不做赘述
1. 基础git环境
3. 基础abigen编译环境
4. 基础solc编译环境(自行上网安装)
5. [区块链浏览器](https://browser.thinkiumdev.net/)
6. [水龙头网址](https://www.thinkiumdev.net/DApp%20Development/Faucet.html)

#### 项目搭建

1. 使用git克隆基础项目

2. 配置abigen

   1. ```shell
      go get -u github.com/ethereum/go-ethereum
      cd $GOPATH/src/github.com/ethereum/go-ethereum/
      make
      make devtools
      ```

   2. 注:需提前安装protoc,在make devtools完成后生成的工具在 $GOPATH/bin/abigen，将路径引入系统变量PATH后测试命令

3. 初始化项目

4. 项目结构分析
   1. eth里面的eth的常量里面RPC_URL为RPC地址
   2. examples是例子
      1. account包含创建钱包和主币转账测试方法
      2. chain包含查询当前块高测试方法
      3. contract包
         1. 包含发布和调用合约测试方法
         2. storage.go是生成的文件(详情可往下查看)
   3. lib为使用bin 和 abi 生成的 go文件
   4. resources里面storage中的bin和abi使用solc编译sol出来的文件,为该次测试源文件

#### 测试准备

1. 首先准备一个Solidity源代码文件,本次测试文件--项目resource中的Storage中的Storage.sol

2. 之后对该文件使用solc进行编译,生成数据(参考命令:sudo solc --bin  --abi Storage.sol),结果如下
   1. ```shell
       solc --bin  --abi Storage.sol
      ======= Storage.sol:Storage =======
      Binary:
      608060405234801561001057600080fd5b50610150806100206000396000f3fe608060405234801561001057600080fd5b50600436106100365760003560e01c80632e64cec11461003b5780636057361d14610059575b600080fd5b610043610075565b60405161005091906100a1565b60405180910390f35b610073600480360381019061006e91906100ed565b61007e565b005b60008054905090565b8060008190555050565b6000819050919050565b61009b81610088565b82525050565b60006020820190506100b66000830184610092565b92915050565b600080fd5b6100ca81610088565b81146100d557600080fd5b50565b6000813590506100e7816100c1565b92915050565b600060208284031215610103576101026100bc565b5b6000610111848285016100d8565b9150509291505056fea26469706673582212205fd13809e018341b527cfb6c32c0bb14338d2116c0487c547c4faea7f246e16464736f6c63430008090033
      Contract JSON ABI
      [{"inputs":[],"name":"retrieve","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"num","type":"uint256"}],"name":"store","outputs":[],"stateMutability":"nonpayable","type":"function"}]
      ```
   
      
   
   2. Binary下面的是合约的bin也就是bytecode,ABI 后面就是合约编译出来的abi
   
3. 得到结果之后将bytecode存放到一个文件中,abi也存放到另一个文件(注意要是空文件),例如项目中的Storage.bin以及Storage.abi

4. 使用abigen对文件进行编译生成go类文件,命令如下

   1. ```hell
      sudo abigen --abi=Storage.abi --bin=Storage.bin --pkg=main --out=storage.go
      ```
      

6. 至此准备工作完成,接下来开始测试

#### 测试流程

1. 先确定调用包里面的Rpc地址

2. 项目中包含了
   1. 钱包创建
      1. ```go
         func TestAccount(t *testing.T) {
         	//get account
         	pv, err := crypto.GenerateKey()
         	if err != nil {
         		panic(err)
         	}
         	privateKey := hex.EncodeToString(crypto.FromECDSA(pv))
         	//c1f14e4132c1858b390ff169dd045082b9ba1ca022de641e7aa24b0322510499
         	fmt.Println("privateKey: ", privateKey)
         	// change to your rpc provider
         	web3, err := web3.NewWeb3(eth.RPC_URL)
         	if err != nil {
         		panic(err)
         	}
         	err = web3.Eth.SetAccount(privateKey)
         	if err != nil {
         		panic(err)
         	}
         	//get address
         	//0x901F21a0a09536F24feF8f5565eBcacB7aC5DE31
         	fmt.Println("address: ", web3.Eth.Address())
         }
         ```
         
         
         
      2. 结果如下
      
         1. ```go
            API server listening at: [::]:41549
            === RUN   TestAccount
            privateKey:  50014d49478a00f62f775e015e389bd73dede68726a14d4f8b28253165bff23a
            address:  0x2aE433ae1C9e34BdD48296fB7BCFE8a6E60c7d38
            ```
      
      3. 地址创建之后去[水龙头网址](https://www.thinkiumdev.net/DApp%20Development/Faucet.html)领取测试tkm
      
   2. 转账功能(主币tkm转账)
      1. ```go
         func TestSendTransaction(t *testing.T) {
         	// change to your rpc provider
         	web3, err := web3.NewWeb3(eth.RPC_URL)
         	if err != nil {
         		panic(err)
         	}
         	web3.Eth.SetAccount("c1f14e4132c1858b390ff169dd045082b9ba1ca022de641e7aa24b0322510499")
         	balance, err := web3.Eth.GetBalance(web3.Eth.Address(), nil)
         	if err != nil {
         		panic(err)
         	}
         	fmt.Println("balance: ", balance)
         	nonce, err := web3.Eth.GetNonce(web3.Eth.Address(), nil)
         	if err != nil {
         		panic(err)
         	}
         	fmt.Println("Latest nonce: ", nonce)
         	//transfer
         	//set chainId
         	web3.Eth.SetChainId(50001)
         	// transfer 1 tkm from 0x901F21a0a09536F24feF8f5565eBcacB7aC5DE31 to 0x90021a2CcA84a0611363ec1f9ccb36B4761DE08E
         	hash, err := web3.Eth.SendRawEIP1559Transaction(common.HexToAddress("0x90021a2CcA84a0611363ec1f9ccb36B4761DE08E"),
         		new(big.Int).SetInt64(1000000000000000000),300000,new(big.Int).SetInt64(300000),new(big.Int).SetInt64(300000),nil)
         	if err != nil {
         		panic(err)
         	}
         	fmt.Println("hash: ", hash)
         }
         ```
         
         
         
      2. 结果展示:
         1. ```go
            API server listening at: [::]:38469
            === RUN   TestSendTransaction
            balance:  97315968400000000000
            Latest nonce:  24
            hash:  0x3c781964d927a2194897830cce8d89bdf2e20a9ff3ddd0fa3cda7b3f3f6adc39
            --- PASS: TestSendTransaction (0.05s)
            PASS
            ```
      
   3. 发布合约
      1. 
      
         ```go
          func TestDeployContract(t *testing.T)  {
         	// change to your rpc provider
         	web3, err := web3.NewWeb3(eth.RPC_URL)
         	if err != nil {
         		panic(err)
         	}
         	privateKey, err := crypto.HexToECDSA("c1f14e4132c1858b390ff169dd045082b9ba1ca022de641e7aa24b0322510499")
         	if err != nil {
         		panic(err)
         	}
         
         	publicKey := privateKey.Public()
         	publicKeyECDSA, ok := publicKey.(*ecdsa.PublicKey)
         	if !ok {
         		fmt.Println("error casting public key to ECDSA")
         	}
         
         	fromAddress := crypto.PubkeyToAddress(*publicKeyECDSA)
         	nonce, err := client.PendingNonceAt(context.Background(), fromAddress)
         	if err != nil {
         		panic(err)
         	}
         
         	gasPrice, err := client.SuggestGasPrice(context.Background())
         	if err != nil {
         		panic(err)
         	}
         	//set chainID
         	auth, err  := bind.NewKeyedTransactorWithChainID(privateKey,new(big.Int).SetInt64(50001))
         	if err != nil {
         		panic(err)
         	}
         	auth.Nonce = big.NewInt(int64(nonce))
         	auth.Value = big.NewInt(0)     // in wei
         	auth.GasLimit = uint64(300000) // in units
         	auth.GasPrice = gasPrice
         	address, tx, instance, err :=DeployMain(auth,client)
         	if err != nil {
         		panic(err)
         	}
         	fmt.Println("contract:"+address.Hex())
         	fmt.Println("deploy hash:"+tx.Hash().String())
         	_, _ = instance, tx
         }
         ```
      
      2. 得到合约地址,后续操作需要使用合约地址,测试执行,得到合约地址为0x63C21800Fc7970D796811ac21d5ca42688382195，结果如下
         1. ```go
            API server listening at: [::]:38869
            === RUN   TestDeployContract
            contract:0x63C21800Fc7970D796811ac21d5ca42688382195
            deploy hash:0x59fe057a32e6650ae5134ae2d9a61d884d05f5f687828ea79d146f4fc747f6d8
            --- PASS: TestDeployContract (0.04s)
            PASS
            ```
      
            
      
   4. 合约调用
      1. ```go
         func TestCallContract(t *testing.T) {
         
         	// change to your rpc provider
         	web3, err := web3.NewWeb3(eth.RPC_URL)
         	if err != nil {
         		panic(err)
         	}
         	//load contract 0x771B03a85843907B89B66dbbCD90E1b8761a4fa3
         	main, err := NewMain(common.HexToAddress("0x797C88225547126879aA4EF444A66D627E402366"), client)
         	if err != nil {
         		panic(err)
         	}
         	num, err := main.Retrieve(nil)
         	if err != nil {
         		panic(err)
         	}
         	fmt.Printf("num:%d", num)
         	fmt.Println("")
         
         	//write contract
         	privateKey, err := crypto.HexToECDSA("c1f14e4132c1858b390ff169dd045082b9ba1ca022de641e7aa24b0322510499")
         	if err != nil {
         		panic(err)
         	}
         	publicKey := privateKey.Public()
         	publicKeyECDSA, ok := publicKey.(*ecdsa.PublicKey)
         	if !ok {
         		fmt.Println("error casting public key to ECDSA")
         	}
         
         	fromAddress := crypto.PubkeyToAddress(*publicKeyECDSA)
         	nonce, err := client.PendingNonceAt(context.Background(), fromAddress)
         	if err != nil {
         		panic(err)
         	}
         
         	gasPrice, err := client.SuggestGasPrice(context.Background())
         	if err != nil {
         		panic(err)
         	}
         	chainId,err := client.ChainID(context.Background())
         	//set chainID
         	auth, err := bind.NewKeyedTransactorWithChainID(privateKey, chainId)
         	if err != nil {
         		panic(err)
         	}
         	auth.Nonce = big.NewInt(int64(nonce))
         	auth.Value = big.NewInt(0)     // in wei
         	auth.GasLimit = uint64(300000) // in units
         	auth.GasPrice = gasPrice
         
         	main.Store(auth,new(big.Int).SetInt64(1000))
         
         }
         
         ```
         
         
         
      2. 上述方法中我们执行了2种方法,分别是读和写
         1. NewMain之后就可以对合约进行一些操作了
         2. 获取合约设置的默认变量值main.Retrieve(nil)这个是读方法
         3. main.Store(auth,new(big.Int).SetInt64(10))重新设置了合约里面的这个值,是写方法
         4. 注:中间睡眠时间因为交易要被确认,写之后直接读取是不行的
         5. 测试结果如下
            1. ```go
               API server listening at: [::]:40991
               === RUN   TestCallContract
               num:100
               newNum:1000
               --- PASS: TestCallContract (5.06s)
               PASS
               ```
            
               
            
            2. 可以清楚的看到默认值是100,写入之后查询是1000



#### 总结

​	以上就是简单的使用golang与thinkium链进行交互的流程和方式,希望你编程愉快!

​		

​								