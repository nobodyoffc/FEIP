# FEIP18V2_Team(zh-CN)

## Contents

[Summary](#summary)

[General consensus of FEIP](#general-consensus-of-feip)

[Consensus of this protocol](#consensus-of-this-protocol)

[Create a team](#create-a-team)

[Disband a team](#disband-a-team)

[Transfer a team](#transfer-a-team)

[Take over a team](#take-over-a-team)

[Update team information](#update-team-information)

[Agree new consensus](#agree-new-consensus)

[Invite members](#invite-members)

[Withdraw invitation](#withdraw-invitation)

[Join a team](#join-a-team)

[Leave a team](#leave-a-team)

[Dismiss members](#dismiss-members)

[Appoint administrator](#appoint-administrator)

[Cancel appointment](#cancel-appointment)

[Rate](#rate-a-team)

[QR code](#qr-code)


## Summary
```
Protocol type: FEIP
Serial number: 18
Protocol name: Team
Version: 1
Description : create and manage teams on freecash blockchain.
Author: C_armX, Deisler-JJ_Sboy
Language: en-US
Created date: 2021-12-06
Last modified date：2023-01-05
```

## General consensus of FEIP

1. FEIP type protocols write data of consensus in OP_RETURN for public witness.

2. The SIGHASH flag of all transaction inputs: ‘ALL’ (value 0x01).

3. The max size of OP_RETURN : 4096 bytes.

4. The format of the data in op_return: JSON.

5. Encoding : utf-8.

6. Since block height `2000000`, any operation of writing to freecash blockchain needs more than `1cd` consumed.


## Consensus of this protocol

1. This protocol is used to create and manage teams on freecash blockchain.
2. Anyone can [create](#create-a-team) a team.
3. The signer of tx creating the team is the `owner` of the team.
4. `TID`(Team identity) is the txid of the transaction in which the team is created. 
5. A team can be called as "[stdName]"+"@"+"[cid/address of the owner]", such as "Freers@Free_cash".
6. The owner should ensure that its teams have different stdNames.
7. Only owner can [disband](#disband-a-team) the team. Once a team was disbanded, it can't be recover forever.
8. Team consensus is published by the owner. `consensusHash` is the double sha256 value of the consensus.
9. Team consensus is accepted by all members when they [`join`](#join-a-team) the team.
10. Only owner can [update](#update-team-information) the "stdName", "localNames", "desc", and "consensusHash" of the team. 
11. `consensusHash` can be updated but can't be deleted.
12. If the "consensusHash" was updated, it's necessary for all members to [agree](#agree-new-consensus) the new consensus.
13. The owner can [transfer](#transfer-a-team) the team to a `transferee`.
14. Transferee can [take over](#take-over-a-team) the team which also means that the transferee accepts the current team consensus.
15. Before `take over` operation, a new `transfer` operation cancels the early one. So, owner can transfer team to itself to cancel unaccepted transfering.
16. The master (see "FEIP6_Master") of owner can transfer the team in case of the owner losing its private key.
17. Owner can [appoint](#appoint-administrator) some active members as administrators and also [Cancel appointment](#cancel-appointment) them.
18. `Owner` is inborn active member and administrator of the team, and can not be dismissed or cancel appointment unless that it succesfully transfered the team to others.
19. Administrator can [invite](#invite-members) others to join the team.
20. Only invitees can [join](#join-a-team) the team which also means that they accepted the current team consensus.
21. Before some invitees joining the team, administrator can [withdraw](#withdraw-invitation) the invitations to them.
22. Active members can [leave](#leave-a-team) the team which will move them from active member list to leaver list.
23. Administrator can dismiss active member including itself but owner.
24. Leaved or dismissed members are no longer administrators.
25. Anyone but the owner can [rate](#rate-a-team) team with score of 0, 1, 2, 3, 4, or 5 and more than `1cd` consumed.
26. The total score(`tRate`) of a team is the cdd weighted average of all ratings to the team.
27. The disbanded team still can be rated.
28. Since block height `2000000`, `create` team needs more than `100cd` consumed.

## Create a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "create"|Y|
|7|data.stdlName|String|Name in English.|Y|
|8|data.localName|String array|Names in different languages|N|
|9|data.consensusHash|String|sha256 value of the team consensus|N|
|10|data.desc|String|Description of this team|N|

* Example of creating a team

consensus：test

```
{
	"type": "FEIP",
	"sn": 18,
	"ver": 1,
	"name": "Team",
	"pid": "",
	"data": {
		"op": "create",
		"stdName": "test",
		"localNames": ["测试", "テスト"],
		"consensusHash": "954d5a49fd70d9b8bcdb35d252267829957f7ef7fa6c74f88419bdc5e82209f4",
		"desc": "This is a test Team"
	}
}
```
## Disband a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "disband"|Y|
|7|data.tid|String|Team ID|Y|

* Example of disbanding a team

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "disband",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c"
    }
}
```

## Transfer a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "transfer"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.transferee|String|Address of transferee|Y|
|9|data.confirm|String|Fixed:"I transfer the team to the transferee."|Y|

* Example of transferring a team

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "transfer",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
"transferee":"FEk41Kqjar45fLDriztUDTUkdki7mmcjWK",
"confirm":"I transfer the team to the transferee."
    }
}
```

## Take over a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "take over"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.confirm|String|Fixed:"I take over the team and agree with the team consensus."|Y|

* Example of taking over a team

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "take over",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
"confirm":"I take over the team and agree with the team consensus.",
"consensusHash": "954d5a49fd70d9b8bcdb35d252267829957f7ef7fa6c74f88419bdc5e82209f4"
}
}
```

## Update team information

Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "update"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.stdlName|String|Name in English.|Y|
|9|data.localName|String array|Names in different languages|N|
|10|data.consensusHash|String|sha256 value of the team consensus|N|
|11|data.desc|String|Description of this team|N|

* Example of updating a team

consensus: test update.

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "update",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
"stdName": "test update",
"localNames": ["测试", "テスト"],
        "consensusHash": "371f7f3ec56330109962f9fb1220fa836ebe89f07ed38515391376ea8e90a1b4",
        "desc": "New description for the test Team"
    }
}
```

## Agree new consensus

Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "agree consensus"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.consensusHash|String|the sha256 value of the new team consensus|Y|
|9|data.confirm|String|Fixed:""I agree with the new consensus."|Y|



* Example of agreeing new consensus

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "agree consensus",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "consensusHash": "371f7f3ec56330109962f9fb1220fa836ebe89f07ed38515391376ea8e90a1b4",
"confirm":"I agree with the new consensus."
    }
}
```

## Invite members
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "invite"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.list|String array|Addresses of the applicants.|Y|

* Example of adding members

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "invite",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "list": [
"F86zoAvNaQxEuYyvQssV5WxEzapNaiDtTW",
"F9pcRpps3T2iHuNGzU3k5b2kWKMRukZP1U"
]
    }
}
```

## Withdraw invitation
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "withdraw invitation"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.list|String array|Addresses.|Y|

* Example of withdrawing invitation

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "withdraw invitation",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "list": [
"F9pcRpps3T2iHuNGzU3k5b2kWKMRukZP1U",
"FFcY22BH2nyamDLdFWaDGwtKej9KdCKb51"
]
    }
}
```

## Join a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "join"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.consensusHash|String|the sha256 value of the new team consensus|Y|
|9|data.confirm|String|Fixed:"I join the team and agree with the team consensus."|Y|


* Example of joining a team
```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "join",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
"confirm":"I join the team and agree with the team consensus.",
        "consensusHash": "954d5a49fd70d9b8bcdb35d252267829957f7ef7fa6c74f88419bdc5e82209f4"
    }
}
```

## Leave a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "leave"|Y|
|7|data.tid|String|Team ID|Y|

* Example of leaving a team

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "leave",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c"
    }
}
```

## Dismiss members
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "dismiss"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.list|String array|3096|Addresses to be dismissed|Y|

* Example of dismiss members

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "dismiss",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "list": [
"F86zoAvNaQxEuYyvQssV5WxEzapNaiDtTW",
"FPL44YJRwPdd2ipziFvqq6y2tw4VnVvpAv",
"FFcY22BH2nyamDLdFWaDGwtKej9KdCKb51"
]
    }
}
```

## Appoint administrator
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "appoint"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.list|String array|the list of addresses being appoint|Y|

* Example of appointing administrators
```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "appoint",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "list": [
"F86zoAvNaQxEuYyvQssV5WxEzapNaiDtTW",
"FCG7ohMm8xZmhPEpE8Eni2ghiZT5ha9iJw",
"FFcY22BH2nyamDLdFWaDGwtKej9KdCKb51"
]
    }
}
```

## Cancel appointment
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "Team"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.op|String|operation: "cancel appointment"|Y|
|7|data.tid|String|Team ID|Y|
|8|data.list|String array|the list of addresses being cancel appointment|Y|

* Example of canceling appointment

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "op": "cancel appointment",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "list": [
"F86zoAvNaQxEuYyvQssV5WxEzapNaiDtTW",
"FCG7ohMm8xZmhPEpE8Eni2ghiZT5ha9iJw",
"FFcY22BH2nyamDLdFWaDGwtKej9KdCKb51"
]
    }
}
```

## Rate a team
Send a tx with the content of op_Return as following:

|field number|field name|type|content|required|
|:----|:----|:----|:----|:----|
|1|type|String|Fixed: "FEIP"|Y|
|2|sn|int|Serial number. Fixed: 18|Y|
|3|ver|int|Fixed: 1|Y|
|4|name|String|Fixed: "CAPP"|N|
|5|pid|string|The PID of this protocol|N|
|6|data.tid|hex|Txid when publishing the team|Y|
|7|data.op|String|operation: "rate"|Y|
|8|data.rate|int|Rating of the service from 0 to 5|Y|

### Example of rating a team

```
{
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "name": "Team",
    "pid": "",
    "data":{
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c",
        "op": "rate",
        "rate": 4
    }
}
```

## QR code

The QR code of a published team has fields as following:

```
{

    "meta":"FC",
    "type": "FEIP",
    "sn": 18,
    "ver": 1,
    "data":{
        "stdName": "test",
        "tid": "974df036a417f5f8ea4e0d028b22c02954be9d1c7953ab9035c4710b2a87756c"
    }
}
```
