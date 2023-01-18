# FEIP3V4_CID(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Register](#register)

[Unregister](#unregister)

## Summary

```

Protocol type: FEIP
Serial number: 3
Protocol name: CID
Version: 4
Description : Register or unregister a human friendly identity for an address.
Author: C_armX, Deisler-JJ_Sboy，Free_Cash
Language: en-US
Created date: 2021-02-5
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

1. CID(Crypto Identity)：“name"+"_"+suffix, e.g. CY_vpAv.

2. Suffix：The last four letters of the address. If the new CID is the same as any CID that has been registered, increase the length of suffix until the new CID is unique, e.g. CY_kvpAv.

3. When an address registers a new cid, its previous cid is automatically unregistered.

4. Once a CID is registered by an address, it cannot be registered by other addresses, even if the CID has been unregistered.

5. An address can re-register its unregistered CID.

6. If address had actively unregistered its cid, it is unregistered untile it register again.

7. One address can only occupy up to 4 CIDs.

## Register

The OP_RETURN of which contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 3|Y|
|3|ver|int|Fixed: 4|Y|
|4|name|String|Fixed: "CID"|N|
|5|pid|hex|The PID of this protocol|N|
|6|data.op|string|operation: "register"|Y|
|7|data.name|string|Nick name given by the user|Y when operation is register,<br>N when operation is unregister|


* Example of registering a CID

```

{
    "type": "FEIP",
    "sn": 3,
    "ver": 4,
    "name": "CID",
    "pid": "",
    "data":{
        "op": "register",
        "name": "CY"
        }
}
```
## Unregister

The OP_RETURN of which contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 3|Y|
|3|ver|int|Fixed: 4|Y|
|4|name|String|Fixed: "CID"|N|
|5|pid|hex|The PID of this protocol|N|
|6|data.op|string|operation: "unregister"|Y|

* Example of unregistering a CID

```

{
    "type": "FEIP",
    "sn": 3,
    "ver": 4,
    "name": "CID",
    "pid": "",
    "data":{
        "op": "unregister"
        }
}
```