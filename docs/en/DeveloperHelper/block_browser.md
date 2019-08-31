<extoc></extoc>
# BlockChain Explorer

## WaykiChain BlockChain Explorer
**mainnet**  https://www.waykiscan.com/

**testnet**   http://testnet.waykiscan.com

## The parsing template of smart contract parameter 

### Introduce
The developer writes the parameter parsing template according to the template rules and uploads the template through the blockchain explorer. When the blockchain explorer displays the transaction details of this contract, it parses the contract invocation parameters according to the rules written by the developer

###  Field definition

**The template use json format, and the meaning of each field is as follows:**  

| Field name      | Type                     | Meaning                |
| :------- | :------- | :------- |
| id | String |Contract RegId|
| name_zh | String |Template name（zh）|
| name_en | String |Template name（en）|
| version | String |Template version|
| methods | [Method](#method) |Mehod|


#### Method

| Field name      | Type                     | Meaning                |
| :------- | :------- | :------- |
| head | String |Method head|
| name_zh | String |Method name（zh）|
| name_en | String |Method name（en）|
| length | Int |The total length of the arguments that the method contains|
| params | [Param](#param) |Parameter|

#### Param 

| Field name      | Type                     | Meaning                |
| :------- | :------- | :------- |
| offset | Int |Parameter starting position|
| length | Int |Parameter length（*When the decode of the parameter is "list", The value intercepted based on the "length" of this parameter represents the number of parameters in "items"*）|
| label_zh| String | Parameter name（zh）|
| label_en | String | Parameter name（en）|
| decode | String |Coding format/Data structure（**number/hex/char/utf-8/list**）|
| options | Map<String, String> |Options（"key"："value"）|
| items |  [Item](#item)  |List parameter template|

#### Item

| Field name      | Type                     | Meaning                |
| :------- | :------- | :------- |
| length | Int |Parameter length|
| label_zh| String | Parameter name（zh）|
| label_en | String | Parameter name（en）|
| decode | String |Coding format/Data structure（**number/hex/char/utf-8/list**）|
| options | Map<String, String> |Options（"key"："value"）|


### Sample  


#### Template
```json
{
    "id":"123456-1",
    "name_zh":"维基竞猜参数解码规则",
    "name_en":"Decoding rule",
    "version":"v1",
    "methods":[
        {
            "head":"f001",
            "name_zh":"投注",
            "name_en":"betting",
            "length":130,
            "params":[
                {
                    "offset":4,
                    "length":68,
                    "label_zh":"用户地址",
                    "label_en":"user address",
                    "name":"用户地址",
                    "decode":"char"
                },
                {
                    "offset":72,
                    "length":16,
                    "label_zh":"投注金额",
                    "label_en":"Betting Amount",
                    "decode":"number"
                },
                {
                    "offset":88,
                    "length":6,
                    "label_zh":"开奖方式",
                    "label_en":"Way of winning",
                    "decode":"hex",
                    "options":{
                        "010101":"主胜",
                        "010102":"平",
                        "010103":"客胜",
                        "010301":"单",
                        "010302":"双"
                    }
                },
                {
                    "offset":94,
                    "length":36,
                    "label_zh":"用户昵称",
                    "label_en":"User Nickname",
                    "decode":"utf-8"
                }
            ]
        },
        {
            "head":"f002",
            "name_zh":"开奖",
            "name_en":"Lottery",
            "length":182,
            "params":[
                {
                    "offset":4,
                    "length":16,
                    "label_zh":"开奖金额",
                    "label_en":"Lottery amount",
                    "decode":"number"
                },
                {
                    "offset":20,
                    "length":8,
                    "label_zh":"中将列表",
                    "label_en":"Lottery amount",
                    "decode":"list",
                    "items":[
                        {
                            "length":68,
                            "label_zh":"派奖地址",
                            "label_en":"Award address",
                            "decode":"char"
                        },
                        {
                            "length":4,
                            "label_zh":"开奖类型",
                            "label_en":"Lottery type",
                            "decode":"hex",
                            "options":{
                                "0102":"单双",
                                "0103":"是否进球",
                                "0104":"胜负"
                            }
                        },
                        {
                            "length":62,
                            "label_zh":"说明",
                            "label_en":"intro",
                            "decode":"utf-8"
                        }
                    ]
                },
                {
                    "offset":430,
                    "length":36,
                    "label_zh":"备注信息",
                    "label_en":"remark",
                    "decode":"utf-8"
                }
            ]
        }
    ]
}
```
#### Demo 1

Original parameters：
>f00157566f516f79567135394753335131577a5a416332446d4d35447653785761796b6900daf89a00000000010301e8bf9be587bbe79a84e7bbb4e59fbae993be  

After split：
>f001<br>
>57566f516f79567135394753335131577a5a416332446d4d35447653785761796b69<br>
>00daf89a00000000 <br>
>010301 <br>
>e8bf9be587bbe79a84e7bbb4e59fbae993be<br>  

Parsing result：
```json
{
    "methodNameZh":"投注",
    "methodNameEn":"betting",
    "params":[
        {
            "labelZh":"用户地址",
            "labelEn":"user address",
            "value":"WVoQoyVq59GS3Q1WzZAc2DmM5DvSxWayki"
        },
        {
            "labelZh":"投注金额",
            "labelEn":"Betting Amount",
            "value":"2600000000"
        },
        {
            "labelZh":"开奖方式",
            "labelEn":"Way of winning",
            "value":"单"
        },
        {
            "labelZh":"用户昵称",
            "labelEn":"User Nickname",
            "value":"进击的维基链"
        }
    ]
}
```

#### Demo 2

Original parameters：
>f002005ed0b20000000003000000574e5057717639627646436e4d6d3164646951644837665577556b3251677273324e0102e5a596e98791e887aae58aa8e6b4bee58f912ce681ade5969ce4b8ade5a596576857534b364d346369424c71715a335a6b627932586445376154336f78705459570102e5a596e98791e887aae58aa8e6b4bee58f912ce681ade5969ce4b8ade5a5965763515435656f4a504748314237547858334275507747455a45364c7437655062690102e5a596e98791e887aae58aa8e6b4bee58f912ce681ade5969ce4b8ade5a596e8bf99e698afe5a487e6b3a8e4bfa1e681af  

After split：
>f002<br>
>005ed0b200000000<br>
>**03000000**<br>
>&emsp;574e5057717639627646436e4d6d3164646951644837665577556b3251677273324e <br>
>&emsp;0102<br>
>&emsp;e5a596e98791e887aae58aa8e6b4bee58f912ce681ade5969ce4b8ade5a596<br>
>
>&emsp;576857534b364d346369424c71715a335a6b627932586445376154336f7870545957 <br>
>&emsp;0102<br>
>&emsp;e5a596e98791e887aae58aa8e6b4bee58f912ce681ade5969ce4b8ade5a596<br>
>
>&emsp;5763515435656f4a504748314237547858334275507747455a45364c743765506269 <br>
>&emsp;0102<br>
>&emsp;e5a596e98791e887aae58aa8e6b4bee58f912ce681ade5969ce4b8ade5a596<br>
>e8bf99e698afe5a487e6b3a8e4bfa1e681af<br>  

Remark：
>The value of the third string "03000000" (decoded to be 3) represents the number of items elements in the parameter, and the number and length of each element should be consistent. 

Parsing result：
```json
{
    "methodNameZh":"开奖",
    "methodNameEn":"Lottery",
    "params":[
        {
            "labelZh":"开奖金额",
            "labelEn":"Lottery amount",
            "value":"3000000000"
        },
        {
            "labelZh":"中将列表",
            "labelEn":"Lottery amount",
            "items":[
                {
                    "items":[
                        {
                            "labelZh":"派奖地址",
                            "labelEn":"Award address",
                            "value":"WNPWqv9bvFCnMm1ddiQdH7fUwUk2Qgrs2N"
                        },
                        {
                            "labelZh":"开奖类型",
                            "labelEn":"Lottery type",
                            "value":"单双"
                        },
                        {
                            "labelZh":"说明",
                            "labelEn":"intro",
                            "value":"奖金自动派发,恭喜中奖"
                        }
                    ]
                },
                {
                    "items":[
                        {
                            "labelZh":"派奖地址",
                            "labelEn":"Award address",
                            "value":"WhWSK6M4ciBLqqZ3Zkby2XdE7aT3oxpTYW"
                        },
                        {
                            "labelZh":"开奖类型",
                            "labelEn":"Lottery type",
                            "value":"单双"
                        },
                        {
                            "labelZh":"说明",
                            "labelEn":"intro",
                            "value":"奖金自动派发,恭喜中奖"
                        }
                    ]
                },
                {
                    "items":[
                        {
                            "labelZh":"派奖地址",
                            "labelEn":"Award address",
                            "value":"WcQT5eoJPGH1B7TxX3BuPwGEZE6Lt7ePbi"
                        },
                        {
                            "labelZh":"开奖类型",
                            "labelEn":"Lottery type",
                            "value":"单双"
                        },
                        {
                            "labelZh":"说明",
                            "labelEn":"intro",
                            "value":"奖金自动派发,恭喜中奖"
                        }
                    ]
                }
            ]
        },
        {
            "labelZh":"备注信息",
            "labelEn":"remark",
            "value":"这是备注信息"
        }
    ]
}
```

