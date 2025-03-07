# FEIP7V4_Mail(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Send](#send)

[Delete](#delete)

[Recover](#recover)

## Summary

```
Protocol type: FEIP
Serial number: 7
Protocol name: Mail
Version: 4
Description: Send encrypted mail via the blockchain of FCH.
Author: C_armX, Deisler-JJ_Sboy
Language: en-US
Created date: 2021-04-03
Last modified date：2023-01-01
```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. This protocol is used to send p2p encrypted mail via the blockchain of FCH.

2. MID（Mail Identity）: The txid of the transaction in which the mail was sent.

3. The sender is the first input address. The recipient is the first output address.

4. Only the recipient of a mail can delete or recover the mail, not the sender.

## Send

When user sends a new item, the OP_RETURN contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number Fixed: 7|Y|
|3|ver|int|Fixed: 4|Y|
|4|name|String|Fixed: "Mail"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|string|fixed:"send"|Y|
|7|data.alg|string|The encrypt algorithm. "ECC256k1-AES256CBC" is recommended.|Y|
|8|data.ciphertextSend|string|mail encrypted with the public key of the sender|Y|
|9|data.ciphertextReci|string|mail encrypted with the public key of the recipient|Y|
|10|data.textHash|string|Double sha256 hash value of the plain text of the mail|Y|

* Example for sending a mail

Sender:
The address of first input: FEk41Kqjar45fLDriztUDTUkdki7mmcjWK
Public key: 6vU3ZMpwggurw92AUy1Vi6WBxEnBPdjupXGKD7Q5Zcw8yvdJAf
Private key: L2bHRej6Fxxipvb4TiR5bu1rkT3tRp8yWEsUy4R1Zb8VMm2x7sd8

Recipient:
The address of first output: F86zoAvNaQxEuYyvQssV5WxEzapNaiDtTW
Public key: 5XEV28ubXGx9Y3sLgQCUvbbE8bJe4tB3xWvY4HanoQeGxmbn3x
Private key: L5DDxf3PkFwi1jArqYokpTsntthLvhDYg44FXyTSgdTx3XEFR1iB

OP_RETURN content:

```
{
	"type": "FEIP",
	"sn": 7,
	"ver": 4,
	"name": "Mail",
	"pid": "",
	"data": {
		"op": "send",
		"alg": "ECC256k1-AES256CBC",
		"ciphertextSend": "AtpfHV/b+P2TMG8eFYpwdR7FCOhRjsK5I9UiZBIwUBtBAzi+B/tS5lMv27T9ofORhmSTYRYEfMIaffEwkwtfgEdormE5u4YKnBf001bWPeIQIXEz8HAj7XVt1rYLciJPdQ==",
		"ciphertextReci": "AtmUPEcIAg3WLfUE+QCCIx8JfwcPLwSAGv6C88yNtXhoO5vPLybsdr9fr5mHXlsK5146tBtgMdX+iB5tme1LAapJAvDjf4MYT7/PnkPBYxuS/Vl0EvoOzIcevf/NfFKEXg==",
		"textHash":"c269f3b03960a695e1e2e4ab451bd645c7134d3328a327c98c00ed102f76b451"
	}
}
```
## Delete

When the sender deletes a mail, the OP_RETURN contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number Fixed: 7|Y|
|3|ver|int|Fixed: 4|Y|
|4|name|String|Fixed: "Mail"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|string|fixed:"delete"|Y|
|7|data.sendTxid|string|The txid in which the mail was sent.|Y|

* Example for deleting a mail

```
{
    "type": "FEIP",
    "sn": 7,
    "ver": 4,
    "name": "Mail",
    "pid": "",
    "data":{
        "op": "delete",
        "sendTxid": "06d457c0012ae309290abe5f0e5cb485263150c504945503a661fb47ee07c9fa"
    }
}
```

## Recover

When the sender recovers a deleted item, the OP_RETURN contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number Fixed: 7|Y|
|3|ver|int|Fixed: 4|Y|
|4|name|String|Fixed: "Mail"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.op|string|fixed:"recover"|Y|
|7|data.sendTxid|string|The txid in which the mail was sent.|Y|

* Example for recovering a mail

```
{
    "type": "FEIP",
    "sn": 7,
    "ver": 4,
    "name": "Mail",
    "pid": "",
    "data":{
        "op": "recover",
        "sendTxid": "06d457c0012ae309290abe5f0e5cb485263150c504945503a661fb47ee07c9fa"
    }
}
```