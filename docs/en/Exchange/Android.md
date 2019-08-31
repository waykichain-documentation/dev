# Android Library
#### Android Wallet Offline Signature Library Development Guide

## Introduction
When developers need to use Android native client to `generate mnemonic`, `mnemonic To private key`, `create address`, `registered address`, `transfer`, `deployment contract`, `call contract`, `vote`, etc., WaykiChain provides Android's offline signature library for its use.

## Sourcecode and library

Android native wallet offline signature library compiled based on `Golang` language offline signature library. source code:

https://github.com/WaykiChain/wicc-wallet-utils-go

Android native wallet offline signature library download url:

https://github.com/WaykiChain/wicc-wallet-utils-go/releases/download/v1.0/and-wiccwallet.aar

## Instructions
### Import the signature library
Put `and-wiccwallet.aar` in the `libs` directory of the corresponding project.

Add `api fileTree(dir: 'libs', include: ['*.jar','*.aar'])` to the corresponding file.

## Instructions for use
 \* The Mainnet wallet address has a length of 34 characters starting with the letter `W` and a network parameter `2`.
 
 \* The Testnet wallet address has a length of 34 characters starting with the letter `w` and a network parameter `1`.

 \* The Mainnet wallet private key has a length of 34 characters starting with the letter `P` and a network parameter `2`.
 
 \* The Testnet wallet private key has a length of 34 characters starting with the letter `Y` and a network parameter `1`.

### Generating mnemonic
By default, a mnemonic with 12 words is generated.
```
  String mnemonics=Wiccwallet.generateMnemonics();
  String[] words=mnemonics.split(" ");
```

### Mnemonic To private key for Mainnet
```
  String privateKey=Wiccwallet.getPrivateKeyFromMnemonic(mnemonics,2);
```

### Generating address for Mainnet
```
  String address=Wiccwallet.getAddressFromMnemonic(mnemonics,2);
```

### Get registered address signature
If you want to use the created address, you need to spend 10000`sawi` fees to register. Before you register, make sure you have a certain WICC balance.
```
  //Get the wallet registration signature
  RegisterAccountTxParam registerAccountTxParam=new RegisterAccountTxParam();
        registerAccountTxParam.setFees(10000);//miners fee at least 0.0001wicc
        registerAccountTxParam.setValidHeight(95762);/block height
        Try {
            String signTx=Wiccwallet.
       signRegisterAccountTx(privateKey,registerAccountTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### Getting a transfer signature
```
      CommonTxParam commonTxParam=new CommonTxParam();
        commonTxParam.setDestAddr("");//payment address
        commonTxParam.setFees(10000);//Mining fee is at least 0.0001wicc
        commonTxParam.setSrcRegId("");//The id obtained by the wallet registration
        commonTxParam.setValidHeight(95762);//block height
        commonTxParam.setValues(100000000);//transfer 1 wicc
        Try {
            String commonTx=Wiccwallet.signCommonTx(privateKey,commonTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### Get the deployment contract signature
```
     RegisterContractTxParam registerContractTxParam=new RegisterContractTxParam();
        registerContractTxParam.setDescription("Contract description");
        registerContractTxParam.setFees (110000000); / / deployment contract tip is best set a little bigger
        registerContractTxParam.setScript("contract".getBytes());
        registerContractTxParam.setSrcRegId("");//The registration id of the wallet of the deployment contract
        registerContractTxParam.setValidHeight(95762);//block height

        Try {
      String signTx= Wiccwallet.signRegisterContractTx(privateKey,registerContractTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### Get the call contract signature
```
     CallContractTxParam callContractTxParam=new CallContractTxParam();
        callContractTxParam.setAppId("");//Registered id of smart contract
        callContractTxParam.setContractHex("f20312312");//call contract assembly field
        callContractTxParam.setFees(10000);
        callContractTxParam.setSrcRegId("");//your wallet registration id
        callContractTxParam.setValidHeight(95762);//block height, need to be available at any time
        callContractTxParam.setValues(100000000);//The amount does not have to be filled, depending on whether the contract needs

        Try {
            Wiccwallet.signCallContractTx(privateKey, callContractTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### Getting a voting signature
```
  DelegateTxParam delegateTxParam=new DelegateTxParam();
        delegateTxParam.setValidHeight(95762);//block height, need to be available at any time
        delegateTxParam.setSrcRegId("");//your wallet registration id
        delegateTxParam.setFees(10000);
        OperVoteFunds funds=new OperVoteFunds();
        OperVoteFund fund=new OperVoteFund();
        fund.setVoteValue(10000);//votes
        fund.setPubKey(hexString2binaryString("03aaee970fec78b646a61b7c61b170ac7e0bfb47faef2b641074ad9f4ed75b31b8"));//The public key of the address at which the vote was obtained
 Funds.add(fund);
        Try {
         String delegateTx= Wiccwallet.signDelegateTx(privateKey,delegateTxParam);
        } catch (Exception e) {
            e.printStackTrace();
        }
```
