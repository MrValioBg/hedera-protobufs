## Table of Contents

- [node_get_info.proto](#node_get_info-proto)
    - [NodeGetInfoQuery](#proto-NodeGetInfoQuery)
    - [NodeGetInfoResponse](#proto-NodeGetInfoResponse)
    - [NodeInfo](#proto-NodeInfo)
  



<a name="node_get_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## node_get_info.proto



<a name="proto-NodeGetInfoQuery"></a>

### NodeGetInfoQuery
Gets information about Node instance. This needs super use privileges to succeed or should be a node operator.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.<br/> This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| node_id | [uint64](#uint64) |  | A node identifier for which information is requested.<br/> If the identified node is not valid, this request SHALL fail with a response code `INVALID_NODE_ID`. |






<a name="proto-NodeGetInfoResponse"></a>

### NodeGetInfoResponse
Response when the client sends the node NodeGetInfoQuery


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.<br/> This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| nodeInfo | [NodeInfo](#proto-NodeInfo) |  | The information requested about this node instance |






<a name="proto-NodeInfo"></a>

### NodeInfo
A query response describing the current state of a node


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | A numeric node identifier.<br/> This value identifies this node within the network address book. |
| account_id | [AccountID](#proto-AccountID) |  | The account is charged for transactions submitted by the node that fail due diligence |
| description | [string](#string) |  | A description of the node with UTF-8 encoding up to 100 bytes |
| gossip_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A node's Gossip Endpoints, ip address and port |
| service_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A node's service Endpoints, ip address or FQDN and port |
| gossip_ca_certificate | [bytes](#bytes) |  | The node's X509 certificate used to sign stream files (e.g., record stream files). Precisely, this field is the DER encoding of gossip X509 certificate. files). |
| grpc_certificate_hash | [bytes](#bytes) |  | node x509 certificate hash. Hash of the node's TLS certificate. Precisely, this field is a string of hexadecimal characters which, translated to binary, are the SHA-384 hash of the UTF-8 NFKD encoding of the node's TLS cert in PEM format. Its value can be used to verify the node's certificate it presents during TLS negotiations. |
| weight | [uint64](#uint64) |  | The consensus weight of this node in the network. |
| deleted | [bool](#bool) |  | Whether the node has been deleted |
| ledger_id | [bytes](#bytes) |  | A ledger ID.<br/> This identifies the network that responded to this query. The specific values are documented in [HIP-198] (https://hips.hedera.com/hip/hip-198). |





 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->


