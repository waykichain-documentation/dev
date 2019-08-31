<extoc></extoc>
# Waykichain.conf Parameter Description
Waykichain nodes support `privatenet`、`testnet`、`mainnet`

Depending on `Waykichain.conf` to configure different nettype to run blockchain

##### Configuration
|       name        | value(example) |                         meaning                          |
|:-----------------:|:--------------:|:--------------------------------------------------------:|
|      rpcuser      |   waykichain   |                       rpc username                       |
|    rpcpassword    |    admin123    |                       rpc password                       |
|   blockminsize    |      1000      |                                                          |
|   zapwallettxes   |       0        |                                                          |
|       debug       |      INFO      |                       debug level                        |
| logprinttoconsole |       0        |           log print to console; 1:true,0:false           |
|   logtimestamps   |       1        |           log print timestamps; 1:true,0:false           |
|  logprinttofile   |       1        |            log print to file; 1:true,0:false             |
| logprintfileline  |       1        |         log print with fileline; 1:true,0:false          |
|      server       |       1        |                                                          |
|      listen       |       1        |                                                          |
|      uiport       |      4555      |                                                          |
|      rpcport      |      6968      |                         rpc port                         |
|    rpcallowip     |  \*.\*.\*.\*   |                      rpc white list                      |
|   isdbtraversal   |       1        |                                                          |
|  disablesafemode  |       1        |                                                          |
|        genblock        |       0        | nodemode; 0(default):common node， 1:Block producer node |
|   genblocklimit    |    100000000     |                                                          |
|    rpcthreads     |       8        |                                                          |
|      testnet      |       0        |         nettype;  1:testnet , otherwise:mainnet          |
|      regtest      |       0        |        nettype;  1:privatenet , otherwise:mainnet        |


**example**


## privatenet
```
# rpc username
rpcuser=waykichain

#  rpc password
rpcpassword=admin123

blockminsize=1000
zapwallettxes=0
debug=INFO
printtoconsole=0
logtimestamps=1

# whether print the log info to file
logprinttofile=1
logprintfileline=1

server=1
listen=1
uiport=4555

# rpc port
rpcport=6968

# rpc white list
rpcallowip=*.*.*.*

isdbtraversal=1
disablesafemode=1
genblock=1
genblocklimit=100000000
rpcthreads=8

# privatenet
regtest=1
```
---

## testnet
```
# rpc username
rpcuser=waykichain

#  rpc password
rpcpassword=admin123

blockminsize=1000
zapwallettxes=0
debug=INFO
printtoconsole=0
logtimestamps=1

# whether print the log info to file
logprinttofile=1
logprintfileline=1

server=1
listen=1
uiport=4555

# rpc port
rpcport=6968

# rpc white list
rpcallowip=*.*.*.*

isdbtraversal=1
disablesafemode=1
genblock=0
genblocklimit=100000000
rpcthreads=8

# testnet
testnet=1
```
---

## mainnet
```
# rpc username
rpcuser=waykichain

#  rpc password
rpcpassword=admin123

blockminsize=1000
zapwallettxes=0
debug=INFO
printtoconsole=0
logtimestamps=1

# whether print the log info to file
logprinttofile=1
logprintfileline=1

server=1
listen=1
uiport=4555

# rpc port
rpcport=6968

# rpc white list
rpcallowip=*.*.*.*

isdbtraversal=1
disablesafemode=1
genblock=0
genblocklimit=100000000
rpcthreads=8

# mainnet

```
---