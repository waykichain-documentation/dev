# Integration Guide
### WaykiChain Integration Guide for Crypto Exchanges

The WaykiChain balance model is `account` model similar to Ethereum, **Not** `UTXO` model.

#### 1. Node Wallet Deployment

Deployment refer to [NodeDeployment Local][1] Or [NodeDeployment Docker][2]

#### 2. Implementation Options

According to WaykiChain's integration experience, there are two groups of Crypto Exchanges when it comes to implementation choices: one that prefers a faster and simplified integration approach which we call Hot Wallet Mode and the other that requires strong security implementation which we call Hot-Cold Wallet mode, as depicted in following diagrams.

In either approach, it is required to host a WaykiChain wallet node exclusively inside a modern Linux system like a recent distro of Ubuntu or CentOS. As for other components for the integration of WaykiChain node, it's up to Crypto Exchange to decide which environment to host their integration components.

##### a. Option-1: Hot Wallet Mode
In this integration mode, there's a network-connected (thus hot) wallet that manages a set of WaykiChain accounts and keys. Strong security measures must be put in place around this wallet to prevent network penetration by any unauthorized party that tries to steal the private keys of those accounts or or sabotage the overall service. Failing to do so, can result in a loss of funds.

In the diagram above, there's a dockerized environment that runs a WaykiChain node which we call a hot wallet node due to its network connection status. All exchange-owned accounts or custody accounts can be managed with this single wallet node which provides the services of account creation, activation and coin transfer between different accounts.

It is also however possible that a crypto exchange might deploy more than one such WaykiChain hot wallet node in order to distribute the crypto accounts more evenly among the wallets for load-balance or safety reasons.


##### b. Option-2: Hot-Cold Wallet Mode

As shown in the following diagram, the major implementation difference compared to Hot Wallet Mode from option-1 is that there is a newly added component called cold wallet program without any network connection. This wallet that also manages a set of WICC accounts can be rather safe from any network mounted attacks.

In this dual-wallet model, the hot wallet usually manages a reasonably sufficient amount of WICC coins to allow customers to withdraw coins freely from the exchange and the cold wallet reverses the bulk of coins that are not very often touched. In this setup, the overall security is better than the hot-wallet mode.

A wallet which its total amount of coins reach a set warning level below which then a customer withdrawal needs could not be met. Since the cold wallet is disconnected from the network, the only way to submit a coin-transfer transaction is to let the cold wallet program generate raw string offline to sign the transaction. Also can be produced in the form of QR code. Thus, an exchange operator can use a mobile phone with a specially built APP to scan the QR code and use it to submit the raw transaction string to the network via the hot wallet.

The rest of the integration components as illustrated in the above diagram are the same as those in Diagram-5-a.


#### 3.Common Method for Exchange
Refer to [3. JSON RPC API]

**Common Method list for Exchange**
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

[1]:	NodeDeployment/build.md
[2]:	NodeDeployment/docker.md
