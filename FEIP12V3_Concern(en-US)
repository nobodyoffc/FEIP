# FEIP12V3_Concern(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Add](#add)

[Delete](#delete)

[Recover](#recover)

## Summary

```
Protocol type: FEIP
Serial number: 12
Protocol name: Concern
Version: 3
Description :  Store the encrypted information of addresses you concerning on freecash blockchain.
Author: C_armX
Language: en-US
Created date: 2021-05-21
Last modified date：2023-01-11
```
## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. Store the encrypted information of addresses you concerning on freecash blockchain.

2. Use the public key of the first input address to encrypt the message. 

3. The concerning belongs to the first input address of the transaction that added it.

4. Only the address who added the concerning can delete or recover it.

5. When updating an concerning, just delete it and add a new one.

## Add

When user adds a concerning, the OP_RETURN contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 12|Y|
|3|ver|int|Fixed: 3|Y|
|4|name|String|Fixed: "Concern"|N|
|5|pid|hex|The PID of this protocol|N|
|6|data.op|string|operation: "add"|Y|
|7|data.alg|string|The encrypt algorithm. "ECC256k1-AES256CBC" is recommended.|Y|
|8|data.ciphertext|string|Encrypted message|Y|

### Decrypted data of data.ciphertext

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|address|string|The FCH address of the concerning|Y|
|2|note|string|Notes to this concerning|N|
|3|noNoticeFee|boolean|Accept all notices from it regardless notice fee(see FEIP27).|N|
|4|seeStatement|boolean|Whether see its statement(see FEIP8).|N|
|5|seeWorks|boolean|Whether see its statement(see FEIP8).|N|

### Example for adding an item
```
The address adding or updating a concerning: FEk41Kqjar45fLDriztUDTUkdki7mmcjWK
Publickey: 6vU3ZMpwggurw92AUy1Vi6WBxEnBPdjupXGKD7Q5Zcw8yvdJAf
Privatekey: L2bHRej6Fxxipvb4TiR5bu1rkT3tRp8yWEsUy4R1Zb8VMm2x7sd8

OP_RETURN content:

{
    "type": "FEIP",
    "sn": 12,
    "ver": 3,
    "name": "Concern",
    "pid": "",
    "data":{
        "op": "add",
        "alg": "ECC256k1-AES256CBC",
        "ciphertext": "AtrFmWIFjVgOAiJV9ecB0V8vpWeGbc8nZwmJUFjan5zfnLQWLl0NH5Sjh/qWBU849x8yTpn7v6V0Hgdm2zuNGk4flfU2wyYBG2sRFlmagSLZNJQ8T/meD3FX3EGXken+bbG9P6MmSWqWZsAqnx/MtIu/ngXy/+TB6UyyvH3/e1rvPzqfrNKpzVRpcfycUFSlHmm4xU15DA/SZu01PYDUI+AR/x2poKftABu7CxQinEp8bWBARYiDkvsplJLl7h+RJDtg5UgZZlAqG03GdgvmDEWkhHDuYrCKbWpoCILeEilW"
        }
}

Decrypted data of data.ciphertext:

{
    "address": "F86zoAvNaQxEuYyvQssV5WxEzapNaiDtTW",
    "note": "A public test address.",
    "noNoticeFee": true,
    "seeStatement": true,
    "seeWorks": true
}
```

## Delete
When user deletes a concerning, the OP_RETURN contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 12|Y|
|3|ver|int|Fixed: 3|Y|
|4|name|String|Fixed: "Concern"|N|
|5|pid|hex|The PID of this protocol|N|
|6|data.op|string|operation: "delete"|Y|
|7|data.addTxid|string|The txid in which the concerning was added.|Y|


### Example for deleting an item
```
{
    "type": "FEIP",
    "sn": 12,
    "ver": 3,
    "name": "Concern",
    "pid": "",
    "data":{
        "op": "delete",
        "addTxid": "c0a5aacdead28266b5d2d2c0f1baa90df3b56e35293d381681b002e73066c14a"
        }
}
```

## Recover
When user recovers a deleted concerning, the OP_RETURN contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 12|Y|
|3|ver|int|Fixed: 3|Y|
|4|name|String|Fixed: "Concern"|N|
|5|pid|hex|The PID of this protocol|N|
|6|data.op|string|operation: "recover"|Y|
|7|data.addTxid|string|The txid in which the concerning was added.|Y|

### Example for recovering an item
```
{
    "type": "FEIP",
    "sn": 12,
    "ver": 3,
    "name": "Concern",
    "pid": "",
    "data":{
        "op": "recover",
        "addTxid": "c0a5aacdead28266b5d2d2c0f1baa90df3b56e35293d381681b002e73066c14a"
        }
}
```