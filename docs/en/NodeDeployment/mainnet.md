<extoc></extoc>
# Mainnet node construction

## Mainnet for Docker
---

 Refer to [build for docker][1] And [WaykiChain.conf][2]

### The local directory `/opt/wicc` is structured as follows
```
~/workspace/wicc/WaykiChain_mainnet$tree
.
├── bin
│   └── run-waykicoind-main.sh
└── conf
    └── WaykiChain.conf

2 directories, 2 files
~/workspace/wicc/WaykiChain_mainnet$
```

#### Startup script content `run-waykicoind-main.sh`
```shell
docker run --name waykicoind-mainnet -p 8920:8920 -p 6968:6968 \
  -v `pwd`/conf/WaykiChain.conf:/root/.WaykiChain/WaykiChain.conf \
  -v `pwd`/data:/root/.WaykiChain/main \
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
```

### Start node

```sh bin/run-waykicoind-main.sh```

**example**

```
\~/workspace/wicc/WaykiChain\_mainnet$sh bin/run-waykicoind-main.sh
6594c134fb1fafa5804a86c8b4c03603830339029439ea2d4d41724e4716c502
\~/workspace/wicc/WaykiChain\_mainnet$docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                                                                 NAMES
6594c134fb1f        wicc/waykicoind     "./coind"           5 seconds ago       Up 4 seconds        0.0.0.0:6968-\>6968/tcp, 0.0.0.0:8920-\>8920/tcp             waykicoind-mainnet
\~/workspace/wicc/WaykiChain\_mainnet$
```

### Check if the node is working properly

```docker exec -it waykicoind-testnet /bin/bash```

**example**

```
\~/workspace/wicc/WaykiChain\_mainnet$docker exec -it 284cb238ac8a /bin/bash
root@6594c134fb1f:/opt/wicc# coind getinfo
{
	"version" : 1010101,
	"fullversion" : "v1.1.1.1-30ac533-release-linux (2019-01-21 09:50:49 +0800)",
	"protocolversion" : 10001,
	"walletversion" : 0,
	"balance" : 0.00000000,
	"timeoffset" : 0,
	"proxy" : "",
	"nettype" : "MAIN_NET",
	"genblock" : 1,
	"chainwork" : "000000000000000000000000000000000000000000000000000000000000069a",
	"tipblocktime" : 1525545370,
	"paytxfee" : 0.00010000,
	"relayfee" : 0.00001000,
	"fuelrate" : 1,
	"fuel" : 0,
	"data directory" : "/root/.WaykiChain/main",
	"tip block hash" : "dd5c6b8ee78f506f6faca879137d69d5b75d80ef4659b4af3f93d01bbc0a2ce3",
	"syncheight" : 1839397,
	"blocks" : 1690,
	"connections" : 8,
	"errors" : ""
}
root@6594c134fb1f:/opt/wicc\#
```
If the return value is the same as above, the Mainnet node is successfully built.


## Mainnets for Linux
---

efer to [build locally](build.md) And [WaykiChain.conf](conf.md)

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
genblock=0
genblocklimit=100000000
rpcthreads=8
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
Startup time: 2019-01-21 07:38:06
Default data directory /root/.WaykiChain
Using data directory /opt/wicc/./main
Using at most 125 connections (      1024 file descriptors available)

```

### Check if the node is working properly

After the node starts, run the following command in the new terminal window to confirm whether the node is running normally.
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
	"nettype" : "MAIN_NET",
	"chainwork" : "0000000000000000000000000000000000000000000000000000000000000774",
	"tipblocktime" : 1525547820,
	"paytxfee" : 0.00010000,
	"relayfee" : 0.00001000,
	"fuelrate" : 1,
	"fuel" : 0,
	"data directory" : "/opt/wicc/./main",
	"tip block hash" : "efb1d29a226692111996a40ea2b433b6b00e31a049f028f723ccf49114d0a98c",
	"syncheight" : 1839151,
	"blocks" : 1908,
	"connections" : 3,
	"errors" : ""
}
root@ubuntu:/opt/wicc\#
\`\`\`
If the return value is the same as above, the Mainnet node is successfully built.


[1]:	./build.md
[2]:	./conf.md
