<extoc></extoc>
# Testnet node construction


## Testnet for Docker
---

Refer to [build for docker][1] And [WaykiChain.conf][2]

### The local directory `/opt/wicc` is should be structured as follows. 
```
~/workspace/wicc/WaykiChain_testnet$tree
.
├── bin
│   └── run-waykicoind-test.sh
└── conf
    └── WaykiChain.conf

2 directories, 2 files
~/workspace/wicc/WaykiChain_testnet$
```

#### Startup script content `run-waykicoind-test.sh`
```shell
docker run --name waykicoind-testnet -p 18920:18920 -p 6967:6968 \
  -v `pwd`/conf/WaykiChain.conf:/root/.WaykiChain/WaykiChain.conf \
  -v `pwd`/data:/root/.WaykiChain/testnet \
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
genblock=0
genblocklimit=100000000
rpcthreads=8
#testnet
testnet=1
```

### Start node

```sh bin/run-waykicoind-test.sh```

**example**

```
\~/workspace/wicc/WaykiChain\_testnet$sh bin/run-waykicoind-test.sh
284cb238ac8a0aa737d1c6c9033358d464c9a274f669e8c79acfc3776f4e6984
\~/workspace/wicc/WaykiChain\_testnet$docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                                                                 NAMES
284cb238ac8a        wicc/waykicoind     "./coind"           6 seconds ago       Up 5 seconds        8920/tcp, 0.0.0.0:6967-\>6968/tcp, 0.0.0.0:18920-\>18920/tcp           waykicoind-testnet
\~/workspace/wicc/WaykiChain\_testnet$
```

### Check if the node is working properly

```docker exec -it waykicoind-testnet /bin/bash```

**example**

```
\~/workspace/wicc/WaykiChain\_testnet$docker exec -it 284cb238ac8a /bin/bash
root@284cb238ac8a:/opt/wicc# coind getinfo
{
	"version" : 1010101,
	"fullversion" : "v1.1.1.1-30ac533-release-linux (2019-01-21 09:50:49 +0800)",
	"protocolversion" : 10001,
	"walletversion" : 0,
	"balance" : 0.00000000,
	"timeoffset" : 0,
	"proxy" : "",
	"nettype" : "TEST_NET",
	"genblock" : 1,
	"chainwork" : "00000000000000000000000000000000000000000000000000000000000005ce",
	"tipblocktime" : 1547977400,
	"paytxfee" : 0.00010000,
	"relayfee" : 0.00001000,
	"fuelrate" : 1,
	"fuel" : 0,
	"data directory" : "/root/.WaykiChain/testnet",
	"tip block hash" : "c25cc8e2e416b82f03ec57b36b662968b8ca962a19dcbfc0efcf74a358cedffb",
	"syncheight" : 6612,
	"blocks" : 1486,
	"connections" : 3,
	"errors" : ""
}
root@284cb238ac8a:/opt/wicc\#
```
If the return value is the same as above, the Testnet node is successfully built.


## Testnet for Linux
---

Refer to [build locally](build.md) And [WaykiChain.conf](conf.md)

### The local directory `/opt/wicc` is structured as follows
```
root@ubuntu:/opt/wicc# ls
coind  WaykiChain  WaykiChain.conf
root@ubuntu:/opt/wicc\#
```


#### Configuration file content`WaykiChain.conf`
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
genblock=0
genblocklimit=100000000
rpcthreads=8
# testnet
testnet=1
```

### Start node
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
Startup time: 2019-01-21 07:36:46
Default data directory /root/.WaykiChain
Using data directory /opt/wicc/./testnet
Using at most 125 connections (      1024 file descriptors available)

```

### Check if the node is working properly

After the node starts, run the following command in a new terminal window to confirm whether the node is running normally.
```
./coind -datadir=. getinfo
```

**example**
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
	"nettype" : "TEST_NET",
	"chainwork" : "00000000000000000000000000000000000000000000000000000000000001f4",
	"tipblocktime" : 1547962110,
	"paytxfee" : 0.00010000,
	"relayfee" : 0.00001000,
	"fuelrate" : 1,
	"fuel" : 0,
	"data directory" : "/opt/wicc/./testnet",
	"tip block hash" : "89ebe282d29d37ff8723e943fa2978d37ed9f28dd03c9b6fa43cb6ce3a89395e",
	"syncheight" : 6562,
	"blocks" : 500,
	"connections" : 1,
	"errors" : ""
}
root@ubuntu:/opt/wicc\#
\`\`\`
If the return value is the same as above, the Testnet node is successfully built.

[1]:	./build.md
[2]:	./conf.md
