<extoc></extoc>
# WaykiBridge

## Introduction
`WaykiBridge` is the DAPP development interface tool launched by the WiykiChain develop team. Developers can integrate the `WaykiBridge` interface when developing DAPP, that is, you only need a set of code（H5） to call the `WaykiMax` and [WaykiTimes APP](https://www.waykichain.com/WaykiTimes.html) wallet for signing and broadcasting WaykiChain transactions so that users can be in `Browser` and [WaykiTimes APP](https://www.waykichain.com/WaykiTimes.html) DAPP is used in the Application Center.

## Tools Download
[Web Wallet Extension WaykiMax Download](webextension.md)

[WaykiTimes Developer IOS Edition Download](https://www.pgyer.com/oqOK)

[WaykiTimes Developer Android Edition Download](https://wicc-dev-mix.oss-cn-shenzhen.aliyuncs.com/android/4prod/waykitimes/%E5%BC%80%E5%8F%91%E8%80%85%E7%89%88%E6%9C%AC/WaykiTimes_1.3.0_productiondev_release.apk)


## WaykiBridge calling example
https://gitlab.com/waykichain-public/wicc-dapps/wicc-appcenter-dev

## WaykiBridge API Description

### Prerequisite work
import the javascript code
```
  <script src="https://sw91.net/devops/js/waykiBridge.js"></script>
```

* Note:  
Please do the delay processing  if you need to call the interface when the page loads after import the file,  (setTimeout(() => this.getAddressInfo(), 100);)

Param：method，param，callback when succeed，callback when failed
```
waykiBridge.walletPlugin(name,query,function(res){},function(err){})
```
---


>Note：All the param of query is string or array

### call smart contract

query param：

```
{
    "regId":"",     // contract regid
    "contractField", "", // contract field
    "inputAmount":"",   // imput money
    "remark" :""  // optional
}
```
example:
```
    waykiBridge.walletPlugin(
        "walletPluginContractInvoke",
        {
          regId: "13103-1",
          contractField: "f0",
          inputAmount: "1",
          remark : ""

        },
        function(res) {
            console.log(res)
        },
        function(err) {
            console.log(err)
        }
    );
```
callback when succeed
```
{ 
    "result": { 
        "amount": 100000000,
        "fee": 1000000, 
        "contract": "f0",
        "txid":"029c86a648030e2b28ccc64c5ed60c96a0c61de95a30cab82159a476ceeeaf3d", 
        "regId": "91647-1", 
        "txType": null,
        "toAddress": "wNTJYM3gyXJH9dPQe96ofyHotf5eq3EP14", 
        "sendAddress": "wLnwB9n9PCdP2sWAF3R3KvMujxGnVsE6aa" 
      }, 
    "errorCode": 0 
}
```
callback when failed
```
{ "errorMsg": "errors.InvalidArgument is not a constructor" }
```


### Contract Publish
query param
```
{
    "contractContent": "", // Contract content
    "contractDesc": ""     // Contract Description
}
```
example
```
    waykiBridge.walletPlugin(
        "walletPluginContractInvoke",
        {
          contractContent: 'mylib = require "mylib"',
          contractDesc: "Description",
        },
        function(res) {
            console.log(res)
        },
        function(err) {
            console.log(err)
        }
    );
```
callback when succeed
```
{
    "result": { 
         "amount": 0, 
         "fee": 110000000, 
         "contract": null, 
         "txid": "4bbbee4320843d19d4a008bfc7dc1a26fdf5ed9c1a96415db033d4f83c6e9fa0", 
         "regId": "91647-1", 
         "txType": null,
         "toAddress": null,
         "sendAddress": "wLnwB9n9PCdP2sWAF3R3KvMujxGnVsE6aa"
    }, 
    "errorCode": 0 
}
```
callback when failed 
```
{ "errorMsg": "error from server response: [-4],submittx Error:operate-account-failed " }
```
### walletPluginTransfer
param
```
{
    "amount",""   //transfer amount 
    "collectionAddress", "" // oject address
    "remark": "" // comment
}
```
example:
```
    waykiBridge.walletPlugin(
        "walletPluginTransfer",
        {
          amount: "100000000", 
          collectionAddress: "Wi2H3XAhMtdLPkjQVSkYXhF3GRNTwAhtqA",
          remark: "comment"
        },
        function(res) {
          _this.transferRes = res;
        },
        function(err) {
          _this.transferRes = err;
        }
    );
```
callback when succeed
```
{ 
    "result": { 
        "amount": 10000000, 
        "fee": 10000, 
        "contract": "", 
        "txid": "64244292b9abb2e5b8d55a3f37584781e0151e0358a440b0e910100f9970957b",
        "regId": "91647-1", 
        "txType": null, 
        "toAddress": "weyg6FeZP5Mf6dNn1TQJbU7pHhLx1QMkZ8", 
        "sendAddress": "wLnwB9n9PCdP2sWAF3R3KvMujxGnVsE6aa" 
    }, 
    "errorCode": 0 
}
```

callback when failed 
```
{ "errorMsg": "error from server response: [-4],submittx Error:dust amount " }
```

### getAddressInfo
param
```
{} // There is no param, but please transfer {}
```
example
```
    waykiBridge.walletPlugin(
        "getAddressInfo",
        {},
        function(res) {
            console.log(res)
        },
        function(err) {
            console.log(err)
        }
    );
```
callback when succeed
```
{ 
    "result": { 
        "account": { 
            "address": "WPqY8RJHN1u4Kzrnj2mHG9V8igJJVVcveb", 
            "id": "bd2356fa-6137-448e-806f-d6ab09785749", "testnetAddress": "wLnwB9n9PCdP2sWAF3R3KvMujxGnVsE6aa" 
            }, 
        "network": "testnet", 
        "address": "wLnwB9n9PCdP2sWAF3R3KvMujxGnVsE6aa" 
    }, 
    "errorCode": 0 
}
```
callback when failed 
```
{ "errorMsg": "Please unlock wallet first" }
```
---

### The unified error message 
```
{
    "errorCode": int   //error code： 0- succeed 100-param error  101-cancel 200-wallet is not exist
    "errorMsg" : ""    //error message
    "result": ""       //return data （when succeed（errorCode == 0））
}
```
