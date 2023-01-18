# FEIP9V1_Homepage(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[consensus of this protocol](#consensus-of-this-protocol)

[Register](#register)

[Unregister](#unregister)



## Summary

```

Protocol type: FEIP
Serial number: 9
Protocol name: Homepage
Version: 1
Description : Register home pages path on the freecash blockchain for an address.
Author: C_armX
Language: en-US
Created date: 2022-10-02
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

1. An address can register some URLs as its homepages with writing in op_return.

2. The homepages are only registered for the address of the first input.

3. When new homepages are registered, all the old ones are automatically unregistered.



## register

The OP_RETURN of which contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 9|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Homepage"<|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|string|operation: "register" |Y|
|7|data.homepages|string array|512|URLs of the homepages|Y|

* Example

```

{
    "type": "FEIP",
    "sn": 9,
    "ver": 1,
    "name": "Homepage",
    "pid": "",
    "data":{
        "op": "register",
        "homepages": ["https://cid.cash/html/others/cid.html?cid=CY_vpAv","100.100.100.100:9300"]
        }
}
```
## Unregister

The OP_RETURN of which contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 9|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Homepage"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|string|operation: "unregister" |Y|

* Example

```
{
    "type": "FEIP",
    "sn": 9,
    "ver": 1,
    "name": "Homepage",
    "pid": "",
    "data":{
        "op": "unregister"
        }
}
```