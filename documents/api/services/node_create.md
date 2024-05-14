# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [node_create.proto](#node_create-proto)
    - [NodeCreateTransactionBody](#com-hedera-hapi-node-addressbook-NodeCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="node_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## node_create.proto



<a name="com-hedera-hapi-node-addressbook-NodeCreateTransactionBody"></a>

### NodeCreateTransactionBody
A transaction body to add a new consensus node to the network address book.

This transaction body SHALL be considered a "privileged transaction".

This message supports a transaction to create a new node in the network
address book. The transaction, once complete, enables a new consensus node
to join the network, and requires governing council authorization.

- A `NodeCreateTransactionBody` MUST be signed by the governing council.
- The newly created node information SHALL be added to the network address
  book information in the network state.
- The new entry SHALL be created in "state" but SHALL NOT participate in
  network consensus and SHALL NOT be present in network "configuration"
  until the next "upgrade" transaction (as noted below).
- All new address book entries SHALL be added to the active network
  configuration during the next `freeze` transaction with the field
  `freeze_type` set to `PREPARE_UPGRADE`.

### Record Stream Effects
Upon completion the newly assigned `node_id` SHALL be in the transaction
receipt.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [proto.AccountID](#proto-AccountID) |  | A Node account identifier. <p> This account identifier MUST be in the "account number" form.<br/> This account identifier MUST NOT use the alias field.<br/> If the identified account does not exist, this transaction SHALL fail.<br/> Multiple nodes MAY share the same node account.<br/> This field is REQUIRED. |
| description | [string](#string) |  | A short description of the node. <p> This value, if set, MUST NOT exceed 100 bytes when encoded as UTF-8.<br/> This field is OPTIONAL. |
| gossip_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gossip. <p> These endpoints SHALL represent the published endpoints to which other consensus nodes may _gossip_ transactions.<br/> This list MUST NOT be empty.<br/> This list MUST NOT contain more than `10` entries.<br/> The first two entries in this list SHALL be the endpoints published to all consensus nodes.<br/> All other entries SHALL be reserved for future use. <p> Each network may have additional requirements for these endpoints. A client MUST check network-specific documentation for those details.<br/> <blockquote>Example<blockquote> Hedera Mainnet _requires_ that address be specified, and does not permit DNS name (FQDN) to be specified.<br/> Mainnet also requires that the first entry be an "internal" IP address and the second entry be an "external" IP address. </blockquote> <blockquote> Solo, however, _requires_ DNS name (FQDN) but also permits address. </blockquote></blockquote> |
| service_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gRPC calls. <p> These endpoints SHALL represent the published gRPC endpoints to which clients may submit transactions.<br/> These endpoints SHOULD specify address and port.<br/> These endpoints MAY specify a DNS name.<br/> This list MUST NOT be empty.<br/> This list MUST NOT contain more than `8` entries. <p> Each network may have additional requirements for these endpoints. A client MUST check network-specific documentation for those details. |
| gossip_ca_certificate | [bytes](#bytes) |  | A certificate used to sign gossip events. <p> This value MUST be a certificate of a type permitted for gossip signatures.<br/> This value MUST be the DER encoding of the certificate presented.<br/> This field is REQUIRED and MUST NOT be empty. |
| grpc_certificate_hash | [bytes](#bytes) |  | A hash of the node gRPC TLS certificate. <p> This value MAY be used to verify the certificate presented by the node during TLS negotiation for gRPC.<br/> This value MUST be a SHA-384 hash.<br/> The TLS certificate to be hashed MUST first be in PEM format and MUST be encoded with UTF-8 NFKD encoding to a stream of bytes provided to the hash algorithm.<br/> This field is OPTIONAL. |





 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->



