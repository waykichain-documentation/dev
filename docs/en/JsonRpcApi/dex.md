submit a dex buy limit price order tx.

Arguments:
1."addr": (string required) order owner address
2."coin_symbol": (string required) coin type to pay
3."asset_symbol": (string required), asset type to buy
4."asset_amount": (numeric, required) amount of target asset to buy
5."price": (numeric, required) bidding price willing to buy
6."symbol:fee:unit":(string:numeric:string, optional) fee paid for miner, default is WICC:10000:sawi

Result:
"txid" (string) The transaction id.

Examples:
> ./coind submitdexbuylimitordertx "10-3" "WUSD" "WICC" 1000000 200000000


As json rpc call
> curl --user myusername -d '{"jsonrpc": "1.0", "id":"curltest", "method": "submitdexbuylimitordertx", "params": ["10-3" "WUSD" "WICC" 1000000 200000000
] }' -H 'Content-Type: application/json;' http://127.0.0.1:8332/
