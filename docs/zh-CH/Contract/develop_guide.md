# DAPP 开发指南

## 智能合约快速入门
请参考[Hello World](./helloworld.md) 

## 智能合约调用时参数说明
调用智能合约时，传入的参数格式必须是 `Hex` ,合约内部通过全局变量 `contract` 获取此传入的参数

### 参数转换规则

**数字**: 先将原数字转化为16进制数字，如果该数字长度为单数则在前面补一个零,然后每两位为单位倒序，最后在后面补相应个数的零。如，10000转为16位编码，具体过程为 1000 => 3e8 => 03e8 => e803 => e803000000000000；解码规则反之；

**字符**: 先获取每一位字符的ASCII码，再将ASCII码转换成16进制；解码规则反之。

如地址 `wKfqqyRzJaMHv1zw77Yb4RFErfZhaVL8cn` => `774b66717179527a4a614d4876317a77373759623452464572665a6861564c38636e`

## 调试智能合约
* 要查看出错信息，修改WaykiChain.conf文件，打开vm调试开关，在原有debug设定上添加debug=vm。  (debug=INFO  --> debug=INFO debug=vm)
* 排错主要靠LogPrint，assert，error打印出错信息到日志。

* 打开main或者testnet目录下的vm.log:

Example
```
2016-10-12 02:12:49 [txmempool.cpp:125]vm: tx hash=69a787e4c7a35f40eff5f696c9f361972a9cb8a7e1e4263e527e689b9bd23e2c CheckTxInMemPool run contract

2016-10-12 02:12:49 [vm/vmrunevn.cpp:125]vm: tx hash:69a787e4c7a35f40eff5f696c9f361972a9cb8a7e1e4263e527e689b9bd23e2c fees=100000 fuelrate=100 maxstep:90000

2016-10-12 02:12:49 [vm/vmrunevn.cpp:93]vm: CVmScriptRun::intial() LUA

2016-10-12 02:12:49 [vm/vmlua.cpp:252]vm: pVmScriptRun=0x7fa006ffb480

2016-10-12 02:12:49 [vm/vmlua.cpp:259]vm: luaL_loadbuffer fail:[string "line"]:375: Account balance is 0.

2016-10-12 02:12:49 [vm/vmlua.cpp:262]vm: run step=-1

2016-10-12 02:12:49 [vm/vmrunevn.cpp:136]vm: CVmScriptRun::run() LUA
```
* 其中luaL_loadbuffer fail:[string "line"]:375: Account balance is 0.提示了脚本375行账户余额为0，由此可知合约执行失败的原因

* 如果在开发过程中，边写代码边调试，可能写的代码有语法错误，在执行时也会出错，通过上面的方法可以查看出错误行。将错误修正后，要重新注册该脚本，再进行调试。

## 注意事项
 *  由于安全性考虑，智能合约中只能使用lua中table，math，string库，不能使用io操作等。
 *  智能合约只能写在一个文件中，不支持多文件。
 *  智能合约文件大小不能超过64k
 *  智能合约内部可以通过默认全局变量`contract`获取合约外部调用合约的参数，类型为`table`
 *  外部调用智能合约的参数长度不能超过4096字节。
 *  智能合约中如果有异常，或逻辑不对，可以通过assert，error退出执行，链上Lua虚拟机器会捕获到该异常。
 *  智能合约必须以mylib = require "mylib"开头，通过mylib调用里面的API，注意一定要放在第一行，第一行如果留空会报异常。
