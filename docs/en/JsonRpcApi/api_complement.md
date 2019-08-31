
<extoc></extoc>

# Complement API 
---

## getbalance
Get balance


**Parameters**

`address` address in wallet (Optional, empty means get balance from all address in wallet)

**Returns**

`Balance` Balance **The unit is `sawi`**

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getbalance","params":["WR4fyUnQKhrxgXuMnjMCea6TNjK29GgHnY"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "balance": 1.00000000
  },
  "error": null,
  "id": "curltext"
}

```

**Comment**

This method can only query the wallet address, and the address must be recorded when the amount is transferred. When querying the account information, try to use the `getaccountinfo` method.
---

##block.md
---
1、### addnode
Add, delete, connect nodes

**Parameters**

`node` Node IP and port number

`command` 

| command | Description |
| :---   | :-----:                           |
| add    | Add a node to the addnode list     |
| remove | Remove a node from the addnode list   |
| onetry | Try to establish a link with the target node |

**Returns**

none

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"addnode","params":["47.92.143.137:18920","add"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":null,
  "error":null,
  "id":"curltext"
}
```
---
