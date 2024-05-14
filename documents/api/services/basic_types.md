# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [basic_types.proto](#basic_types-proto)
    - [AccountAmount](#proto-AccountAmount)
    - [AccountID](#proto-AccountID)
    - [ContractID](#proto-ContractID)
    - [CurrentAndNextFeeSchedule](#proto-CurrentAndNextFeeSchedule)
    - [FeeComponents](#proto-FeeComponents)
    - [FeeData](#proto-FeeData)
    - [FeeSchedule](#proto-FeeSchedule)
    - [FileID](#proto-FileID)
    - [Fraction](#proto-Fraction)
    - [Key](#proto-Key)
    - [KeyList](#proto-KeyList)
    - [NftID](#proto-NftID)
    - [NftTransfer](#proto-NftTransfer)
    - [NodeAddress](#proto-NodeAddress)
    - [NodeAddressBook](#proto-NodeAddressBook)
    - [RealmID](#proto-RealmID)
    - [ScheduleID](#proto-ScheduleID)
    - [SemanticVersion](#proto-SemanticVersion)
    - [ServiceEndpoint](#proto-ServiceEndpoint)
    - [ServicesConfigurationList](#proto-ServicesConfigurationList)
    - [Setting](#proto-Setting)
    - [ShardID](#proto-ShardID)
    - [Signature](#proto-Signature)
    - [SignatureList](#proto-SignatureList)
    - [SignatureMap](#proto-SignatureMap)
    - [SignaturePair](#proto-SignaturePair)
    - [StakingInfo](#proto-StakingInfo)
    - [ThresholdKey](#proto-ThresholdKey)
    - [ThresholdSignature](#proto-ThresholdSignature)
    - [TokenAssociation](#proto-TokenAssociation)
    - [TokenBalance](#proto-TokenBalance)
    - [TokenBalances](#proto-TokenBalances)
    - [TokenID](#proto-TokenID)
    - [TokenRelationship](#proto-TokenRelationship)
    - [TokenTransferList](#proto-TokenTransferList)
    - [TopicID](#proto-TopicID)
    - [TransactionFeeSchedule](#proto-TransactionFeeSchedule)
    - [TransactionID](#proto-TransactionID)
    - [TransferList](#proto-TransferList)
  
    - [HederaFunctionality](#proto-HederaFunctionality)
    - [SubType](#proto-SubType)
    - [TokenFreezeStatus](#proto-TokenFreezeStatus)
    - [TokenKeyValidation](#proto-TokenKeyValidation)
    - [TokenKycStatus](#proto-TokenKycStatus)
    - [TokenPauseStatus](#proto-TokenPauseStatus)
    - [TokenSupplyType](#proto-TokenSupplyType)
    - [TokenType](#proto-TokenType)
  
- [Scalar Value Types](#scalar-value-types)



<a name="basic_types-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## basic_types.proto



<a name="proto-AccountAmount"></a>

### AccountAmount
An account, and the amount that it sends or receives during a cryptocurrency or token transfer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID that sends/receives cryptocurrency or tokens |
| amount | [sint64](#sint64) |  | The amount of tinybars (for Crypto transfers) or in the lowest denomination (for Token transfers) that the account sends(negative) or receives(positive) |
| is_approval | [bool](#bool) |  | If true then the transfer is expected to be an approved allowance and the accountID is expected to be the owner. The default is false (omitted). |






<a name="proto-AccountID"></a>

### AccountID
The ID for an a cryptocurrency account


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | The shard number (nonnegative) |
| realmNum | [int64](#int64) |  | The realm number (nonnegative) |
| accountNum | [int64](#int64) |  | A non-negative account number unique within its realm |
| alias | [bytes](#bytes) |  | The public key bytes to be used as the account's alias. The public key bytes are the result of serializing a protobuf Key message for any primitive key type. Currently only primitive key bytes are supported as an alias (ThresholdKey, KeyList, ContractID, and delegatable_contract_id are not supported)

May also be the ethereum account 20-byte EVM address to be used initially in place of the public key bytes. This EVM address may be either the encoded form of the shard.realm.num or the keccak-256 hash of a ECDSA_SECP256K1 primitive key.

At most one account can ever have a given alias and it is used for account creation if it was automatically created using a crypto transfer. It will be null if an account is created normally. It is immutable once it is set for an account.

If a transaction auto-creates the account, any further transfers to that alias will simply be deposited in that account, without creating anything, and with no creation fee being charged.

If a transaction lazily-creates this account, a subsequent transaction will be required containing the public key bytes that map to the EVM address bytes. The provided public key bytes will then serve as the final alias bytes. |






<a name="proto-ContractID"></a>

### ContractID
The ID for a smart contract instance


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | The shard number (nonnegative) |
| realmNum | [int64](#int64) |  | The realm number (nonnegative) |
| contractNum | [int64](#int64) |  | A nonnegative number unique within a given shard and realm |
| evm_address | [bytes](#bytes) |  | The 20-byte EVM address of the contract to call.

Every contract has an EVM address determined by its <tt>shard.realm.num</tt> id. This address is as follows: <ol> <li>The first 4 bytes are the big-endian representation of the shard.</li> <li>The next 8 bytes are the big-endian representation of the realm.</li> <li>The final 8 bytes are the big-endian representation of the number.</li> </ol>

Contracts created via CREATE2 have an <b>additional, primary address</b> that is derived from the <a href="https://eips.ethereum.org/EIPS/eip-1014">EIP-1014</a> specification, and does not have a simple relation to a <tt>shard.realm.num</tt> id.

(Please do note that CREATE2 contracts can also be referenced by the three-part EVM address described above.) |






<a name="proto-CurrentAndNextFeeSchedule"></a>

### CurrentAndNextFeeSchedule
This contains two Fee Schedules with expiry timestamp.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| currentFeeSchedule | [FeeSchedule](#proto-FeeSchedule) |  | Contains current Fee Schedule |
| nextFeeSchedule | [FeeSchedule](#proto-FeeSchedule) |  | Contains next Fee Schedule |






<a name="proto-FeeComponents"></a>

### FeeComponents
A set of prices the nodes use in determining transaction and query fees, and constants involved
in fee calculations.  Nodes multiply the amount of resources consumed by a transaction or query
by the corresponding price to calculate the appropriate fee. Units are one-thousandth of a
tinyCent.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| min | [int64](#int64) |  | A minimum, the calculated fee must be greater than this value |
| max | [int64](#int64) |  | A maximum, the calculated fee must be less than this value |
| constant | [int64](#int64) |  | A constant contribution to the fee |
| bpt | [int64](#int64) |  | The price of bandwidth consumed by a transaction, measured in bytes |
| vpt | [int64](#int64) |  | The price per signature verification for a transaction |
| rbh | [int64](#int64) |  | The price of RAM consumed by a transaction, measured in byte-hours |
| sbh | [int64](#int64) |  | The price of storage consumed by a transaction, measured in byte-hours |
| gas | [int64](#int64) |  | The price of computation for a smart contract transaction, measured in gas |
| tv | [int64](#int64) |  | The price per hbar transferred for a transfer |
| bpr | [int64](#int64) |  | The price of bandwidth for data retrieved from memory for a response, measured in bytes |
| sbpr | [int64](#int64) |  | The price of bandwidth for data retrieved from disk for a response, measured in bytes |






<a name="proto-FeeData"></a>

### FeeData
The total fee charged for a transaction. It is composed of three components - a node fee that
compensates the specific node that submitted the transaction, a network fee that compensates the
network for assigning the transaction a consensus timestamp, and a service fee that compensates
the network for the ongoing maintenance of the consequences of the transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nodedata | [FeeComponents](#proto-FeeComponents) |  | Fee paid to the submitting node |
| networkdata | [FeeComponents](#proto-FeeComponents) |  | Fee paid to the network for processing a transaction into consensus |
| servicedata | [FeeComponents](#proto-FeeComponents) |  | Fee paid to the network for providing the service associated with the transaction; for instance, storing a file |
| subType | [SubType](#proto-SubType) |  | SubType distinguishing between different types of FeeData, correlating to the same HederaFunctionality |






<a name="proto-FeeSchedule"></a>

### FeeSchedule
A list of resource prices fee for different transactions and queries and the time period at which
this fee schedule will expire. Nodes use the prices to determine the fees for all transactions
based on how much of those resources each transaction uses.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactionFeeSchedule | [TransactionFeeSchedule](#proto-TransactionFeeSchedule) | repeated | List of price coefficients for network resources |
| expiryTime | [TimestampSeconds](#proto-TimestampSeconds) |  | FeeSchedule expiry time |






<a name="proto-FileID"></a>

### FileID
The ID for a file


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | The shard number (nonnegative) |
| realmNum | [int64](#int64) |  | The realm number (nonnegative) |
| fileNum | [int64](#int64) |  | A nonnegative File number unique within its realm |






<a name="proto-Fraction"></a>

### Fraction
A rational number, used to set the amount of a value transfer to collect as a custom fee


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| numerator | [int64](#int64) |  | The rational's numerator |
| denominator | [int64](#int64) |  | The rational's denominator; a zero value will result in FRACTION_DIVIDES_BY_ZERO |






<a name="proto-Key"></a>

### Key
A Key can be a public key from either the Ed25519 or ECDSA(secp256k1) signature schemes, where
in the ECDSA(secp256k1) case we require the 33-byte compressed form of the public key. We call
these public keys <b>primitive keys</b>.

If an account has primitive key associated to it, then the corresponding private key must sign
any transaction to transfer cryptocurrency out of it.

A Key can also be the ID of a smart contract instance, which is then authorized to perform any
precompiled contract action that requires this key to sign.

Note that when a Key is a smart contract ID, it <i>doesn't</i> mean the contract with that ID
will actually create a cryptographic signature. It only means that when the contract calls a
precompiled contract, the resulting "child transaction" will be authorized to perform any action
controlled by the Key.

A Key can be a "threshold key", which means a list of M keys, any N of which must sign in order
for the threshold signature to be considered valid. The keys within a threshold signature may
themselves be threshold signatures, to allow complex signature requirements.

A Key can be a "key list" where all keys in the list must sign unless specified otherwise in the
documentation for a specific transaction type (e.g.  FileDeleteTransactionBody).  Their use is
dependent on context. For example, a Hedera file is created with a list of keys, where all of
them must sign a transaction to create or modify the file, but only one of them is needed to sign
a transaction to delete the file. So it's a single list that sometimes acts as a 1-of-M threshold
key, and sometimes acts as an M-of-M threshold key.  A key list is always an M-of-M, unless
specified otherwise in documentation. A key list can have nested key lists or threshold keys.
Nested key lists are always M-of-M. A key list can have repeated primitive public keys, but all
repeated keys are only required to sign once.

A Key can contain a ThresholdKey or KeyList, which in turn contain a Key, so this mutual
recursion would allow nesting arbitrarily deep. A ThresholdKey which contains a list of primitive
keys has 3 levels: ThresholdKey -> KeyList -> Key. A KeyList which contains several primitive
keys has 2 levels: KeyList -> Key. A Key with 2 levels of nested ThresholdKeys has 7 levels:
Key -> ThresholdKey -> KeyList -> Key -> ThresholdKey -> KeyList -> Key.

Each Key should not have more than 46 levels, which implies 15 levels of nested ThresholdKeys.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | smart contract instance that is authorized as if it had signed with a key |
| ed25519 | [bytes](#bytes) |  | Ed25519 public key bytes |
| RSA_3072 | [bytes](#bytes) |  | (NOT SUPPORTED) RSA-3072 public key bytes |
| ECDSA_384 | [bytes](#bytes) |  | (NOT SUPPORTED) ECDSA with the p-384 curve public key bytes |
| thresholdKey | [ThresholdKey](#proto-ThresholdKey) |  | a threshold N followed by a list of M keys, any N of which are required to form a valid signature |
| keyList | [KeyList](#proto-KeyList) |  | A list of Keys of the Key type. |
| ECDSA_secp256k1 | [bytes](#bytes) |  | Compressed ECDSA(secp256k1) public key bytes |
| delegatable_contract_id | [ContractID](#proto-ContractID) |  | A smart contract that, if the recipient of the active message frame, should be treated as having signed. (Note this does not mean the <i>code being executed in the frame</i> will belong to the given contract, since it could be running another contract's code via <tt>delegatecall</tt>. So setting this key is a more permissive version of setting the contractID key, which also requires the code in the active message frame belong to the the contract with the given id.) |






<a name="proto-KeyList"></a>

### KeyList
A list of keys that requires all keys (M-of-M) to sign unless otherwise specified in
documentation. A KeyList may contain repeated keys, but all repeated keys are only required to
sign once.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| keys | [Key](#proto-Key) | repeated | list of keys |






<a name="proto-NftID"></a>

### NftID
Identifier for a unique token (or "NFT"), used by both contract and token services.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_ID | [TokenID](#proto-TokenID) |  | The (non-fungible) token of which this NFT is an instance; uppercase for "ID" for backward compatibility with original definition of this field. |
| serial_number | [int64](#int64) |  | The serial number of this NFT within its token type |






<a name="proto-NftTransfer"></a>

### NftTransfer
A sender account, a receiver account, and the serial number of an NFT of a Token with
NON_FUNGIBLE_UNIQUE type. When minting NFTs the sender will be the default AccountID instance
(0.0.0) and when burning NFTs, the receiver will be the default AccountID instance.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| senderAccountID | [AccountID](#proto-AccountID) |  | The accountID of the sender |
| receiverAccountID | [AccountID](#proto-AccountID) |  | The accountID of the receiver |
| serialNumber | [int64](#int64) |  | The serial number of the NFT |
| is_approval | [bool](#bool) |  | If true then the transfer is expected to be an approved allowance and the senderAccountID is expected to be the owner. The default is false (omitted). |






<a name="proto-NodeAddress"></a>

### NodeAddress
The data about a node, including its service endpoints and the Hedera account to be paid for
services provided by the node (that is, queries answered and transactions submitted.)

If the `serviceEndpoint` list is not set, or empty, then the endpoint given by the
(deprecated) `ipAddress` and `portno` fields should be used.

All fields are populated in the 0.0.102 address book file while only fields that start with # are
populated in the 0.0.101 address book file.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ipAddress | [bytes](#bytes) |  | **Deprecated.** The IP address of the Node with separator & octets encoded in UTF-8. Usage is deprecated, ServiceEndpoint is preferred to retrieve a node's list of IP addresses and ports |
| portno | [int32](#int32) |  | **Deprecated.** The port number of the grpc server for the node. Usage is deprecated, ServiceEndpoint is preferred to retrieve a node's list of IP addresses and ports |
| memo | [bytes](#bytes) |  | **Deprecated.** Usage is deprecated, nodeAccountId is preferred to retrieve a node's account ID |
| RSA_PubKey | [string](#string) |  | The node's X509 RSA public key used to sign stream files (e.g., record stream files). Precisely, this field is a string of hexadecimal characters which, translated to binary, are the public key's DER encoding. |
| nodeId | [int64](#int64) |  | # A non-sequential identifier for the node |
| nodeAccountId | [AccountID](#proto-AccountID) |  | # The account to be paid for queries and transactions sent to this node |
| nodeCertHash | [bytes](#bytes) |  | # Hash of the node's TLS certificate. Precisely, this field is a string of hexadecimal characters which, translated to binary, are the SHA-384 hash of the UTF-8 NFKD encoding of the node's TLS cert in PEM format. Its value can be used to verify the node's certificate it presents during TLS negotiations. |
| serviceEndpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | # A node's service IP addresses and ports |
| description | [string](#string) |  | A description of the node, with UTF-8 encoding up to 100 bytes |
| stake | [int64](#int64) |  | **Deprecated.** [Deprecated] The amount of tinybars staked to the node |






<a name="proto-NodeAddressBook"></a>

### NodeAddressBook
A list of nodes and their metadata that contains all details of the nodes for the network.  Used
to parse the contents of system files <tt>0.0.101</tt> and <tt>0.0.102</tt>.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nodeAddress | [NodeAddress](#proto-NodeAddress) | repeated | Metadata of all nodes in the network |






<a name="proto-RealmID"></a>

### RealmID
The ID for a realm. Within a given shard, every realm has a unique ID. Each account, file, and
contract instance belongs to exactly one realm.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | The shard number (nonnegative) |
| realmNum | [int64](#int64) |  | The realm number (nonnegative) |






<a name="proto-ScheduleID"></a>

### ScheduleID
Unique identifier for a Schedule


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | A nonnegative shard number |
| realmNum | [int64](#int64) |  | A nonnegative realm number |
| scheduleNum | [int64](#int64) |  | A nonnegative schedule number |






<a name="proto-SemanticVersion"></a>

### SemanticVersion
Hedera follows semantic versioning (https://semver.org/) for both the HAPI protobufs and the
Services software.  This type allows the <tt>getVersionInfo</tt> query in the
<tt>NetworkService</tt> to return the deployed versions of both protobufs and software on the
node answering the query.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| major | [int32](#int32) |  | Increases with incompatible API changes |
| minor | [int32](#int32) |  | Increases with backwards-compatible new functionality |
| patch | [int32](#int32) |  | Increases with backwards-compatible bug fixes |
| pre | [string](#string) |  | A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers (https://semver.org/#spec-item-9); so given a semver 0.14.0-alpha.1+21AF26D3, this field would contain 'alpha.1' |
| build | [string](#string) |  | Build metadata MAY be denoted by appending a plus sign and a series of dot separated identifiers immediately following the patch or pre-release version (https://semver.org/#spec-item-10); so given a semver 0.14.0-alpha.1+21AF26D3, this field would contain '21AF26D3' |






<a name="proto-ServiceEndpoint"></a>

### ServiceEndpoint
Contains the IP address and the port representing a service endpoint of a Node in a network. Used
to reach the Hedera API and submit transactions to the network.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ipAddressV4 | [bytes](#bytes) |  | The 4-byte IPv4 address of the endpoint encoded in left to right order (e.g. 127.0.0.1 has bytes [127, 0, 0, 1]) |
| port | [int32](#int32) |  | The port of the service endpoint |
| domain_name | [string](#string) |  | A node domain name.<br/> This MUST be the fully qualified domain(DNS) name of the node.<br/> This value MUST NOT be more than 253 characters. |






<a name="proto-ServicesConfigurationList"></a>

### ServicesConfigurationList
UNDOCUMENTED


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nameValue | [Setting](#proto-Setting) | repeated | list of name value pairs of the application properties |






<a name="proto-Setting"></a>

### Setting
UNDOCUMENTED


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | name of the property |
| value | [string](#string) |  | value of the property |
| data | [bytes](#bytes) |  | any data associated with property |






<a name="proto-ShardID"></a>

### ShardID
Each shard has a nonnegative shard number. Each realm within a given shard has a nonnegative
realm number (that number might be reused in other shards). And each account, file, and smart
contract instance within a given realm has a nonnegative number (which might be reused in other
realms).  Every account, file, and smart contract instance is within exactly one realm. So a
FileID is a triplet of numbers, like 0.1.2 for entity number 2 within realm 1  within shard 0.
Each realm maintains a single counter for assigning numbers,  so if there is a file with ID
0.1.2, then there won't be an account or smart  contract instance with ID 0.1.2.

Everything is partitioned into realms so that each Solidity smart contract can  access everything
in just a single realm, locking all those entities while it's  running, but other smart contracts
could potentially run in other realms in  parallel. So realms allow Solidity to be parallelized
somewhat, even though the  language itself assumes everything is serial.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | the shard number (nonnegative) |






<a name="proto-Signature"></a>

### Signature
This message is <b>DEPRECATED</b> and <b>UNUSABLE</b> with network nodes. It is retained
here only for historical reasons.

Please use the SignaturePair and SignatureMap messages.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contract | [bytes](#bytes) |  | smart contract virtual signature (always length zero) |
| ed25519 | [bytes](#bytes) |  | ed25519 signature bytes |
| RSA_3072 | [bytes](#bytes) |  | RSA-3072 signature bytes |
| ECDSA_384 | [bytes](#bytes) |  | ECDSA p-384 signature bytes |
| thresholdSignature | [ThresholdSignature](#proto-ThresholdSignature) |  | A list of signatures for a single N-of-M threshold Key. This must be a list of exactly M signatures, at least N of which are non-null. |
| signatureList | [SignatureList](#proto-SignatureList) |  | A list of M signatures, each corresponding to a Key in a KeyList of the same length. |






<a name="proto-SignatureList"></a>

### SignatureList
This message is <b>DEPRECATED</b> and <b>UNUSABLE</b> with network nodes. It is retained
here only for historical reasons.

Please use the SignaturePair and SignatureMap messages.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sigs | [Signature](#proto-Signature) | repeated | each signature corresponds to a Key in the KeyList |






<a name="proto-SignatureMap"></a>

### SignatureMap
A set of signatures corresponding to every unique public key used to sign a given transaction. If
one public key matches more than one prefixes on the signature map, the transaction containing
the map will fail immediately with the response code KEY_PREFIX_MISMATCH.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sigPair | [SignaturePair](#proto-SignaturePair) | repeated | Each signature pair corresponds to a unique Key required to sign the transaction. |






<a name="proto-SignaturePair"></a>

### SignaturePair
The client may use any number of bytes from zero to the whole length of the public key for
pubKeyPrefix. If zero bytes are used, then it must be that only one primitive key is required
to sign the linked transaction; it will surely resolve to <tt>INVALID_SIGNATURE</tt> otherwise.

<b>IMPORTANT:</b> In the special case that a signature is being provided for a key used to
authorize a precompiled contract, the <tt>pubKeyPrefix</tt> must contain the <b>entire public
key</b>! That is, if the key is a Ed25519 key, the <tt>pubKeyPrefix</tt> should be 32 bytes
long. If the key is a ECDSA(secp256k1) key, the <tt>pubKeyPrefix</tt> should be 33 bytes long,
since we require the compressed form of the public key.

Only Ed25519 and ECDSA(secp256k1) keys and hence signatures are currently supported.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| pubKeyPrefix | [bytes](#bytes) |  | First few bytes of the public key |
| contract | [bytes](#bytes) |  | smart contract virtual signature (always length zero) |
| ed25519 | [bytes](#bytes) |  | ed25519 signature |
| RSA_3072 | [bytes](#bytes) |  | RSA-3072 signature |
| ECDSA_384 | [bytes](#bytes) |  | ECDSA p-384 signature |
| ECDSA_secp256k1 | [bytes](#bytes) |  | ECDSA(secp256k1) signature |






<a name="proto-StakingInfo"></a>

### StakingInfo
Staking metadata for an account or a contract returned in CryptoGetInfo or ContractGetInfo queries


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| decline_reward | [bool](#bool) |  | If true, this account or contract declined to receive a staking reward. |
| stake_period_start | [Timestamp](#proto-Timestamp) |  | The staking period during which either the staking settings for this account or contract changed (such as starting staking or changing staked_node_id) or the most recent reward was earned, whichever is later. If this account or contract is not currently staked to a node, then this field is not set. |
| pending_reward | [int64](#int64) |  | The amount in tinybars that will be received in the next reward situation. |
| staked_to_me | [int64](#int64) |  | The total of balance of all accounts staked to this account or contract. |
| staked_account_id | [AccountID](#proto-AccountID) |  | The account to which this account or contract is staking. |
| staked_node_id | [int64](#int64) |  | The ID of the node this account or contract is staked to. |






<a name="proto-ThresholdKey"></a>

### ThresholdKey
A set of public keys that are used together to form a threshold signature.  If the threshold is N
and there are M keys, then this is an N of M threshold signature. If an account is associated
with ThresholdKeys, then a transaction to move cryptocurrency out of it must be signed by a list
of M signatures, where at most M-N of them are blank, and the other at least N of them are valid
signatures corresponding to at least N of the public keys listed here.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| threshold | [uint32](#uint32) |  | A valid signature set must have at least this many signatures |
| keys | [KeyList](#proto-KeyList) |  | List of all the keys that can sign |






<a name="proto-ThresholdSignature"></a>

### ThresholdSignature
This message is <b>DEPRECATED</b> and <b>UNUSABLE</b> with network nodes. It is retained
here only for historical reasons.

Please use the SignaturePair and SignatureMap messages.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sigs | [SignatureList](#proto-SignatureList) |  | for an N-of-M threshold key, this is a list of M signatures, at least N of which must be non-null |






<a name="proto-TokenAssociation"></a>

### TokenAssociation
A token - account association


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | The token involved in the association |
| account_id | [AccountID](#proto-AccountID) |  | The account involved in the association |






<a name="proto-TokenBalance"></a>

### TokenBalance
A number of <i>transferable units</i> of a certain token.

The transferable unit of a token is its smallest denomination, as given by the token's
<tt>decimals</tt> property---each minted token contains <tt>10<sup>decimals</sup></tt>
transferable units. For example, we could think of the cent as the transferable unit of the US
dollar (<tt>decimals=2</tt>); and the tinybar as the transferable unit of hbar
(<tt>decimals=8</tt>).

Transferable units are not directly comparable across different tokens.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tokenId | [TokenID](#proto-TokenID) |  | A unique token id |
| balance | [uint64](#uint64) |  | Number of transferable units of the identified token. For token of type FUNGIBLE_COMMON - balance in the smallest denomination. For token of type NON_FUNGIBLE_UNIQUE - the number of NFTs held by the account |
| decimals | [uint32](#uint32) |  | Tokens divide into <tt>10<sup>decimals</sup></tt> pieces |






<a name="proto-TokenBalances"></a>

### TokenBalances
A sequence of token balances


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tokenBalances | [TokenBalance](#proto-TokenBalance) | repeated |  |






<a name="proto-TokenID"></a>

### TokenID
Unique identifier for a token


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | A nonnegative shard number |
| realmNum | [int64](#int64) |  | A nonnegative realm number |
| tokenNum | [int64](#int64) |  | A nonnegative token number |






<a name="proto-TokenRelationship"></a>

### TokenRelationship
Token's information related to the given Account


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tokenId | [TokenID](#proto-TokenID) |  | The ID of the token |
| symbol | [string](#string) |  | The Symbol of the token |
| balance | [uint64](#uint64) |  | For token of type FUNGIBLE_COMMON - the balance that the Account holds in the smallest denomination. For token of type NON_FUNGIBLE_UNIQUE - the number of NFTs held by the account |
| kycStatus | [TokenKycStatus](#proto-TokenKycStatus) |  | The KYC status of the account (KycNotApplicable, Granted or Revoked). If the token does not have KYC key, KycNotApplicable is returned |
| freezeStatus | [TokenFreezeStatus](#proto-TokenFreezeStatus) |  | The Freeze status of the account (FreezeNotApplicable, Frozen or Unfrozen). If the token does not have Freeze key, FreezeNotApplicable is returned |
| decimals | [uint32](#uint32) |  | Tokens divide into <tt>10<sup>decimals</sup></tt> pieces |
| automatic_association | [bool](#bool) |  | Specifies if the relationship is created implicitly. False : explicitly associated, True : implicitly associated. |






<a name="proto-TokenTransferList"></a>

### TokenTransferList
A list of token IDs and amounts representing the transferred out (negative) or into (positive)
amounts, represented in the lowest denomination of the token


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | The ID of the token |
| transfers | [AccountAmount](#proto-AccountAmount) | repeated | Applicable to tokens of type FUNGIBLE_COMMON. Multiple list of AccountAmounts, each of which has an account and amount |
| nftTransfers | [NftTransfer](#proto-NftTransfer) | repeated | Applicable to tokens of type NON_FUNGIBLE_UNIQUE. Multiple list of NftTransfers, each of which has a sender and receiver account, including the serial number of the NFT |
| expected_decimals | [google.protobuf.UInt32Value](#google-protobuf-UInt32Value) |  | If present, the number of decimals this fungible token type is expected to have. The transfer will fail with UNEXPECTED_TOKEN_DECIMALS if the actual decimals differ. |






<a name="proto-TopicID"></a>

### TopicID
Unique identifier for a topic (used by the consensus service)


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| shardNum | [int64](#int64) |  | The shard number (nonnegative) |
| realmNum | [int64](#int64) |  | The realm number (nonnegative) |
| topicNum | [int64](#int64) |  | Unique topic identifier within a realm (nonnegative). |






<a name="proto-TransactionFeeSchedule"></a>

### TransactionFeeSchedule
The fees for a specific transaction or query based on the fee data.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hederaFunctionality | [HederaFunctionality](#proto-HederaFunctionality) |  | A particular transaction or query |
| feeData | [FeeData](#proto-FeeData) |  | **Deprecated.** Resource price coefficients |
| fees | [FeeData](#proto-FeeData) | repeated | Resource price coefficients. Supports subtype price definition. |






<a name="proto-TransactionID"></a>

### TransactionID
The ID for a transaction. This is used for retrieving receipts and records for a transaction, for
appending to a file right after creating it, for instantiating a smart contract with bytecode in
a file just created, and internally by the network for detecting when duplicate transactions are
submitted. A user might get a transaction processed faster by submitting it to N nodes, each with
a different node account, but all with the same TransactionID. Then, the transaction will take
effect when the first of all those nodes submits the transaction and it reaches consensus. The
other transactions will not take effect. So this could make the transaction take effect faster,
if any given node might be slow. However, the full transaction fee is charged for each
transaction, so the total fee is N times as much if the transaction is sent to N nodes.

Applicable to Scheduled Transactions:
 - The ID of a Scheduled Transaction has transactionValidStart and accountIDs inherited from the
   ScheduleCreate transaction that created it. That is to say that they are equal
 - The scheduled property is true for Scheduled Transactions
 - transactionValidStart, accountID and scheduled properties should be omitted


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactionValidStart | [Timestamp](#proto-Timestamp) |  | The transaction is invalid if consensusTimestamp < transactionID.transactionStartValid |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID that paid for this transaction |
| scheduled | [bool](#bool) |  | Whether the Transaction is of type Scheduled or no |
| nonce | [int32](#int32) |  | The identifier for an internal transaction that was spawned as part of handling a user transaction. (These internal transactions share the transactionValidStart and accountID of the user transaction, so a nonce is necessary to give them a unique TransactionID.)

An example is when a "parent" ContractCreate or ContractCall transaction calls one or more HTS precompiled contracts; each of the "child" transactions spawned for a precompile has a id with a different nonce. |






<a name="proto-TransferList"></a>

### TransferList
A list of accounts and amounts to transfer out of each account (negative) or into it (positive).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountAmounts | [AccountAmount](#proto-AccountAmount) | repeated | Multiple list of AccountAmount pairs, each of which has an account and an amount to transfer into it (positive) or out of it (negative) |





 <!-- end messages -->


<a name="proto-HederaFunctionality"></a>

### HederaFunctionality
The transactions and queries supported by Hedera Hashgraph.

| Name | Number | Description |
| ---- | ------ | ----------- |
| NONE | 0 | UNSPECIFIED - Need to keep first value as unspecified because first element is ignored and not parsed (0 is ignored by parser) |
| CryptoTransfer | 1 | crypto transfer |
| CryptoUpdate | 2 | crypto update account |
| CryptoDelete | 3 | crypto delete account |
| CryptoAddLiveHash | 4 | Add a livehash to a crypto account |
| CryptoDeleteLiveHash | 5 | Delete a livehash from a crypto account |
| ContractCall | 6 | Smart Contract Call |
| ContractCreate | 7 | Smart Contract Create Contract |
| ContractUpdate | 8 | Smart Contract update contract |
| FileCreate | 9 | File Operation create file |
| FileAppend | 10 | File Operation append file |
| FileUpdate | 11 | File Operation update file |
| FileDelete | 12 | File Operation delete file |
| CryptoGetAccountBalance | 13 | crypto get account balance |
| CryptoGetAccountRecords | 14 | crypto get account record |
| CryptoGetInfo | 15 | Crypto get info |
| ContractCallLocal | 16 | Smart Contract Call |
| ContractGetInfo | 17 | Smart Contract get info |
| ContractGetBytecode | 18 | Smart Contract, get the runtime code |
| GetBySolidityID | 19 | Smart Contract, get by solidity ID |
| GetByKey | 20 | Smart Contract, get by key |
| CryptoGetLiveHash | 21 | Get a live hash from a crypto account |
| CryptoGetStakers | 22 | Crypto, get the stakers for the node |
| FileGetContents | 23 | File Operations get file contents |
| FileGetInfo | 24 | File Operations get the info of the file |
| TransactionGetRecord | 25 | Crypto get the transaction records |
| ContractGetRecords | 26 | Contract get the transaction records |
| CryptoCreate | 27 | crypto create account |
| SystemDelete | 28 | system delete file |
| SystemUndelete | 29 | system undelete file |
| ContractDelete | 30 | delete contract |
| Freeze | 31 | freeze |
| CreateTransactionRecord | 32 | Create Tx Record |
| CryptoAccountAutoRenew | 33 | Crypto Auto Renew |
| ContractAutoRenew | 34 | Contract Auto Renew |
| GetVersionInfo | 35 | Get Version |
| TransactionGetReceipt | 36 | Transaction Get Receipt |
| ConsensusCreateTopic | 50 | Create Topic |
| ConsensusUpdateTopic | 51 | Update Topic |
| ConsensusDeleteTopic | 52 | Delete Topic |
| ConsensusGetTopicInfo | 53 | Get Topic information |
| ConsensusSubmitMessage | 54 | Submit message to topic |
| UncheckedSubmit | 55 |  |
| TokenCreate | 56 | Create Token |
| TokenGetInfo | 58 | Get Token information |
| TokenFreezeAccount | 59 | Freeze Account |
| TokenUnfreezeAccount | 60 | Unfreeze Account |
| TokenGrantKycToAccount | 61 | Grant KYC to Account |
| TokenRevokeKycFromAccount | 62 | Revoke KYC from Account |
| TokenDelete | 63 | Delete Token |
| TokenUpdate | 64 | Update Token |
| TokenMint | 65 | Mint tokens to treasury |
| TokenBurn | 66 | Burn tokens from treasury |
| TokenAccountWipe | 67 | Wipe token amount from Account holder |
| TokenAssociateToAccount | 68 | Associate tokens to an account |
| TokenDissociateFromAccount | 69 | Dissociate tokens from an account |
| ScheduleCreate | 70 | Create Scheduled Transaction |
| ScheduleDelete | 71 | Delete Scheduled Transaction |
| ScheduleSign | 72 | Sign Scheduled Transaction |
| ScheduleGetInfo | 73 | Get Scheduled Transaction Information |
| TokenGetAccountNftInfos | 74 | Get Token Account Nft Information |
| TokenGetNftInfo | 75 | Get Token Nft Information |
| TokenGetNftInfos | 76 | Get Token Nft List Information |
| TokenFeeScheduleUpdate | 77 | Update a token's custom fee schedule, if permissible |
| NetworkGetExecutionTime | 78 | Get execution time(s) by TransactionID, if available |
| TokenPause | 79 | Pause the Token |
| TokenUnpause | 80 | Unpause the Token |
| CryptoApproveAllowance | 81 | Approve allowance for a spender relative to the owner account |
| CryptoDeleteAllowance | 82 | Deletes granted allowances on owner account |
| GetAccountDetails | 83 | Gets all the information about an account, including balance and allowances. This does not get the list of account records. |
| EthereumTransaction | 84 | Ethereum Transaction |
| NodeStakeUpdate | 85 | Updates the staking info at the end of staking period to indicate new staking period has started. |
| UtilPrng | 86 | Generates a pseudorandom number. |
| TransactionGetFastRecord | 87 | Get a record for a transaction. |
| TokenUpdateNfts | 88 | Update the metadata of one or more NFT's of a specific token type. |
| NodeCreate | 89 | Create a node |
| NodeUpdate | 90 | Update a node |
| NodeDelete | 91 | Delete a node |
| NodeGetInfo | 92 | Get Node information |



<a name="proto-SubType"></a>

### SubType
Allows a set of resource prices to be scoped to a certain type of a HAPI operation.

For example, the resource prices for a TokenMint operation are different between minting fungible
and non-fungible tokens. This enum allows us to "mark" a set of prices as applying to one or the
other.

Similarly, the resource prices for a basic TokenCreate without a custom fee schedule yield a
total price of $1. The resource prices for a TokenCreate with a custom fee schedule are different
and yield a total base price of $2.

| Name | Number | Description |
| ---- | ------ | ----------- |
| DEFAULT | 0 | The resource prices have no special scope |
| TOKEN_FUNGIBLE_COMMON | 1 | The resource prices are scoped to an operation on a fungible common token |
| TOKEN_NON_FUNGIBLE_UNIQUE | 2 | The resource prices are scoped to an operation on a non-fungible unique token |
| TOKEN_FUNGIBLE_COMMON_WITH_CUSTOM_FEES | 3 | The resource prices are scoped to an operation on a fungible common token with a custom fee schedule |
| TOKEN_NON_FUNGIBLE_UNIQUE_WITH_CUSTOM_FEES | 4 | The resource prices are scoped to an operation on a non-fungible unique token with a custom fee schedule |
| SCHEDULE_CREATE_CONTRACT_CALL | 5 | The resource prices are scoped to a ScheduleCreate containing a ContractCall. |



<a name="proto-TokenFreezeStatus"></a>

### TokenFreezeStatus
Possible Freeze statuses returned on TokenGetInfoQuery or CryptoGetInfoResponse in
TokenRelationship

| Name | Number | Description |
| ---- | ------ | ----------- |
| FreezeNotApplicable | 0 | UNDOCUMENTED |
| Frozen | 1 | UNDOCUMENTED |
| Unfrozen | 2 | UNDOCUMENTED |



<a name="proto-TokenKeyValidation"></a>

### TokenKeyValidation
Types of validation strategies for token keys.

| Name | Number | Description |
| ---- | ------ | ----------- |
| FULL_VALIDATION | 0 | Currently the default behaviour. It will perform all token key validations. |
| NO_VALIDATION | 1 | Perform no validations at all for all passed token keys. |



<a name="proto-TokenKycStatus"></a>

### TokenKycStatus
Possible KYC statuses returned on TokenGetInfoQuery or CryptoGetInfoResponse in TokenRelationship

| Name | Number | Description |
| ---- | ------ | ----------- |
| KycNotApplicable | 0 | UNDOCUMENTED |
| Granted | 1 | UNDOCUMENTED |
| Revoked | 2 | UNDOCUMENTED |



<a name="proto-TokenPauseStatus"></a>

### TokenPauseStatus
Possible Pause statuses returned on TokenGetInfoQuery

| Name | Number | Description |
| ---- | ------ | ----------- |
| PauseNotApplicable | 0 | Indicates that a Token has no pauseKey |
| Paused | 1 | Indicates that a Token is Paused |
| Unpaused | 2 | Indicates that a Token is Unpaused. |



<a name="proto-TokenSupplyType"></a>

### TokenSupplyType
Possible Token Supply Types (IWA Compatibility).
Indicates how many tokens can have during its lifetime.

| Name | Number | Description |
| ---- | ------ | ----------- |
| INFINITE | 0 | Indicates that tokens of that type have an upper bound of Long.MAX_VALUE. |
| FINITE | 1 | Indicates that tokens of that type have an upper bound of maxSupply, provided on token creation. |



<a name="proto-TokenType"></a>

### TokenType
Possible Token Types (IWA Compatibility).
Apart from fungible and non-fungible, Tokens can have either a common or unique representation.
This distinction might seem subtle, but it is important when considering how tokens can be traced
and if they can have isolated and unique properties.

| Name | Number | Description |
| ---- | ------ | ----------- |
| FUNGIBLE_COMMON | 0 | Interchangeable value with one another, where any quantity of them has the same value as another equal quantity if they are in the same class. Share a single set of properties, not distinct from one another. Simply represented as a balance or quantity to a given Hedera account. |
| NON_FUNGIBLE_UNIQUE | 1 | Unique, not interchangeable with other tokens of the same type as they typically have different values. Individually traced and can carry unique properties (e.g. serial number). |


 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->



