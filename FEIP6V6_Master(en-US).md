# FEIP6V6_Master(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Authorize](#authorize)



## Summary

```
Protocol type: FEIP
Serial number: 6
Protocol name: Master
Version number: 6
Description : An address authorize another address as its master.
Author: C_armX
Language: en-US
Created date: 2021-06-23
Last modified date：2023-01-13

```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. Once the master address is authorized, it can control all the rights and interests of the original address.

2. An address can't authorize itself.

3. The authorization can't be canceled.

4. An address can only implement a valid master authorization once, and subsequent authorizations are invalid.


## Authorize

The OP_RETURN of which contains the data as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Fixed: 6|Y|
|3|ver|int|Fixed: 6|Y|
|4|name|string|Fixed: "Master"|Y|
|5|pid|hex|The PID of this protocol|N|
|6|data.master|string|The address designated as the master.|Y|
|7|data.promise|string|Fixed:"The master owns all my rights."|Y|

## Example of a authorization

```
{
    "type": "FEIP",
    "sn": 6,
    "ver": 6,
    "name": "Master",
    "pid": ""
    "data":{
        "master":"FTqiqAyXHnK7uDTXzMap3acvqADK4ZGzts",
        "promise":"The master owns all my rights."
        }
}
```