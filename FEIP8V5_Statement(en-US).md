# FEIP8V5_Statement(en-US)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Publish a statement](#publish-a-statement)

## Summary

```
Protocol type: FEIP
Serial number: 8
Protocol name: Statement
Version: 5
Description : Make statements on chain.
Author: C_armX
Language: en-US
Create: 2021-06-18
Update：2023-01-13

```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.

## Consensus of this protocol

1. Anyone can sign and publish an on-chain statement.

2. STID(statement identity):Take the txid where the statement was published as the ID of this statement.

3. The content of the statement is that the signer clearly agrees and is willing to bear corresponding responsibilities.

4. Once a statement published, it can't be deleted or updated.

## Publish a statement

The OP_RETURN of which contains the data as follows:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 8|Y|
|3|ver|int|Fixed: 5|Y|
|4|name|String|Fixed: "Statement"|N|
|5|pid|hex|Id of this protocol|N|
|6|data.title|string|the title of statement |N|
|7|data.content|string|the content of statement |Y|
|8|data.confirm|string|Fixed: "This is a formal and irrevocable statement."|Y|

* Example

```
{
	"type": "FEIP",
	"sn": 8,
	"ver": 5,
	"name": "Statement",
	"pid": "",
	"data": {
		"title": "About Donation",
		"content": "I accept donations from any one.",
		"confirm": "This is a formal and irrevocable statement."
	}
}
```