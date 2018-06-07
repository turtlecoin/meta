# TurtleCoin Daemon Redevelopment Plan

***This is a living and breathing document. It is subject to change, discussion, debate, and complete dismissal.***

## Objective

Architect, design, and implement a stable, scalable, and efficient network daemon from the ground up to mitigate the shortcomings in the current daemon that have been inherited from the original ByteCoin code. The next iteration of the daemon must be designed as a service to be run 24/7/365 if the user so chooses.

## Daemon Responsibilities

* Maintain the blockchain
    * Request(Pull)/Accept blocks from peers
    * Validate blocks
    * Share (Push) blocks with peers, replicate the top block(s) immediately
    * Share (Respond) existing blocks with peers
    * Expose blockchain via client interfaces
    * Build alternative chains and cleanup when stale
    * Provide blocktemplate to miners
* Maintain list of network peer nodes
    * Discovery of new peer nodes
    * Prune old peer nodes
    * Share list of peer nodes with other peers
    * Verify peer nodes are viable candidates (consensus, version, etc)
    * Ensure unique peers
        - instead of all peers linking to each other exclusively spread out
        - 1/3 peer nodes the same, 2/3 peers connected unique (goal rather than mandatory)
    * Option for priority peers (service operators able to assemble directly connected nodes)
        - helps ensure resilience/durability for service operator
* Maintain transaction mempool
    * Validate new transactions
    * Share (Push) transactions to peers
    * Accept transactions from peers
    * Request (Pull) transactions from peers

## Current Issues

* ***Daemon is wrapped in a giant try/catch block***
* Fails to sync or loses sync sporadically
* Core services do not recover from exceptions gracefully (abort, dumps, etc)
* Client services (RPC) are not designed to handle more than a few connections safely.
* Daemon forks (not as often as it once did) and continues down an alternate chain until resynced.
* Daemon fails to exit in some circumstances
* Blockchain storage may become corrupted in edge cases
* Does not always support legacy hardware
* Network stack (connection handling) needs substantial work

## Daemon Requirements

* Implement the **Ron Popeil** service delivery model, *Start it and forget it*
* Multi-threaded implementation
* Support all features of the current mainline daemon
* Maintain the current blockchain
* Maintain compatibility with other core applications (poolwallet, simplewallet)
    * Either through backwards compatibility or through updating those applications.
* Highly documented and standardized patterns
* Abstraction of the blockchain storage layer
    * Easily support alternative storage backends
    * Requires full DB schema documentation
* Standardized error reporting including:
    * Meaningful Log Levels
    * Descriptive error messages including a standard set of error codes
    * Better edge case handling
    * Bug reports that are auto-generated to simplify error reporting
* UML Style Documentation of Software Components & Services
* Meaningful Namespace creation and utilization
* Ability to STONITH peer nodes that are not updated
* Inter-daemon & daemon to client communication using [Protocol Buffers](https://developers.google.com/protocol-buffers/) payloads
    * Avoid binary packing of data wherever possible
* Build on proven p2p library: [libp2p](https://libp2p.io/)
* More robust peer candidate election process
    * Using metrics such as:
        * Reachability
        * Latency
        * Current Synchronization Status
        * <#> of connected Peer nodes (upstream)
        * <#> of connected Peer nodes (downstream)
        * Block verification time
        * Is Archival node?
        
## Requested Enhancements

* RingCT Implementation
* Implement WebSockets in addition to the standard HTTP style calls
    * Basic query information still available via HTTP style calls (getinfo & blockexplorer)
* Support for SSL/TLS connections on all client interfaces
    * Let's Encrypt?
    * Self-Signed?
* Archival mode
    * Clients can query the daemon but cannot submit new data via the daemon

## Core Libraries

The current core libraries in use by the daemon include the following:

* Daemon Core (daemon)
* Cryptographic Hash Library (crypto)
* CryptoNote Implementation (CryptoNoteCore)
* Peer-to-Peer Implementation (P2P)
* Remote Procedured Call Interface (Rpc)
* Data Packing & Serialization (Serialization)
* HTTP Services (Http)
* Common Library (Common)
* Blockchain Storage (rocksDB)
* Blockchain Explorer (blockchainexplorer)
* Serialization (serialization)

*Note: Although these are the core libraries used today, there is no requirement to maintain the same library structure or methodology in the future.*

### Development Targets

Based on observed behavior and debugging, the areas of the daemon that require the most attention are the following:

* Daemon Core
* Peer-to-Peer Implementation
* Remote Procedure Call Interface
* Data Packing & Serialization
* HTTP Services
* Blockchain Explorer

## Proposed Daemon Architecture

![](https://i.imgur.com/JacoZax.png)

## Block Schema

### BlockHeader

|Property|Type|Content|
|---|---|---|
|majorVersion|uint32|Major block header version|
|minorVersion|uint32|Minor block header version|
|timestamp|uint64|Block creation time (UNIX timestamp)|
|nonce|uint32|Any value which is used in the network consensus algorithm|
|prevId|string|Hex representation of 256-bit identifier of the previous block|

### Base Transaction

|Property|Type|Content|
|---|---|---|
|version|uint32|Transaction format version|
|unlockTime|uint64|UNIX timestamp or Blockchain Height if < CRYPTONOTE_MAX_BLOCK_NUMBER(500000000)|
|inputNum|uint32|Number of inputs - Always 1 for base transactions|
|inputType|int32|Always 0xff for base transactions|
|height|uint64|Height of the block which contains the transaction|
|outputNum|uint32|Number of outputs|
|extraSize|uint32|Number of bytes in the Extra Field|
|extra|[uint32]|Additional data associated with a transaction|

### Transaction Identifier

|Property|Type|Content|
|---|---|---|
|transactionNum|uint64|Number of transaction indentifiers|
|identifiers|[string]|Array of hex representation of 256-bit transaction identifiers|

## Transaction Schema

### Transaction

|Property|Type|Content|
|---|---|---|
|transaction|TransactionData|Transaction data without the signatures|
|signatures|[Signature]|Array of Transaction signatures|

### Transaction Data

|Property|Type|Content|
|---|---|---|
|version|uint32|Transaction format version|
|unlockTime|uint64|UNIX timestamp or Blockchain Height if < CRYPTONOTE_MAX_BLOCK_NUMBER(500000000)|
|inputNum|uint32|Number of inputs - Always 1 for base transactions|
|inputs|[TransactionInput]|Array of Transaction Inputs|
|outputNum|uint32|Number of outputs|
|outputs|[TransactionOutput]|Array of Transaction Outputs|
|extraSize|uint32|Number of bytes in the Extra Field|
|extra|[uint32]|Additional data associated with a transaction|

### Transaction Inputs

#### txin_gen

This type of transaction is generated as the result of mining a block. There can only be one of these transactions per block.

|Property|Type|Content|
|---|---|---|
|inputType|int32|Input type - 0xff. Created by peer who found the block|
|height|uint64|Height of the block which contains the transaction|

#### txin_to_key

This is the type of transaction created by a spend event.

|Property|Type|Content|
|---|---|---|
|inputType|int32|Input type - 0x2|
|amount|uint64|Input Amount|
|keyOffsetNum|uint32|Number of keys used by the input|
|keyOffsets|[uint32]|Offsets corresponding to the outputs referenced by the input|
|keyImage|string|Hex representation of the 256-bit value of the output spent by the input, used to prevent double spending|

### Transaction Outputs

#### Outputs

|Property|Type|Content|
|---|---|---|
|amount|uint64|Output Amount|
|target|TransactionOutputTarget|Output destination. Destinations may be of different types.|

#### Transaction Output Target

|Property|Type|Content|
|---|---|---|
|outputType|uint32|Output type. 0x2 = txout_to_key|
|key|string|Output One Time Public Key|

### Signatures

|Property|Type|Content|
|---|---|---|
|signature|string|Hex representation of 512-bit value of the signature|

## HTTP API

This endpoint will be redesigned to support better data packing. This will deprecate the legacy binary packing model for the use of [Protocol Buffers](https://developers.google.com/protocol-buffers/)

### Example Protobuf Definitions

```text

message BlockHeader {
  required uint32 majorVersion = 0;
  required uint32 minorVersion = 1;
  required uint64 timestamp = 2;
  required uint32 nonce = 3;
  required string prevId = 4;
}

message BaseTransaction {
  required uint32 version = 0;
  required unlockTime uint64 = 1;
  required uint32 inputNum = 2;
  required int32 inputType = 3;
  required uint64 height = 4;
  required uint32 outputNum = 5;
  required uint32 extraSize = 6;
  repeated uint32 extra = 7;
}

message TransactionIdentifier {
  required uint64 transactionNum = 0;
  repeated string identifiers = 1;
}

message Transaction {
  required TransactionData transaction = 0;
  repeated Signature signatures = 1;
}

message TransactionData {
  required uint32 version = 0;
  required uint64 unlockTime = 1;
  required uint32 inputNum = 2;
  oneof TransactionInput {
    repeated TransactionInputGen = 3;
    repeated TransactionInputKey = 4;
  }
  required uint32 outputNum = 5;
  repeated TransactionOutput = 6;
  required uint32 extraSize = 7;
  repeated uint32 extra = 8;
}

message TransactionInputGen {
  required int32 inputType = 0;
  required uint64 height = 1;
}

message TransactionInputKey {
  required int32 inputType = 0;
  required uint64 amount = 1;
  required uint32 keyOffsetNum = 2;
  repeated uint32 keyOffsets = 3;
  required string keyImage = 4;
}

message TransactionOutput {
  message OutputTarget {
    required uint32 outputType = 0;
    required string key = 1;
  }
  required uint64 amount = 0;
  required OutputTarget target = 1;
}

message Signature {
  required string signature = 0;
}
```

## JSON API

The goal here is to retain the current JSON API to maintain compatibility with existing third-party packages including pools, block explorers, etc.

All data supplied below is an **example** of the data provided or required for correct operation.

### /getheight

```json
{
  "height": 455231,
  "network_height": 455231,
  "status": "OK"
}
```

### /getinfo

```json
{
  "alt_blocks_count": 3,
  "difficulty": 171548865,
  "grey_peerlist_size": 5000,
  "hashrate": 5718295,
  "height": 455234,
  "incoming_connections_count": 16,
  "last_known_block_index": 455231,
  "major_version": 4,
  "minor_version": 0,
  "network_height": 455234,
  "outgoing_connections_count": 8,
  "start_time": 1526385600,
  "status": "OK",
  "synced": true,
  "testnet": false,
  "tx_count": 420165,
  "tx_pool_size": 0,
  "upgrade_height": 500000,
  "version": "0.5.0",
  "white_peerlist_size": 1000
}
```

### /getpeers

```json
{
  "peers": [
    "172.92.135.174:11897",
    "176.150.166.14:11897",
    "195.218.10.26:11897",
    "144.132.59.58:11897"
  ],
  "status": "OK"
}
```

### /gettransactions

POST Data:
```json
{
  "txs_hashes": [
    "c5d52999269742473a98e093e06fad7d210841dff788eece48192bdd779ee71d"
  ]
}
```

```json
{
  "missed_tx": [],
  "status": "OK",
  "txs_as_hex": [
    "019be51b01fff3e41b05040264d0273b0f51caf71a33bc4c3fed217974722fa13f6d496d0689d28bcd80afba46027cdc3c2d75f116d93fdf280277ad7465d8f79e347fe810574eff76e543827f54c0b802026d3f0c66cf32bd56b7764780402eac495ad9e2669660c2dbab151a290cae9c9fa0f73602e89f453e0d8aee68747cf67fe0f81685281951b6e424f72b91fd38bffeb462c080897a020f72241a3865e9fa3d31296608c82dcdd0ce8b84433dcd1f02ecbb02fe6b9f682b01eb22199278d4e145d0bdbdc3df4c012a983ba982f594446ac20214c5a27f76cd020800000000f63be4ef"
  ]
}
```

### /json_rpc

All of the following API calls are processed against a common endpoint available via HTTP (/json_rpc)

The standard structure of the POST data is:

```json
{
  "jsonrpc": "2.0",
  "method": "<methodname>",
  "params": {}
}
```

#### getblockcount

POST Data:

```json
{
  "jsonrpc": "2.0",
  "method": "getblockcount",
  "params": {}
}
```

Response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "count": 455591,
    "status": "OK"
  }
}
```

#### getblockhash

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "on_getblockhash",
  "params": [
    455580
  ]
}
```

Response:

```json
{
  "jsonrpc": "2.0",
  "result": "d6ef1b7c597a2051185065653a7e87071b5d6a0b4742faa43626a6848becc9ae"
}
```

#### getblocktemplate

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "getblocktemplate",
  "params": {
    "reserve_size": 200,
    "wallet_address": "TRTLuxN6FVALYxeAEKhtWDYNS9Vd9dHVp3QHwjKbo76ggQKgUfVjQp8iPypECCy3MwZVyu89k1fWE2Ji6EKedbrqECHHWouZN6g"
  }
}
```

Response:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "blocktemplate_blob": "0400ebfe6327fdd98d274c5db33f748e2ab8889be63b44352a5c8ce3f075653bf0180000ddadeed705000000000000000000000000000000000000000000000000000000000000000000000000010000000023032100000000000000000000000000000000000000000000000000000000000000000001e1e51b01ffb9e51b060802963263216fb47d5d61e176f2d78d9ee65b782ef0569e6ba3518918e46cb410094602d49259145f8c176a688cd864f52eb540513a87d137ed1941414aef8793dd06d16402e3caa5014f94722bd0d46e38de9e2b6afdfa6ecd72f13e68ef9b8c43fdc27cb4c0b80202133add1315a37b2e3471672c4d5871371c391d8cdab89802d0c1e65944ff822aa0f7360267a41ac6b2c9ef904b1721a0b5f2f5f3084d6c07c193cd70695606c4e8fec37180897a02ffdd3825c8b02e49072eb7dd3dc4ab0a2924e60072cd4565522b0900b10781dbeb0101e72c54f95fa10141656d04d508f50aa9e008b92092d36f1d8711ebd3297b763f02c80000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000029a08b121204c5cf6604afbac5fbdf176d86804130739330cd04eb05795b406e16a2cb12ba842b60026a7d8e7f534f7246bee719523164ad6201d851af9041c60",
    "difficulty": 201447423,
    "height": 455353,
    "reserved_offset": 376,
    "status": "OK"
  }
}
```

#### submitblock

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "submitblock",
  "params": [
    "0100b...."
  ]
}
```

Response:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

#### getlastblockheader

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "getlastblockheader"
}
```

Response:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 349,
      "depth": 0,
      "difficulty": 199998346,
      "hash": "29e3d8fd377d84331087142514c371c9745fc9ef3d22dad1f188641afaea1988",
      "height": 455351,
      "major_version": 4,
      "minor_version": 0,
      "nonce": 11088,
      "num_txes": 1,
      "orphan_status": false,
      "prev_hash": "80eca84132279364e41dc1e471a73000c021975217bdef78256598ec9223256d",
      "reward": 2940068,
      "timestamp": 1526437453
    },
    "status": "OK"
  }
}
```

#### getblockheaderbyhash

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "getblockheaderbyhash",
  "params": {
    "hash": "56c945e94b6aa913de81377f30ab2aec43cfcbbdc5aa1d8e95873b938c735a9e"
  }
}
```

Response:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 31962,
      "depth": 14,
      "difficulty": 382188237,
      "hash": "56c945e94b6aa913de81377f30ab2aec43cfcbbdc5aa1d8e95873b938c735a9e",
      "height": 455334,
      "major_version": 4,
      "minor_version": 0,
      "nonce": 66664,
      "num_txes": 5,
      "orphan_status": false,
      "prev_hash": "c12c2d36e10c8fd6b96e743f026f02ea225558e1543d07aa0aa0df0430e84a51",
      "reward": 2940470,
      "timestamp": 1526436759
    },
    "status": "OK"
  }
}
```

#### getblockheaderbyheight

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "getblockheaderbyheight",
  "params": {
    "height": 455340
  }
}
```

Response:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 10904,
      "depth": 7,
      "difficulty": 245257467,
      "hash": "1853b52bf6078e736c8031679f602abe11d27756acbfe9abf128db9a5fffd13b",
      "height": 455340,
      "major_version": 4,
      "minor_version": 0,
      "nonce": 1932738639,
      "num_txes": 2,
      "orphan_status": false,
      "prev_hash": "64bcf257745406ab439aa0fae0624308a6bfcdf34a842e723593d15d9333259c",
      "reward": 2940169,
      "timestamp": 1526437146
    },
    "status": "OK"
  }
}
```

#### getcurrencyId

POST Data:
```json
{
  "jsonrpc": "2.0",
  "method": "getcurrencyid"
}
```

Response:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "currency_id_blob": "7fb97df81221dd1366051b2d0bc7f49c66c22ac4431d879c895b06d66ef66f4c"
  }
}
```

### Block Explorer (/json_rpc)

### f_blocks_list_json

POST Data:

```json
{
  "jsonrpc": "2.0",
  "method": "f_blocks_list_json",
  "params": {
    "height": 455589
  }
}
```

Response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "blocks": [
      {
        "cumul_size": 349,
        "difficulty": 304731837,
        "hash": "a618bbba5e26a523380eacfd9ee82064f61fd52138fea657ba9931a00f249580",
        "height": 455589,
        "timestamp": 1526444823,
        "tx_count": 1
      },
      {
        "cumul_size": 5762,
        "difficulty": 284608423,
        "hash": "09c920d51d262cd68af4bfb9826a8e7f53b918c0e73b0f17b49d33744d6d9ab0",
        "height": 455588,
        "timestamp": 1526444795,
        "tx_count": 2
      },
      {
        "cumul_size": 349,
        "difficulty": 283944571,
        "hash": "2db092b698ad6e12485e65535adcbd156595347dbbcc7dde20a37cdb2e479d26",
        "height": 455587,
        "timestamp": 1526444778,
        "tx_count": 1
      }
    ],
    "status": "OK"
  }
}
```

#### f_block_json

POST Data:

```json
{
  "jsonrpc": "2.0",
  "method": "f_block_json",
  "params": {
    "hash": "1bbedc0de1f3e842fb408fee1dfaf66d39d12fe0b1f4895bb7aeb37b7e70170a"
  }
}
```

Response:

```json
{
  "id": "test",
  "jsonrpc": "2.0",
  "result": {
    "block": {
      "alreadyGeneratedCoins": "1348455353402",
      "alreadyGeneratedTransactions": 876153,
      "baseReward": 2940045,
      "blockSize": 41012,
      "depth": 7,
      "difficulty": 211550194,
      "effectiveSizeMedian": 100000,
      "hash": "1bbedc0de1f3e842fb408fee1dfaf66d39d12fe0b1f4895bb7aeb37b7e70170a",
      "height": 455617,
      "major_version": 4,
      "minor_version": 0,
      "nonce": 715832412,
      "orphan_status": false,
      "penalty": 0,
      "prev_hash": "ee9b3552e7c8a9d9df357f667b02f75deac5a5bac1635356814b20243a745ed6",
      "reward": 2940355,
      "sizeMedian": 230,
      "timestamp": 1526445771,
      "totalFeeAmount": 310,
      "transactions": [
        {
          "amount_out": 2940355,
          "fee": 0,
          "hash": "cc0c51c82c25f27de5e71341f133dc1292554bb2d33b213b533acb9f38d6c326",
          "size": 265
        },
        {
          "amount_out": 6109562,
          "fee": 100,
          "hash": "b2fa90433c5f3205d19383e7dc6617500d87dea035554ba5741ed64ed649edc7",
          "size": 8809
        },
        {
          "amount_out": 7138118,
          "fee": 100,
          "hash": "4694804778ff0c459f46edd5002ac086c4ce63865b0da6740bd09eecea2fdc10",
          "size": 11278
        },
        {
          "amount_out": 13771628,
          "fee": 100,
          "hash": "e28c193ead41bc897105b94dd68c74e6a2a611db2873a91abb78578e5e5ff796",
          "size": 17899
        },
        {
          "amount_out": 123068690,
          "fee": 10,
          "hash": "138e711ebab9cb13d6b4527b9b5ab13f501608fec56e46b60e5471b04c14ddaf",
          "size": 2514
        }
      ],
      "transactionsCumulativeSize": 40765
    },
    "status": "OK"
  }
}
```

#### f_transaction_json

POST Data:

```json
{
  "jsonrpc": "2.0",
  "method": "f_transaction_json",
  "params": {
    "hash": "1b4bedfd1e9f64638c484208702c4d363b620326a8791d6d851768922f75bd3d"
  }
}
```

Response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "block": {
      "cumul_size": 349,
      "difficulty": 74534657,
      "hash": "ee9b3552e7c8a9d9df357f667b02f75deac5a5bac1635356814b20243a745ed6",
      "height": 455616,
      "timestamp": 1526445747,
      "tx_count": 1
    },
    "status": "OK",
    "tx": {
      "extra": "014098af950f67c4347ae6f96dd14c2cd7c81402d66d5435035c36037617e8ce2f020800000000a889a3a3",
      "unlock_time": 455656,
      "version": 1,
      "vin": [
        {
          "type": "ff",
          "value": {
            "height": 455616
          }
        }
      ],
      "vout": [
        {
          "amount": 5,
          "target": {
            "data": {
              "key": "4f8b2e2f6ec218f0f1b1ecb7d52e0ae23ffee1caa4603e426af1550e980a6062"
            },
            "type": "02"
          }
        },
        {
          "amount": 40,
          "target": {
            "data": {
              "key": "4f18f68a89e2e5379f57789b70ba9f2647d5547295ecf6c2f3cb615fc3145095"
            },
            "type": "02"
          }
        },
        {
          "amount": 40000,
          "target": {
            "data": {
              "key": "c4aa254feabf6e22eea2defe309166e92978a571edd0cb32d1c28b836e8b4c6f"
            },
            "type": "02"
          }
        },
        {
          "amount": 900000,
          "target": {
            "data": {
              "key": "cf4d252f741557d18f7e782d81b55d250283b22709022f0f168305610793c178"
            },
            "type": "02"
          }
        },
        {
          "amount": 2000000,
          "target": {
            "data": {
              "key": "9c14a6d2a695302b41c17848a5b352b057a1a9fb4d149b9c15e8297debd547ff"
            },
            "type": "02"
          }
        }
      ]
    },
    "txDetails": {
      "amount_out": 2940045,
      "fee": 0,
      "hash": "1b4bedfd1e9f64638c484208702c4d363b620326a8791d6d851768922f75bd3d",
      "mixin": 0,
      "paymentId": "",
      "size": 230
    }
  }
}
```

#### f_on_transactions_pool_json

POST Data:

```json
{
  "jsonrpc": "2.0",
  "method": "f_on_transactions_pool_json",
  "params": {}
}
```

Response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "transactions": [
      {
        "amount_out": 291000,
        "fee": 0,
        "hash": "91fc16ad4fa79dcb6dedad27af293635f7cf199f1abfcef0f9e95c7e81460511",
        "size": 28552
      },
      {
        "amount_out": 7000000,
        "fee": 0,
        "hash": "1f5d9e6efa1158bb120d9d752026534900fc6b0b13cbd00861732841b791edd7",
        "size": 28577
      },
      {
        "amount_out": 1840000,
        "fee": 0,
        "hash": "56d66eb011f661b5025b91f993ea35ca60236c8cd150907c1c7729ccab27b112",
        "size": 28582
      }
    ]
  }
}
```
