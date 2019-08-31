# Dapp Development Guide
## Quickstart
Refer to [Hello World][1] 

## Debug smart contract
* add `debug=vm` in `WaykiChain.conf` and `restart` the node
* view vm.log in the directory of `testnet` or `main`
* use function `LogPrint`、`assert`、`error`print error message to log。

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
* the `luaL_loadbuffer fail:[string "line"]:375: Account balance is 0.` prompt that the 375 line in smart contract account balance is 0, which can be seen why the contract execution failed.


## Note
*  Due to security considerations, only the `table`, `math`, and `string` libraries in the smart contract can be used in the contract, **io operations cannot be used**.
*  Smart contract can only be written in one file. Please don’t write the contract in multiple files.
*  The max size of smart contract is 64KB.
*  The calling content is passed to the smart contract via the global variable "contract", which can be used in the smart contract.
*  The account balance detection switch is turned off by default by the global variable "gCheckAccount" in the lua script.
*  The contents of the smart contract call cannot exceed 4096 bytes.
*  If there is an exception in the smart contract, or the logic is incorrect, you can exit the execution by methods of “assert” or “error”, and the lua interpreter will catch the exception.
*  Smart contracts must start with mylib = require "mylib". Call the API inside mylib, and be sure to put it in the first line. The first line will report an exception if left blank.

[1]:	helloworld.md
