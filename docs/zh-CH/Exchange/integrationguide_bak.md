# 对接指南

维基链余额模型为类似`ETH`的账户余额模型，比特币的`UTXO`模型

## 交易所

### Linux钱包节点部署

请参考[Docker部署](NodeDeployment/docker.md)或者[本地部署](NodeDeployment/build.md)

#### 钱包分类说明

根据WaykiChain的经验，在实施选择方面有两种方法：一种是更快速和简化的方法，我们称之为热钱包模式，另一种需要强大的安全实施，我们称之为热冷钱包模式。
在任何一种方法中，都需要在最近的Ubuntu或CentOS发行版的现代Linux系统中部署一个WaykiChain钱包节点。

##### 钱包模式1: 热钱包模式
在这种模式中，有一个网络连接钱包管理一组WaykiChain帐户和密钥。必须在此钱包周围采取强有力的安全措施，以防止试图窃取这些帐户的私钥或破坏整体服务的任何未经授权方进行网络攻击。如果不这样做，可能会造成巨大的经济损失。所有交易所拥有的账户或托管账户都可以通过这个单个钱包节点进行管理，该节点提供不同账户之间的账户创建，激活和转账服务。

交易所也可以部署多于一个这样的WaykiChain热钱包节点，以便为了负载平衡或安全原因在钱包中更均匀地分配账户。

##### 钱包模式2: 冷-热钱包模型

与模式1的热钱包模式相比，主要的实现差异在于，新添加的组件称为冷钱包程序，没有任何网络连接。这个管理一组WICC帐户的钱包可以安全地抵御任何网络攻击。
在这种双钱包模型中，热钱包通常管理足够数量的WICC，以允许用户从交易所自由提取币，而冷钱包则反转大量不经常触摸的硬币。在此设置中，整体安全性优于热钱包模式。

当币的总量达到警告水平以下，则确实需要冷钱包将硬币转移到热钱包中，低于该警告水平不能很快满足用户的提币需求。由于冷钱包与网络断开连接，转账的唯一方法是让冷钱包程序生成离线签名交易原始字符串，该字符串可以QR码等形式进一步输出。因此，交易所可以使用QR码扫描器等工具并使用它通过热钱包将原始交易字符串广播至网络。

## 交易所

### 交易所常用API
请参考 [3. JSON RPC API]

**交易所常用API列表**
```
listaddr
getnewaddress
registaccounttx
registaccounttxraw
sendtoaddress
sendtoaddresswithfee
sendtoaddressraw
getaccountinfo
getbalance
dumpprivkey
importprivkey
validateaddress
encryptwallet
walletpassphrase
walletlock
walletpassphrasechange
dumpwallet
importwallet
islocked
getwalletinfo
submittx
gettxdetail
listtx
listunconfirmedtx
getalltxinfo
```
