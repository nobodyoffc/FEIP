# FEIP15V1_APP(en-US)

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
Serial number: 15
Protocol name: APP
Version: 1
Description :  Publish and manage APPs on freecash blockchain.
Author: C_armX
Language: en-US
Published date: 2021-11-01
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

1. This protocol is used to publish and manage APPs on freecash blockchain.

2. AID（Application Identity）: The txid of the transaction in which the APP was published.

3. The publisher can name the APP freely. We can take "[stdName]"+"@"+"[cid/address of the publisher]" to refer to the APP, such as "FreeSign@Free_cash".

4. The publisher should ensure its different APPs having different stdName.

5. One can only update, stop, or recover its own published APP.

6. The publisher can't rate its own APP.

7. Stopped or closed APPs still can be rated.

8. Owner or Owner's master (see FEIP6_Master) can [Close](#close) the APP and giving a `closeStatement`.

9. A closed APP can never be operated again.

## Publish

The publisher send a tx with the content op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 15|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "APP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|string|Operation: "publish"|Y|
|7|data.stdName|string|he name of the APP in English|Y|
|8|data.localNames|string array|APP names in different languages|N|
|9|data.desc|string|Description of this APP|N|
|10|data.types|string array|The types of the App|N|
|11|data.urls|string array|URLs，the locations to get the APP|N|
|12|data.downloads|object array|Download information: os,link,hash. see below.|N|
|13|data.protocols|string array|The PIDs of the protocols followed by this APP|N|
|14|data.services|string array|The SIDs of the services used by this APP|N|
|15|data.codes|string array|The COIDs of the codes used by this APP|N|
|16|data.pubKeyAdmin|hex|The public key of the FCH identity designated by the publisher of this APP|


data.downloads:
|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|os|String|The name of OS|N|
|2|link|String|The link to download installing file|N|
|3|hash|String|Double SHA256 hash value of the installing file|N|


* Example of creating an APP

```
{
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "name": "APP",
    "pid": "",
    "data":{
        "op":"publish",
        "stdName": "Signer",
        "localNames": ["飞签","フライング宝くじ"],
        "desc": "Save the private key offline and provide offline signature，and provide other functions.",
		"types":["signer"],
        "urls": ["https://sign.cash"],
        "downloads":[{
			"os":"android",
			"link":"https://sign.cash/download/cryptosigner",
			"hash":"2d45fda951ed5c6d621a38266e327ed64e6e582e83ddb8dba3f243caeecdaa8e"
		}
		],
        "protocols":["b1674191a88ec5cdd733e4240a81803105dc412d6c6708d53ab94fc248f4f553","37406e3e45750efccdb060ca2e748f9f026aebb7dadade8e8747340f380edaca"],
        "services":["c86e039f466434862585e38c0fd1a11f47dcc07839647a452424503b30f81b39","403d3146bdd1edbd8d71b01ffbad75972e07617971acb767a9bae150d4154dc25"],
        "codes":[""],
        "pubKeyAdmin": "02966dc682850550b1df046f2a03cfe546c4e4cf83f739d1497f6c292fabdad1b4"
    }
}
```
The txid is "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2". It's AID of this APP。

## update

The publisher of an APP can update the APP information. All fields will be replaced together.

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|2erial number<br>Fixed: 15|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|4ixed: "APP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.aid|hex|Txid when publishing the APP|Y|
|7|data.op|string|Operation: "update"|Y|
|8|data.stdName|string|the name of the APP in English|Y|
|9|data.localNames|string array|APP names in different languages|N|
|10|data.desc|string|Description of this APP|N|
|11|data.types|string array|The types of the App|N|
|12|data.urls|string array|URLs，the locations to get the APP|N|
|13|data.downloads|object array|Download information: os,link,hash. see below.|N|
|14|data.protocols|string array|The PIDs of the protocols followed by this APP|N|
|15|data.services|string array|The SIDs of the services used by this APP|N|
|16|data.codes|string array|The COIDs of the codes used by this APP|N|
|17|data.pubKeyAdmin|hex|The public key of the FCH identity designated by the publisher of this APP|

data.downloads:
|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|os|String|The name of OS|N|
|2|link|String|The link to download installing file|N|
|3|hash|String|Double SHA256 hash value of the installing file|N|

* Example of updating an APP

```
{
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "name": "APP",
    "pid": "",
    "data":{
        "aid": "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2",
        "op":"update",
        "stdName": "Crypto Signer",
        "localNames": ["密签","秘密のサイン"],
		"types":["construct","signer"],
        "desc": "Save the private key offline and provide offline signature，and provide other functions.",
        "urls": ["https://sign.cash"],
        "downloads":[{
			"os":"android",
			"link":"https://sign.cash/download/cryptosigner",
			"hash":"2d45fda951ed5c6d621a38266e327ed64e6e582e83ddb8dba3f243caeecdaa8e"
		}
		],
        "protocols":["b1674191a88ec5cdd733e4240a81803105dc412d6c6708d53ab94fc248f4f553","37406e3e45750efccdb060ca2e748f9f026aebb7dadade8e8747340f380edaca"],
        "services":["c86e039f466434862585e38c0fd1a11f47dcc07839647a452424503b30f81b39","403d3146bdd1edbd8d71b01ffbad75972e07617971acb767a9bae150d4154dc25"],
		"codes":[""],
        "pubKeyAdmin": "02966dc682850550b1df046f2a03cfe546c4e4cf83f739d1497f6c292fabdad1b4"
    }
}
```

## Stop

The owner can stop maintaining an APP as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 15|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "APP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.aid|hex|Txid when publishing the APP|Y|
|7|data.op|string|Operation: "stop"|Y|

* Example of stoping an APP

```
{
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "name": "APP",
    "pid": "",
    "data":{
        "aid": "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2",
        "op":"stop"
    }
}
```
## Recover

The owner can recover a Stopped APP as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 15|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "APP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.aid|hex|Txid when publishing the APP|Y|
|7|data.op|string|Operation: "recover"|Y|

* Example of stoping an APP

```
{
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "name": "APP",
    "pid": "",
    "data":{
        "aid": "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2",
        "op":"recover"
    }
}
```

## Close

The owner or its master can close a Stopped APP as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 15|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "APP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.aid|hex|Txid when publishing the APP|Y|
|7|data.op|string|Operation: "close"|Y|

### Example of stoping an APP

```
{
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "name": "APP",
    "pid": "",
    "data":{
        "aid": "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2",
        "op":"close"
    }
}
```

## Rate

Anyone but the owner can rate a published APP.


|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number<br>Fixed: 15|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "APP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.aid|hex|Txid when publishing the APP|Y|
|7|data.op|string|operation: "rate"|Y|
|8|data.rate|int|Score of rating from 0 to 5|N|

### Example of rate an APP
```
{
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "name": "APP",
    "pid": "",
    "data":{
        "aid": "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2",
        "op": "rate",
        "rate": 4
    }
}
```

## QR code

The QR code of a published APP has fields as following:

```
{
    "meta":"FC",
    "type": "FEIP",
    "sn": 15,
    "ver": 1,
    "data":{
        "aid": "b7af2d8c32e8c46159af450226065c898d473321b4096a14ca293c0f86888ee2"
    }
}
```