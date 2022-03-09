# Thinkium链web3开发教程-JAVA

### 目标

 	1. 了解链的基础操作过程
 	2. 了解合约，并在链上做合约相关操作
 	3. 使用java与链交互

注:本文测试rpc为https://test1.thinkiumrpc.net,还拥有其他可用test rpc依次为https://test2.thinkiumrpc.net、https://test103.thinkiumrpc.net ,读者可自行选择,本文源码克隆地址为https://github.com/ThinkiumGroup/web3j-demo.git

#### 基础环境和工具

1. 基础java环境，这里不做赘述(java、maven)
2. 基础git环境
3. 基础web3j编译环境(自行上网安装)
4. 基础solc编译环境(自行上网安装)
5. [区块链浏览器](https://browser.thinkiumdev.net/)
6. [水龙头网址](https://www.thinkiumdev.net/DApp%20Development/Faucet.html)

#### 项目搭建

1. 使用git克隆基础项目
2. 配置maven
3. 初始化项目
4. 项目结构分析
   1. com.web3j是项目核心
      1. demo为整个测试demo
      2. generate为使用web3j编译出来的java源文件
      3. util里面配置了Rpc环境
   2. resources里面bin和abi使用solc编译sol出来的文件

#### 测试准备

1. 首先准备一个Solidity源代码文件,本次测试文件--项目resource中的Storage.sol

2. 之后对该文件使用solc进行编译,生成数据(参考命令:sudo solc --bin  --abi Storage.sol),结果如下
   1. ```shell
      sudo solc --bin  --abi Storage.sol
      ======= Storage.sol:Storage =======
      Binary:
      608060405234801561001057600080fd5b50610150806100206000396000f3fe608060405234801561001057600080fd5b50600436106100365760003560e01c80632e64cec11461003b5780636057361d14610059575b600080fd5b610043610075565b60405161005091906100a1565b60405180910390f35b610073600480360381019061006e91906100ed565b61007e565b005b60008054905090565b8060008190555050565b6000819050919050565b61009b81610088565b82525050565b60006020820190506100b66000830184610092565b92915050565b600080fd5b6100ca81610088565b81146100d557600080fd5b50565b6000813590506100e7816100c1565b92915050565b600060208284031215610103576101026100bc565b5b6000610111848285016100d8565b9150509291505056fea26469706673582212205fd13809e018341b527cfb6c32c0bb14338d2116c0487c547c4faea7f246e16464736f6c63430008090033
      Contract JSON ABI
      [{"inputs":[],"name":"retrieve","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"num","type":"uint256"}],"name":"store","outputs":[],"stateMutability":"nonpayable","type":"function"}]
      ```
   
      
   
   2. Binary下面的是合约的bin也就是bytecode,ABI 后面就是合约编译出来的abi
   
3. 得到结果之后将bytecode存放到一个文件中,abi也存放到另一个文件(注意要是空文件),例如项目中的Storage.bin以及Storage.abi

4. 使用web3j对文件进行编译生成java类文件,命令如下

   1. ```shell
      web3j  generate solidity  -b 包含bin的文件位置  -a 包含abi的文件位置  -o 生成java文件到位置 -p 生成java文件方哪个包
      例如:web3j  generate solidity   -b /Users/muzi/Desktop/StoBin.bin -a /Users/muzi/Desktop/Storage.abi -o /Users/muzi/IdeaProjects/web3j-sample/main/java -p com.muzi.muxin.wrapper
      ```

      

5. 之后将java文件移动到项目的generate里面

6. 至此准备工作完成,接下来开始测试

#### 测试流程

1. 先确定util包里面的Rpc地址
2. 项目中包含了
   1. 钱包创建
      1. ```java
         public static void createWallet(String password) throws InvalidAlgorithmParameterException, NoSuchAlgorithmException, NoSuchProviderException, CipherException, JsonProcessingException {
                 WalletFile walletFile;
                 ECKeyPair ecKeyPair = Keys.createEcKeyPair();
                 walletFile = Wallet.createStandard(password, ecKeyPair);
                 System.out.println("address " + walletFile.getAddress());
                 ObjectMapper objectMapper = ObjectMapperFactory.getObjectMapper();
                 String jsonStr = objectMapper.writeValueAsString(walletFile);
                 decryptWallet(jsonStr, password);
             }
         ```
      
         
      
      2. ```java
         address 0x4Da8e87539E3c988D6e2431510C8fccBcaf83855
         privateKey 95dda4681ed7b9e0eab5e21c07e495a3fdc9975e51e0a8ee7344c55cb45782cf
         ```
      
         
      
      3. 地址创建之后去[水龙头网址](https://www.thinkiumdev.net/DApp%20Development/Faucet.html)领取测试tkm
   2. 转账功能(主币tkm转账)
      1. ```java
         public static void sendIP1559Transaction() throws Exception {
                 //转账到哪个钱包账户地址
                 String toAddress = "0xAc1656e2c41711053c6Ad151bcF4A395A24C1Ee1";
         
                 //获取证书
                 Credentials credentials = getCredentials("", "", privateKey);
         
                 /**
                  * 创建并发送转账交易
                  */
                 TransactionReceipt transactionReceipt = Transfer.sendFundsEIP1559(
                         web3j,
                         credentials,
                         toAddress, //toAddress
                         BigDecimal.ONE.valueOf(1), //转账值
                         Convert.Unit.ETHER, //转账单位
                         BigInteger.valueOf(300000), // gasLimit
                         DefaultGasProvider.GAS_LIMIT, //maxPriorityFeePerGas (max fee per gas transaction willing to give to miners)
                         BigInteger.valueOf(3_100_000_000L) //maxFeePerGas (max fee transaction willing to pay)
                 ).send();
                 System.out.println("get transaction status-------------------->>>:"+transactionReceipt.isStatusOK());
             }
         ```
      
         
      
      2. 结果展示:
         1. ```java
            get transaction status-------------------->>>:true
            ```
         
         2. true 就是成功  false 就是失败
   3. 发布合约
      1. 
      
         ```java
          public static void deployContract() throws Exception {
                 /**
                  * 创建证书
                  */
                 Credentials credentials = getCredentials("", "", privateKey);
                 /**
                  * 获取链id
                  */
                 String chainId = getChainId();
                 /**
                  * 构建交易管理器
                  */
                 TransactionManager transactionManager = new RawTransactionManager(
                         web3j, credentials, Long.parseLong(chainId));
         
                 /**
                  * 默认的gasPrice和gasLimit
                  */
                 ContractGasProvider contractGasProvider = new DefaultGasProvider();
                 /**
                  * 部署合约  注意：Storage类是根据sol编译后生成的abi文件以及bin文件，利用Command Line Tools生成的合约java类
                  */
         
                 Storage contract = Storage.deploy(web3j, transactionManager, contractGasProvider).send();
                 System.out.println("contractAddress------->>>>>:" + contract.getContractAddress());
         
             }
         
         ```
      
      2. 得到合约地址,后续操作需要使用合约地址,测试执行,得到合约地址为0xca7630e0df9b5cfed112da7587c3aa8177690641，结果如下
         1. ```java
            contractAddress------->>>>>:0xca7630e0df9b5cfed112da7587c3aa8177690641
            ```
         
      
   4. 合约调用
      1. ```java
         public static void loadContract(String contractAddress) throws Exception {
                 /**
                  * 创建证书
                  */
                 Credentials credentials = getCredentials("", "", privateKey);
                 String chainId = getChainId();
                 /**
                  * 构建交易管理器
                  */
                 TransactionManager transactionManager = new RawTransactionManager(
                         web3j, credentials, Long.parseLong(chainId));
                 /**
                  * 默认的gasPrice和gasLimit
                  */
                 ContractGasProvider contractGasProvider = new DefaultGasProvider();
                 //加载合约
                 Storage contract = Storage.load(contractAddress, web3j, transactionManager, contractGasProvider);
                 if (contract.isValid()) {
                     /**
                      * 获取合约设置的默认变量值
                      */
                     BigInteger retrive = contract.retrieve().send();
                     System.out.println("retrieve------->>>>>:" + retrive.toString());
                     /**
                      * 修改合约设置的默认变量值
                      * 注意：此方法调用之后会有一段时间延迟，不能立刻获取到刚设置的值
                      */
                     TransactionReceipt send = contract.store(new BigInteger("999")).send();
         
                     Thread.sleep(8000);
         
                     BigInteger retrivetSecond = contract.retrieve().send();
                     System.out.println("retrieve second------->>>>>:" + retrivetSecond.toString());
                 }
             }
         ```
      
         
      
      2. 上述方法中我们执行了2种方法,分别是读和写
         1. loadContract之后就可以对合约进行一些操作了
         2. 获取合约设置的默认变量值 contract.retrieve()这个是读方法
         3. contract.store重新设置了合约里面的这个值,是写方法
         4. 注:中间睡眠时间因为交易要被确认,写之后直接读取是不行的
         5. 测试结果如下
            1. ```java
               retrieve------->>>>>:1000
               retrieve second------->>>>>:999
               ```
            
               
            
            2. 可以清楚的看到默认值是1000,写入之后查询是999



#### 总结

​	以上就是简单的使用java与thinkium链进行交互的流程和方式,希望你编程愉快!

​		

​								