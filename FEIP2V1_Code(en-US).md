# FEIP2V1_Code(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[consensus of this protocol](#consensus-of-this-protocol)

[Publish](#publish)

[Update](#update)

[Stop](#stop)

[Recover](#recover)

[Close](#close)

[Rate](#rate)

[QR code](#qr-code)

## Summary

```
Protocol type: FEIP
Serial number: 2
Protocol name: code
Version: 1
Description : Publish program code information on Freecash blockchain. 
Author: C_armX
Language: en-US
Published date: 2022-12-28
Last modified date：2023-01-10
```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. This protocol is used to publish program code information on Freecash blockchain. 

2. COID（Code Identity）: The txid when publishing the code is the identity of the code。

3. The publisher can name the code freely. We can take "[name]"+"@"+"[cid/address of the publisher]" to refer to the code, such as "FreeChain@No1_NrC7".

4. The publisher should ensure its different codes having different name.

5. One can only update, stop, or recover its own published code.

6. The publisher can't rate its own code.

7. Stopped or closed codes still can be rated.

8. Owner or Owner's master (see FEIP6_Master) can [Close](#close) the code and giving a `closeStatement`.

9. A closed code can never be operated again.

## Publish
The publisher sends a tx with the content op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 2|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Code"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|string|Operation: "publish"|Y|
|7|data.name|string|The name of the code in english|Y|
|8|data.version|string|The version of the code in english|Y|
|9|data.hash|string|The hash of this version code file|Y|
|10|data.desc|string|Description of this code|N|
|11|data.langs|string array|The program languages of the code|N|
|12|data.urls|string array|URLs，the locations to get the code|N|
|13|data.protocols|string array|The protocols followed by this code|N|
|14|data.pubKeyAdmin|hex|The public key of the FCH identity designated by the publisher of this code|

*  Example of publishing a code
```
{
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "name": "Code",
    "pid": "",
    "data":{
        "op":"publish",
        "name": "FreeChain",
		"version": "1",
		"hash": "fd3f2096a6f2c83cfada751db524d978f798b7a1530c212e0fd9c27a10bf85fb",
        "desc": "The code to parse basic information from the freecash blockchain.",
		"langs":["java"],
        "urls": ["https://github.com/nobodyoffc/FreeChain.git"],
        "protocols":["b1674191a88ec5cdd733e4240a81803105dc412d6c6708d53ab94fc248f4f553","37406e3e45750efccdb060ca2e748f9f026aebb7dadade8e8747340f380edaca"],
        "pubKeyAdmin": "02966dc682850550b1df046f2a03cfe546c4e4cf83f739d1497f6c292fabdad1b4"
    }
}
```
The txid is : 28e85989ce7e3bba56c8179d6dd9b180b23ff48a4ad031c72539d02750659212. It's also coid of the published code。


## Update

The publisher of a code updates the code information. All fields will be replaced together.

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|2erial number<br>Fixed: 2|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Code"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.coid|hex|Txid when the code was published|Y|
|7|data.op|string|Operation: "update"|Y|
|8|data.name|string|he name of the code in english|Y|
|9|data.version|string|he version of the code in english|Y|
|10|data.hash|string|The hash of this version code file|Y|
|11|data.desc|string|Description of this code|N|
|12|data.langs|string array|The program languages of the code|N|
|13|data.urls|string array|URLs，the locations to get the code|N|
|14|data.protocols|string array|The protocols followed by this code|N|
|15|data.pubKeyAdmin|hex|The public key of the FCH identity designated by the publisher of this code|

*  Example of updating a code

```
{
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "name": "Code",
    "pid": "",
    "data":{
        "coid": "b8e1a19eadb3f7a639bddf360fe253226439c32197c81242b69a1ca390f87151",
        "op":"update",
        "name": "FreeChain",
		"version": "2",
		"hash": "fd3f2096a6f2c83cfada751db524d978f798b7a1530c212e0fd9c27a10bf85fb",
        "desc": "The code to parse basic information from the freecash blockchain.",
		"langs":["java"],
        "urls": ["https://github.com/nobodyoffc/FreeChain.git"],
        "protocols":[],
        "pubKeyAdmin": "03c731d4db424e15920bf801e73a81a0e33c36391ab00c88ae95cc3e8fae5ed101"
    }
}
```

## Stop

The owner stops maintaining a code as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 2|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Code"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.coid|hex|Txid when the code was published|Y|
|7|data.op|string|Operation: "stop"|Y|

*  Example of stoping a code

```
{
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "name": "Code",
    "pid": "",
    "data":{
        "coid": "b8e1a19eadb3f7a639bddf360fe253226439c32197c81242b69a1ca390f87151",
        "op":"stop"
    }
}
```
## Recover

The owner recovers a Stopped code as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 2|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Code"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.coid|hex|Txid when the code was published|Y|
|7|data.op|string|Operation: "recover"|Y|

*  Example of stoping a code

```
{
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "name": "Code",
    "pid": "",
    "data":{
        "coid": "b8e1a19eadb3f7a639bddf360fe253226439c32197c81242b69a1ca390f87151",
        "op":"recover"
    }
}
```

## Close

The owner or its master close a code permanently, the OP_RETURN of which contains the data as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 2|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Code"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.coid|hex|Txid when the code was published|Y|
|7|data.op|string|Operation: "close"|Y|

* Example of closing a code:

```
{
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "name": "Code",
    "pid": "",
    "data":{
        "coid": "b8e1a19eadb3f7a639bddf360fe253226439c32197c81242b69a1ca390f87151",
        "op":"close"
    }
}
```

## Rate

Anyone but the owner rate a published code as following:


|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 2|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Code"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.coid|hex|Txid when the code was published|Y|
|7|data.op|string|operation: "rate"|Y|
|8|data.rate|int|Score of rating from 0 to 5|N|

*  Example of rate a code

```
{
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "name": "Code",
    "pid": "",
    "data":{
        "coid": "b8e1a19eadb3f7a639bddf360fe253226439c32197c81242b69a1ca390f87151",
        "op": "rate",
        "rate": 4
    }
}
```

## QR code

The QR code of a published code has fields as following:

```
{
    "meta":"FC",
    "type": "FEIP",
    "sn": 2,
    "ver": 1,
    "data":{
        "coid": "b8e1a19eadb3f7a639bddf360fe253226439c32197c81242b69a1ca390f87151"
    }
}
```