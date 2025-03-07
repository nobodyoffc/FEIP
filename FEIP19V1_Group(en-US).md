# FEIP19V1_Group(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Create](#create)

[Update](#update)

[Join](#join)

[Leave](#leave)

## Summary

```
Protocol type: FEIP
Serial number: 19
Protocol name: Group
Version: 1
Description :  Public group protocol
Author: C_armX, Deisler-JJ_Sboy
Language: en-US
Previous version PID:""
Created date: 2021-04-03
Last modified date：2023-01-02
```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. Everyone can create groups. The creator has no privilege.
2. The hash of the creating transaction is the group id, called gid.
3. Group information includes name, description, tCDD.
4. Any active member of a group can update the 'name' and 'desc' of the group with consuming CoinDays more than last update operation consumed.
5. Everyone can join any group or leave any group it belonging to.
6. All CDD of operations will be added to 'tCdd' which indicates the hot of the group.
7. Since block height `2000000`, creating group needs more than `100cd` consumed.


## Create
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 19|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Group"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|String|operation: "create"|Y|
|7|data.name|String|Group name|Y|
|8|data.desc|String|Description of this group|Y|

* Example of creating a group

```
{
    "type": "FEIP",
    "sn": 19,
    "ver": 1,
    "name": "Group",
    "pid": "",
    "data":{
        "op": "create",
        "name": "test",
        "desc": "This is a test group"
    }
}
```

## Update
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 19|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Group"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|String|operation: "update"|Y|
|7|data.gid|string|Group ID|Y|
|8|data.name|String|Group name|Y|
|9|data.desc|String|Description of this group|Y|

* Example of updating group information

```
{
    "type": "FEIP",
    "sn": 19,
    "ver": 1,
    "name": "Group",
    "pid": "",
    "data":{
        "op": "update",
        "gid": "6305d16c89fb98763b6968049096a984eea9334e12c514c9b72098c3f332d114",
        "name": "test1",
        "desc": "This is an updating test."
    }
}
```

## Join

Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 19|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Group"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|String|operation: "join"|Y|
|7|data.gid|String|Group ID|Y|

* Example of joining in a group

```
{
    "type": "FEIP",
    "sn": 19,
    "ver": 1,
    "name": "Group",
    "pid": "",
    "data":{
        "op": "join",
        "gid": "6305d16c89fb98763b6968049096a984eea9334e12c514c9b72098c3f332d114"
    }
}
```

## Leave

Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 19|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Group"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|String|operation: "leave"|Y|
|7|data.gid|String|Group ID|Y|

* Example of leaving a group

```
{
    "type": "FEIP",
    "sn": 19,
    "ver": 1,
    "name": "Group",
    "pid": "",
    "data":{
        "op": "leave",
        "gid": "6305d16c89fb98763b6968049096a984eea9334e12c514c9b72098c3f332d114"
    }
}
```
## QR code

The QR code of a published group has fields as following:

```
{
	"meta":"FC",
    "type": "FEIP",
    "sn": 19,
    "ver": 1,
    "data":{
        "name": "test",
        "gid": "6305d16c89fb98763b6968049096a984eea9334e12c514c9b72098c3f332d114"
    }
}
```