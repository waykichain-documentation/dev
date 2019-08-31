# 安卓钱包离线签名库开发指南

## 简介
当开发者需要使用安卓原生客户端进行 `生成助记词`、`助记词转私钥`、`创建地址`、`注册地址`、`转账`、`部署合约`、`调用合约`、`投票`等操作时，维基链提供安卓的离线签名库供其使用。

## 源码和库

安卓原生钱包离线签名库基于`Golang`语言离线签名库编译而来，源码:

https://github.com/WaykiChain/wicc-wallet-utils-go

安卓原生钱包离线签名库下载地址:

https://github.com/WaykiChain/wicc-wallet-utils-go/releases/download/v1.1/and-wiccwallet.aar

## 使用方法
### 引入签名库
将`and-wiccwallet.aar`放在对应工程的`libs`目录下

在对应文件中添加`api fileTree(dir: 'libs', include: ['*.jar','*.aar'])` 引入库文件

## 使用说明
 * 正式网钱包地址以字母W开头长度34位，主网网络参数2。
 
 * 测试网钱包地址以字母w开头长度34位，测试网络参数1。
 
 * 正式网钱包私钥以字母P开头长度34位，主网网络参数2。
 
 * 测试网钱包私钥以字母Y开头长度34位，测试网络参数1。

### 生成助记词
默认生成英文助记词，生成包含12个单词
```
  String mnemonics=Wiccwallet.generateMnemonics();
  String[] words=mnemonics.split(" ");
```

### 助记词转正式网私钥
```
  String privateKey=Wiccwallet.getPrivateKeyFromMnemonic(mnemonics,2);
```

### 维基链正式网地址生成
  * 助记词转地址
```
  String address=Wiccwallet.getAddressFromMnemonic(mnemonics,2);
```

### 获得注册地址签名
如果要使用地址，需要花费10000`sawi` fees 进行注册，注册前，先确保你有一定的wicc余额
```   
  //获得钱包注册签名
  RegisterAccountTxParam registerAccountTxParam=new RegisterAccountTxParam();
        registerAccountTxParam.setFees(10000);//矿工费最少0.0001wicc
        registerAccountTxParam.setValidHeight(95762);/区块高度
        try {  
            String signTx=Wiccwallet.
       signRegisterAccountTx(privateKey,registerAccountTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### 获得转账签名
```
      CommonTxParam commonTxParam=new CommonTxParam();
        commonTxParam.setDestAddr("");//收款地址
        commonTxParam.setFees(10000);//矿工费最少0.0001wicc
        commonTxParam.setSrcRegId("");//钱包注册获得的id
        commonTxParam.setValidHeight(95762);//区块高度
        commonTxParam.setValues(100000000);//转账1 wicc
        try {
            String commonTx=Wiccwallet.signCommonTx(privateKey,commonTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
 ```

### 获得部署合约签名
````
     RegisterContractTxParam registerContractTxParam=new RegisterContractTxParam();
        registerContractTxParam.setDescription("Contract description");
        registerContractTxParam.setFees(110000000);//部署合约小费最好设置大一点
        registerContractTxParam.setScript("contract".getBytes());
        registerContractTxParam.setSrcRegId("");//部署合约的钱包的注册id
        registerContractTxParam.setValidHeight(95762);//区块高度

        try {
      String signTx=   Wiccwallet.signRegisterContractTx(privateKey,registerContractTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
````

### 获得调用合约签名
```
     CallContractTxParam  callContractTxParam=new CallContractTxParam();
        callContractTxParam.setAppId("");//智能合约的注册id
        callContractTxParam.setContractHex("f20312312");//调用合约组装字段
        callContractTxParam.setFees(10000);
        callContractTxParam.setSrcRegId("");//你的钱包注册id
        callContractTxParam.setValidHeight(95762);//区块高度,需要随时获取
        callContractTxParam.setValues(100000000);//金额不一定要填，这要看合约是否需要

        try {
            Wiccwallet.signCallContractTx(privateKey,callContractTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### 获得投票签名
```
  DelegateTxParam delegateTxParam=new DelegateTxParam();
        delegateTxParam.setValidHeight(95762);//区块高度,需要随时获取
        delegateTxParam.setSrcRegId("");//你的钱包注册id
        delegateTxParam.setFees(10000);
        OperVoteFunds funds=new OperVoteFunds();
        OperVoteFund fund=new OperVoteFund();
        fund.setVoteValue(10000);//投票数
        fund.setPubKey(hexString2binaryString("03aaee970fec78b646a61b7c61b170ac7e0bfb47faef2b641074ad9f4ed75b31b8"));//获得投票的地址的公钥
 funds.add(fund);
        try {
         String delegateTx=   Wiccwallet.signDelegateTx(privateKey,delegateTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```


