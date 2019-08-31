<extoc></extoc>
# RegTestNet-mode Node Deployment


## Run RegTestNet node via Docker
---

Refer to [build for Docker][1] And [WaykiChain.conf][2]

### The local directory `/opt/wicc` is structured as follows
```
~/workspace/wicc/WaykiChain_pri$tree
.
├── bin
│   └── run-waykicoind-pri.sh
└── conf
    └── WaykiChain.conf

2 directories, 2 files
~/workspace/wicc/WaykiChain_pri$
```

#### Startup script content `run-waykicoind-pri.sh`
```shell
docker run --name waykicoind-pri -p 1920:18920 -p 1968:6968 \
  -v `pwd`/conf/WaykiChain.conf:/root/.WaykiChain/WaykiChain.conf \
  -v `pwd`/data:/root/.WaykiChain/regtest \
  -v `pwd`/bin:/opt/wicc/bin \
  -v `pwd`/lua:/tmp/lua \
  -d wicc/waykicoind
```

#### Configuration file content `WaykiChain.conf`
```
rpcuser=waykichain
rpcpassword=wicc123
blockminsize=1000
zapwallettxes=0
debug=INFO
logprinttoconsole=0
logtimestamps=1
logprinttofile=1
logprintfileline=1
server=1
listen=1
uiport=4555
rpcport=6968
rpcallowip=*.*.*.*
isdbtraversal=1
disablesafemode=1
genblock=1
genblocklimit=100000000
rpcthreads=8
#regtest
regtest=1
```

### Start node

```sh bin/run-waykicoind-pri.sh```

**example**

```
\~/workspace/wicc/WaykiChain\_pri$sh bin/run-waykicoind-pri.sh
dfc791d535b05fa273112b3c0b9319670dfaf88218649e63eddfb110828c6541
\~/workspace/wicc/WaykiChain\_pri$docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                                                                 NAMES
dfc791d535b0        wicc/waykicoind     "./coind"           13 seconds ago      Up 12 seconds       8920/tcp, 0.0.0.0:1920-\>1920/tcp, 18920/tcp, 0.0.0.0:1968-\>6968/tcp   waykicoind-pri
\~/workspace/wicc/WaykiChain\_pri$
```

### Check if the node is up and running properly

```docker exec -it waykicoind-pri /bin/bash```

**example**

```
\~/workspace/wicc/WaykiChain\_pri$docker exec -it waykicoind-pri /bin/bash
root@dfc791d535b0:/opt/wicc# coind getinfo
{
	"version" : 1010101,
	"fullversion" : "v1.1.1.1-30ac533-release-linux (2019-01-21 09:50:49 +0800)",
	"protocolversion" : 10001,
	"walletversion" : 0,
	"balance" : 207899994.99999595,
	"timeoffset" : 0,
	"proxy" : "",
	"nettype" : "REGTEST_NET",
	"genblock" : 1,
	"chainwork" : "000000000000000000000000000000000000000000000000000000000000267a",
	"tipblocktime" : 1548053720,
	"paytxfee" : 0.00010000,
	"relayfee" : 0.00001000,
	"fuelrate" : 1,
	"fuel" : 0,
	"data directory" : "/root/.WaykiChain/regtest",
	"tip block hash" : "94a5ffffd06f6d21ac8dd4ab7e395434ac6c857a612ef671c5b6819243efab15",
	"syncheight" : 0,
	"blocks" : 0,
	"connections" : 0,
	"errors" : ""
}
root@dfc791d535b0:/opt/wicc\#
```
If the return value is the same as above, the Privatenet node is successfully built.

### Import all 11-miner addresses
Please refer to the above section for direct host deployment.

The WaykiChain `DPOS` consensus mechanism generates a block every 10s.

## Run RegTestNet Node within Linux
---

Refer to [build locally](build.md) And [WaykiChain.conf](conf.md)

### The local directory `/opt/wicc` is structured as follows
```
root@ubuntu:/opt/wicc# ls
coind  WaykiChain  WaykiChain.conf
root@ubuntu:/opt/wicc\#
```

#### Configuration file content `WaykiChain.conf`
```
rpcuser=wiccuser
rpcpassword=123456
blockminsize=1000
zapwallettxes=0
debug=INFO
logprinttoconsole=0
logtimestamps=1
logprinttofile=1
logprintfileline=1
server=1
listen=1
uiport=4555
rpcport=6968
rpcallowip=*.*.*.*
isdbtraversal=1
disablesafemode=1
genblock=1
genblocklimit=100000000
rpcthreads=8
# regtest
regtest=1
```

### Start the node
command
```
./coind -datadir=.
```

**example**
```
root@ubuntu:/opt/wicc# ./coind -datadir=.
WaykiChain version v1.1.1.1-b7f1563-release-linux (2019-01-18 16:18:40 +0800)
Using OpenSSL version OpenSSL 1.0.1f 6 Jan 2014
Using Lua version Lua 5.3.1
Using Boost version 1.54
Using Level DB version 1.17
Using Berkeley DB version Berkeley DB 4.8.30: (April  9, 2010)
Startup time: 2019-01-21 03:13:08
Default data directory /root/.WaykiChain
Using data directory /opt/wicc/./regtest
Using at most 125 connections (      1024 file descriptors available)

```

### Check if the node is up and running properly

After the node starts, run the following command in the new terminal window to confirm whether the node is running normally.
```
./coind -datadir=. getinfo
```

**Examples**
```
root@ubuntu:/opt/wicc# ./coind -datadir=. getinfo
{
	"version" : 1010101,
	"fullversion" : "v1.1.1.1-b7f1563-release-linux (2019-01-18 16:18:40 +0800)",
	"protocolversion" : 10001,
	"walletversion" : 0,
	"balance" : 0.00000000,
	"timeoffset" : 0,
	"proxy" : "",
	"nettype" : "REGTEST_NET",
	"chainwork" : "0000000000000000000000000000000000000000000000000000000000000000",
	"tipblocktime" : 1504305600,
	"paytxfee" : 0.00010000,
	"relayfee" : 0.00001000,
	"fuelrate" : 100,
	"fuel" : 0,
	"data directory" : "/opt/wicc/./regtest",
	"tip block hash" : "ab8d8b1d11784098108df399b247a0b80049de26af1b9c775d550228351c768d",
	"syncheight" : 0,
	"blocks" : 0,
	"connections" : 0,
	"errors" : ""
}
root@ubuntu:/opt/wicc\#
```
If the return value is the same as above, the RegTestNet node program was successfully built.

### Import all 11-miner addresses
WaykiChain adopts `DPoS` consensus mechanism for block production and validation and requires having 11 elected miners (through votes) take turns in adding transactions into new blocks. In order to run a single-node program that emulates all 11 miners, following 11 miners keys must be imported into the same node wallet.

Refer to JSON RPC [importprivkey](../JsonRpcApi/account.md/#importprivkey) to import privatekey

| regid | privatekey                                           | remark                |
|:----- |:---------------------------------------------------- |:--------------------- |
| 0-1   | Y6J4aK6Wcs4A3Ex4HXdfjJ6ZsHpNZfjaS4B9w7xqEnmFEYMqQd13 | Asset initial account |
| 0-2   | Y5F2GraTdQqMbYrV6MG78Kbg4QE8p4B2DyxMdLMH7HmDNtiNmcbM | miner                 |
| 0-3   | Y7HWKeTHFnCxyTMtCEE6tVkqBzXoN1Yjxcx5Rs8j2dsSSvPxvF7p | miner                 |
| 0-4   | Y871eB5Xiss2ugKWQRb4nmMhKTnmXAEyUqBimTCupogzoSTVCSU9 | miner                 |
| 0-5   | Y9cAUsEhfsihbePnCYYCETpN1PVovqTMX4kauKRsZ9ERdz1uumeK | miner                 |
| 0-6   | Y4unEjiFk1YJQi1jaT3deY4t9Hm1eSk9usCam35LcN85cUA2QmZ5 | miner                 |
| 0-7   | Y5XKsR95ymf2pEyuhDPLtuvioHRo6ogDDNnaf4YU91ABvLb68QBU | miner                 |
| 0-8   | Y7diE8BXuwTkjSzgdZMnKNhzYGrU8oSk31anJ1mwipSCcnPakzTA | miner                 |
| 0-9   | YCjoCrtGEvMPZDLzBoY9GP3r7pqWa5mgzUxqAsVub6xnUVBwQHxE | miner                 |
| 0-10  | Y6bKBN4ZKBNHJZpQpqE7y7TC1QpdT32YtAjw4Me9Bvgo47b5ivPY | miner                 |
| 0-11  | Y8G5MwTFVsqj1FvkqFDEENzUBn4yu4Ds83HkeSYP9SkjLba7xQFX | miner                 |
| 0-12  | YAq1NTUKiYPhV9wq3xBNCxYZfjGPMtZpEPA4sEoXPU1pppdjSAka | miner                 |

Refer to JSON RPC [listaddr](../JsonRpcApi/account.md/#listaddr) to check all of address
```
{
	"result": [
	    {
	        "addr": "wLKf2NqwtHk3BfzK5wMDfbKYN1SC3weyR4",
	        "balance": 207900000,
	        "haveminerkey": false,
	        "regid": "0-1"
	    },
	    {
	        "addr": "wNDue1jHcgRSioSDL4o1AzXz3D72gCMkP6",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-2"
	    },
	    {
	        "addr": "wNuJM44FPC5NxearNLP98pg295VqP7hsqu",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-3"
	    },
	    {
	        "addr": "wP64X59EoRmeq2M5GrJ23UVttE9uxnuoFa",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-4"
	    },
	    {
	        "addr": "wQewSbKL5kAfpwnrivSiCcaiFffgNva4uB",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-5"
	    },
	    {
	        "addr": "wQquTWgzNzLtjUV4Du57p9YAEGdKvgXs9t",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-6"
	    },
	    {
	        "addr": "wRQwgYkPNe1oX9Ts3cfuQ4KerqiV2e8gqM",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-7"
	    },
	    {
	        "addr": "wSjMDgKWHC2MzrUamhJtyyR2FTtw8oMUfx",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-8"
	    },
	    {
	        "addr": "wSms4pZnNe7bxjouLxUXQLowc7JqtNps94",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-9"
	    },
	    {
	        "addr": "wT75mYY9C8xgqVgXquBmEfRmAXPDpJHU62",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-10"
	    },
	    {
	        "addr": "wUt89R4bjD3Ca6Vb7mk18oGsVtSTCxJu2q",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-11"
	    },
	    {
	        "addr": "wVTUdfEaeAAVSuXKrmMyqQXH5j5Z9oGmTt",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": "0-12"
	    },
	    {
	        "addr": "whM1dLmYb8uAPEaeCMFKrt7FJWFMdS2jKg",
	        "balance": 0,
	        "haveminerkey": false,
	        "regid": " "
	    }
	],
	"error": null,
	"id": "curltext"
}
\`\`\`
The WaykiChain `DPOS` consensus mechanism generates a block every 10s.


[1]:	./build.md
[2]:	./conf.md
