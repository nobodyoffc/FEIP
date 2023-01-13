# FEIP1V7_FreeProtocol(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Publish](#publish)

[Update](#update)

[Stop](#stop)

[Recover](#recover)

[Rate](#rate)

[QR code](#qr-code)

## Summary

```
Protocol type: FEIP
Serial number: 1
Protocol name: FreeProtocol
Version number: 7
Description : Formulate the consensuses of freely publishing and managing protocols on the blockchain of freecash.
Author: C_armX, Deisler-JJ_Sboy
Language: en-US
Created date: 2022-02-01
Update date：2023-01-10
```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. This protocol is used to publish protocol information on Freecash blockchain. 

2. PID（Protocol Identity）: The txid when publishing the protocol is the identity of the protocol。

3. One can send a transaction to publish a protocol by writing data in the format given by [Publish](#publish) .

4. The title of protocol published on chain is in following format: 

`type + serial number + 'V' + version number +'_' + protocol name + '(' + language + ')'`

e.g: `FEIP3V4_CID(en-US)`

5. One can only update its own protocols by writing data in the format given by [Update](#update).

6. One can only stop its own protocols by writing data in the format given by [Stop](#stop).

7. One can rate other's protocols by writing data in the format given by [Rate](#rate).

8. One can't rate its own protocols.

9. Stopped  or closed protocols still can be rated.

10. If the consensus of a new version is not compatible with the previous version, a new protocol should be released instead of a new version, and the old pid should be given in prePid.

11. Owner or Owner's master (see FEIP6_Master) can [Close](#close) the protocol and giving a closing statement.

12. A closed protocol can never be operated again.

## Publish

Publish a protocol by send a transaction, the OP_RETURN of which contains the data as following:


|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP". Case insensitive|Y|
|2|sn|int|Fixed: 1|Y|
|3|ver|int|Fixed: 7|Y|
|4|name|String|Fixed:"FreeProtocol". Case insensitive|N|
|5|pid|string|The PID of this protocol|N|
|6|data.type|String|Type of the protocol being published.|N|
|7|data.sn|String|Serial number of the protocol being published, counting from 1.|N|
|8|data.ver|String|Version number of the protocol being published. counting from 1.|N|
|9|data.name|String|Name of the protocol being published.|N|
|10|data.desc|String|Short description of the protocol being published.|N|
|11|data.authors|String array|FCH addresses of the authors of the protocol being published.|N|
|12|data.hash|string|The double sha256 value of this version protocol file being published.|Y|
|13|data.lang|String|The language of the protocol being published, formatted with i18n(internationalization),such as "en-US".|N|
|14|data.op|String|Fixed: "publish"|Y|
|15|data.prePid|string|The double sha256 value of the previous protocol file.|N|
|16|data.fileUrls|String array|Locations to find the protocol file.|N|

**Publish Example:**

```
{
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "name": "FreeProtocol",
    "pid": "",
    "data": {
        "type": "FEIP",
        "sn": "3",
        "ver": "4",
        "name": "CID",
        "desc": "Register or unregister a human friendly identity for an address.",
        "authors": ["FPL44YJRwPdd2ipziFvqq6y2tw4VnVvpAv","FS2AWq1dgdhCpNTwqfBbMBBJGNNj1LSboy","FLx88wdsbLQyZRmbqtpeXA9u5FG9EyCash"],
        "hash": "1403e5b7100d8e24724f12cd1ea0b722086c02250a7c66b711947f14546cfcfd",
        "lang": "zh-CN",
        "op": "publish",
        "prePid":"",
        "fileUrls": ["https://github.com/freecashorg/FEIP/FEIP3_CID/"]
    }
}
```
## Update
Update a protocol by send a transaction. All field contents will be updated.

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP". Case insensitive|Y|
|2|sn|int|Fixed: 1|Y|
|3|ver|int|Fixed: 7|Y|
|4|name|String|Fixed:"FreeProtocol". Case insensitive|N|
|5|pid|string|The PID of this protocol|N|
|6|data.type|String|Type of the protocol being published.|N|
|7|data.sn|String|Serial number of the protocol being published, counting from 1.|N|
|8|data.ver|String|Version number of the protocol being published. counting from 1.|N|
|9|data.hash|string|The double sha256 value of this version protocol file being published.|Y|
|10|data.name|String|Name of the protocol being published.|N|
|11|data.desc|String|Short description of the protocol being published.|N|
|12|data.authors|String array|FCH addresses of the authors of the protocol being published.|N|
|13|data.pid|string|The txid in which the protocol was published.|Y|
|14|data.lang|String|The language of the protocol being published, formatted with i18n(internationalization),such as "en-US".|N|
|15|data.op|String|Fixed: "update"|Y|
|16|data.prePid|string|Double sha256 value of  the previous version protocol file.|N|
|17|data.fileUrls|String array|Locations to find the protocol file.|N|

**Update Example:**

```
{
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "name": "FreeProtocol",
    "pid": "",
    "data": {
        "type": "FEIP",
        "sn": "3",
        "ver": "4",
		"hash": "1403e5b7100d8e24724f12cd1ea0b722086c02250a7c66b711947f14546cfcfd",
        "name": "CID",
        "desc": "Register, update or unregister a human friendly identity for an address.",
        "authors": ["FPL44YJRwPdd2ipziFvqq6y2tw4VnVvpAv","FS2AWq1dgdhCpNTwqfBbMBBJGNNj1LSboy","FLx88wdsbLQyZRmbqtpeXA9u5FG9EyCash"],
        "pid": "c50d307c3ac0c193dad6c671ad3cebb881c01c747e03abfeaecc378419739ff4",
        "lang": "en-US",
        "op": "update",
        "prePid":"",
        "fileUrls": ["https://github.com/freecashorg/FEIP/FEIP3_CID/"]
    }
}
```
## Stop
Stop a protocol by send a transaction, the OP_RETURN of which contains the data as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP". Case insensitive|Y|
|2|sn|int|Fixed: 1|Y|
|3|ver|int|Fixed: 7|Y|
|4|name|String|Fixed:"FreeProtocol". Case insensitive|N|
|5|pid|string|The PID of this protocol|N|
|6|data.pid|string|The txid in which the protocol was published.|Y|
|7|data.op|String|Fixed: "stop"|Y|

**Stop Example:**

```
{
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "name": "FreeProtocol",
    "pid": "",
    "data": {
        "pid": "c50d307c3ac0c193dad6c671ad3cebb881c01c747e03abfeaecc378419739ff4",
        "op": "stop"
    }
}

```
## Recover

Recover a protocol by send a transaction, the OP_RETURN of which contains the data as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP". Case insensitive|Y|
|2|sn|int|Fixed: 1|Y|
|3|ver|int|Fixed: 7|Y|
|4|name|String|Fixed:"FreeProtocol". Case insensitive|N|
|5|pid|string|The PID of this protocol|N|
|6|data.pid|string|The txid in which the protocol was published.|Y|
|7|data.op|String|Fixed: "recover"|Y|

**Recover Example:**

```
{
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "name": "FreeProtocol",
    "pid": "",
    "data": {
        "pid": "c50d307c3ac0c193dad6c671ad3cebb881c01c747e03abfeaecc378419739ff4",
        "op": "recover"
    }
}

```

## Close

The owner or its master close a protocol permanently, the OP_RETURN of which contains the data as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP". Case insensitive|Y|
|2|sn|int|Fixed: 1|Y|
|3|ver|int|Fixed: 7|Y|
|4|name|String|Fixed:"FreeProtocol". Case insensitive|N|
|5|pid|string|The PID of this protocol|N|
|6|data.pid|string|The txid in which the protocol was published.|Y|
|7|data.op|String|Fixed: "close"|Y|
|8|data.closeStatement|String|Fixed: "close"|Y|

**Stop Example:**

```
{
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "name": "FreeProtocol",
    "pid": "",
    "data": {
        "pid": "c50d307c3ac0c193dad6c671ad3cebb881c01c747e03abfeaecc378419739ff4",
        "op": "stop"
    }
}

```

## Rate

Evaluate a protocol by send a transaction, the OP_RETURN of which contains the data as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP". Case insensitive|Y|
|2|sn|int|Fixed: 1|Y|
|3|ver|int|Fixed: 7|Y|
|4|name|String|Fixed:"FreeProtocol". Case insensitive|N|
|5|pid|string|The PID of this protocol|N|
|6|data.pid|string|The txid in which the protocol was published.|Y|
|7|data.op|String|Fixed: "rate"|Y|
|8|data.rate|int|Score of rating from 0 to 5|Y|

**Rate Example:**

```
{
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "name": "FreeProtocol",
    "pid": "",
    "data": {
        "pid": "c50d307c3ac0c193dad6c671ad3cebb881c01c747e03abfeaecc378419739ff4",
        "op": "rate",
        "rate": 5
    }
}
```
## QR code

The QR code of a published protocol has fields as following:

```
{
    "meta":"FC",
    "type": "FEIP",
    "sn": 1,
    "ver": 7,
    "data":{
        "pid": "c50d307c3ac0c193dad6c671ad3cebb881c01c747e03abfeaecc378419739ff4"
    }
}
```