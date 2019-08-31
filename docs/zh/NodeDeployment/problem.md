
### 一. 建议操作流程
#### 1.启动节点

**根据不同节点环境启动维基链节点**

#### 2.操作节点

(1) 常规操作

| 执行命令                           |   命令说明   |
|:---------------------------------- |:------------:|
| ./coind -datadir=cur help          | 查询帮助信息 |
| ./coind -datadir=cur getinfo       | 查询节点信息 |
| ./coind -datadir=cur listaddr      | 查询地址列表 |
| ./coind -datadir=cur getnewaddress |   新建地址   |

(2）向新建地址中转入测试币
根据[用户自动获取测试币水龙头工具](../DeveloperHelper/faucet.md)操作转入测试币

**Note: 维基链的每个账号，激活之前，可以收到维基币，激活之后，才可以转出维基币。**

(3) 激活账号
详细参考节点[RPC-JSON API文档](../JsonRpcApi/account.md)中的`registaccounttx/registeraccounttx`方法进行激活

(4) 过一分钟后，参照[listaddr](../JsonRpcApi/account.md#listaddr) 查看是否激活成功，

**Note: 如果regid为空字符串，则没有激活成功，否则 如果有值 如 “18247-1”，则激活成功。**

(5) 激活成功后，使用[sendtoaddress](../JsonRpcApi/tx.md#sendtoaddress) 命令 执行向其它地址转账操作。

(6) 使用 [listaddr](../JsonRpcApi/account.md#listaddr)命令查看余额是否减少。

(7) 使用[getaccountinfo](../JsonRpcApi/account.md#getaccountinfo) 查看转账地址的目的地地址余额。

### 二. Linux节点的各文件分析：

| 文件/文件夹     |                                 说明                                 |
|:--------------- |:--------------------------------------------------------------------:|
| coind           |                              可执行文件                              |
| WaykiChain.conf |                               配置文件                               |
| main            |                           正式链数据文件夹                           |
| testnet         |                           测试链数据文件夹                           |
| peer.dat        |                   位于数据文件夹内， 节点连接信息                    |
| wallet.dat      | 位于数据文件夹内，钱包数据，**该文件存放了钱包公私钥，一定要保管好** |
| blocks          |                位于数据文件夹内，同步的区块数据文件夹                |
| syncdata        |                 位于数据文件夹内，同步情况数据文件夹                 |
| database        |  位于数据文件夹内，节点程序启动时才会有，节点程序停止后自动删除掉。  |

### 三. 不同命令行启动方式的对比说明

| running command            | meaning                               |
|:-------------------------- |:------------------------------------- |
| ./coind -datadir=cur       | 配置文件在当前路径                    |
| ./coind -datadir=.         | 配置文件在当前路径                    |
| ./coind -datadir=/opt/wicc | 配置文件在路径: /opt/wicc             |
| ./coind                    | 配置文件在当前用户路径：~/.WaykiChain |

**注意**

```
./coind -datadir=cur      //正确的命令
./coind -datadir =cur     //错误的命令
```

#### 作业
* 使用Linux服务器部署测试链维基链节点，并同步到最新区块高度， 检查方式：仅对作业培训师的ip地址开放白名单，供作业检查者掉json rpc方法的getinfo方法检查;
* 完成测试链节点操作流程， 检查方式： 向培训师的账号地址发送1.2个测试链维基币;
* 奖励：完成以上两个作业奖励 1000个WTIMES.
