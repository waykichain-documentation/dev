# WaykiChain development FAQ

## The smallest unit is `sawi`
![decimal][image-1]

## Account description

WaykiChain uses an account model like `ETH`, not `UTXO`, only normal accounts and contract accounts. Each account can be represented by the base58 address like `WKfqqyRzJaMHv1zw77Yb4RFErfZhaVL8cn` or regid like `1234-1`.

New accounts do not cost WICC, each account can receive WICC before registration, after the registration is successful, you can start sending transactions with the WICC.

When registering an account, you can register yourself, provided that there is enough balance in the account, because the registration is actually a transaction, and it takes at least 10000`sawi`=0.0001 WICC. After registration, the corresponding `regid` will be generated.

## Consensus mechanism

`DPOS`,11 BP nodes currently

## Configuration file path error

 * The following error is means that the configuration file path is incorrect.
 ```
  error: You must set rpcpassword=<password> in the configuration file:
 /root/.WaykiChain/WaykiChain.conf
 If the file does not exist, create it with owner-readable-only file permissions.
 ```
 * if you run  `./coind -datadir=.` , the `WaykiChain.conf` Should be in the same directory as `coind`
 * if you run  `./coind`  the `WaykiChain.conf` should be in the current user directory under the folder of `.WaykiChain`


## Node startup failed

log
```
root@ubuntu:/home/jk/workspace/wicc/WaykiChain_deply1# ./coind -datadir=.


************************
EXCEPTION: N5boost16exception_detail10clone_implINS0_19error_info_injectorINS_6system12system_errorEEEEE
bind: Address already in use
Coin in AppInit()

Shutdown : done
root@ubuntu:/home/jk/workspace/wicc/WaykiChain_deply1#
```

* Solution: kill the running node process and then start the node you want.

```
root@ubuntu:/home/jk/workspace/wicc/WaykiChain_deply1# ps -ef|grep coind
root      20588   1794  6 Dec25 ?        08:46:04 ./coind
root      29810  29768 93 11:51 pts/0    04:55:05 ./coind -datadir=.
root      30489  30322  0 17:05 pts/25   00:00:00 grep --color=auto coind
root@ubuntu:/home/jk/workspace/wicc/WaykiChain_deply1# kill 29810
root@ubuntu:/home/jk/workspace/wicc/WaykiChain_deply1# ./coind -datadir=.
WaykiChain version v1.1.0.1-075db0b-dirty-release-linux (2018-12-26 15:47:51 +0800)
Using OpenSSL version OpenSSL 1.0.1f 6 Jan 2014
Using Lua version Lua 5.3.1
Using Boost version 1.54
Using Level DB version 1.17
Using Berkeley DB version Berkeley DB 4.8.30: (April  9, 2010)
Startup time: 2018-12-31 09:06:01
Default data directory /root/.WaykiChain
Using data directory /home/jk/workspace/wicc/WaykiChain_deply1/./testnet
Using at most 125 connections (     1024 file descriptors available)
```
## The node has a block download abort

**Solution**
1. **restart**
2. If the block synchronization is often aborted for a long time, you can add a timed execution script to restart the task by using the `crontab` command.


#### Deployment locally

* `/root/wayki/checkblock.sh`The script content is as follows:

```
#!/bin/bash

echo "start check to blockcount:"

date

blockfile="wicc.blocks"
touch $blockfile
past_blockcount=`tail -n 1 $blockfile`
echo  "past blockcount:$past_blockcount"

current_blockcount=`docker exec -it waykicoind-main /opt/wicc/coind  getblockcount`
echo "current blockcount:$current_blockcount"

if [ $current_blockcount = $past_blockcount ]
then
  echo "current blockcount equal to past blockcount, so restart the waykicoind."
  docker restart waykicoind-main
else
  echo "current blockcount does not equal to past blockcount, so go ahead."
  past_blockcount=$current_blockcount
fi

echo $current_blockcount>$blockfile
echo "Now the past blockcount is $past_blockcount "

echo "end！"

```

* use `crontab -e` add task

**example**

Detect it once per hour
```
0 */1 * * *   /root/wayki/checkblock.sh >> /root/wayki/checkblock.log

```


#### Docker local deployment

* `/root/wayki/auto_restart.sh`The script content is as follows:

```shell
#!/bin/bash

echo "native start check to blockcount:"
date

echo $1
cd $1

blockfile="wicc.blocks"
touch $blockfile
past_blockcount=`tail -n 1 $blockfile`
echo  "past blockcount:$past_blockcount"

current_blockcount=`./coind -datadir=. getblockcount`
echo "current blockcount:$current_blockcount"

if [ $current_blockcount = $past_blockcount ]
then
  echo "current blockcount equal to past blockcount, so restart the waykicoind."
  ./coind -datadir=. stop
  sleep 3
  ./coind -datadir=.
else
  echo "current blockcount does not equal to past blockcount, so go ahead."
  past_blockcount=$current_blockcount
fi

echo $current_blockcount>$blockfile
echo "Now the past blockcount is $past_blockcount "

echo "end！"

```

* use `crontab -e` add task

**example**

Detect it once per hour
```
0 */1 * * *   /root/wayki/auto_restart.sh >> /root/wayki/auto_restart.log

```
**Note: the parameter `current_blockcount` in shell script is folder path of `coind` **

## Download blocks too slow
\*\* downloading blocks that have been synced manually\*\*

It is estimated that it takes more than 10 hours to download the block from 0. If you want to quickly synchronize the block, you can download the synchronized data through the following link and copy the the folder named `blocks` to the folder named `main`(mainnet) and then restart.

* mainnet:
http://waykichain.oss-cn-shenzhen.aliyuncs.com/mainnet/wicc-main-blocks.tar.gz


## `WTIMES`
* wtimes is an important token in the Waykichain. In the future, you can redeem the gifts around the Waykichain. Please pay attention to APP from [Official website][1]
* wtimes operates at a penny RMB, can only redeem gifts, There is no withdrawal.

[1]:	http://www.waykichain.com/

[image-1]:	../images/decimal_en.png

# WiccWallet
WaykiMax provides the `WiccWallet` interface to developers. Developers can integrate `WiccWallet` when developing DApp so that they can sign and broadcast WaykiChain transactions.In turn enabling the use of Dapps from within the browser.

**Note 1: Global parameter `WiccWallet` is injected by WaykiMax**

**Note 2: If the developer needs to call the wallet on the mobile and Web using the same set of code (H5), please use [WaykiBridge](../DeveloperHelper/application_api.md)**

## getDefaultAccount
Get the default account with method below:

```
WiccWallet.getDefaultAccount().then((account) => {
 console.log(account) 
}, (error) => {
  console.log(error)
})
```

If the wallet is unlocked, and the account is created, the return result is below: 

```
{
  "account": {
    "address": "WTTqpSPZ9JHQJAPsmJQb1kXcMz5HvRWJLv",
    "id": "eddd9578-8264-4f62-b215-da22a455152b",
    "testnetAddress": "wQREsAsRAV1j133FHK4M5XQPPG3n4W23P1"
  },
  "network": "testnet",
  "address": "wQREsAsRAV1j133FHK4M5XQPPG3n4W23P1"
}
```

* network: testnet or mainnet； 
* address: current address in WaykiMax  
* other parameter can be ignored

## callContract
call the smart contract api 

**Parameters**

`regId` smart contract regid 

`contract` the command to call smart contract

`amount` wicc WICC amount transferred to smart contract, **unit is `sawi`** 

`callback` callback after the user click the confirm button, the first param is error，the second param is data; error is null，wwhen call successfully。

**Return data when call successfully** 
 `amount` WICC Amount **unit is `sawi`**
 `fee` miner fee **unitis `sawi`**
 `contract` the command to call smart contract
 `txid` the transaction hash
 `regid` smart contract regid

**Method**

`WiccWallet.callContract(regId, contract, amount, callback)`

**Example**
```
// call
WiccWallet.callContract('13103-1', 'f0', 0, (error, data) => console.log(error, data)).then(() => {
}, (error) => {
  console.log(error)
})

// return
{
  amount: 0,
  fee: 1000000,
  contract: "f0",
  txid: "55ec1c7abe38c492894a09f0c99e16d7c8d68608a2f1f6d334d0fd101b503e5b",
  regId: "9109-1", 
   …
}
```

## publishContract
call publish smart contract api

**Parameters**

`content` the smartcontract content 

`description` the smartcontract descrition

`callback` please refer to callContract 

**Return data when call successfully** 
* please refer to callContract

```
// call
WiccWallet.publishContract('mylib = require "mylib"', '33333', (error, data) => console.log(error, data)).then(() => {
}, (error) => {
  console.log(error)
})
```

## requestPay
call request pay api 

**Parameters**

`destAddress` the destination address 

`amount` WICC amount **unit is `sawi`**

`description`  the request pay description

`callback` please refer to callContract 

**Return data when call successfully** 
* please refer to callContract

**Example**
```
WiccWallet.requestPay('wYXV7QzHZnb8LuWw7Xa24dfUTqmH2tNZBq', 0.01 * Math.pow(10, 8), 'desc', (error, data) => console.log(error, data)).then(() => {
}, (error) => {
  console.log(error)
})
```


## requestVote
call request vote api

**Parameter**
`votelist` the vote content arraylist, there are parameter address and votes in the arraylist 
* address : the voted destination address  
* votes : the voted count, WICC amount **unit is `sawi`**
`callback` please refer to callContract 

**Return data when call successfully** 
* please refer to callContract

```
WiccWallet. requestVote([{
  address: 'wNQ8bTNN9eRj4U5hawdRJhvoGpMAfHfrZP',
  votes: 2000001
}, {
  address: 'wfhKnmXbE3n49trwd9CsvnzGqScJLLf4Jb',
  votes: 2000002
}], (error, data) => console.log(error, data)).then(() => {
}, (error) => {
  console.log(error)
})
```

