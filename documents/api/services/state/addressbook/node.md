## Table of Contents

- [state/addressbook/node.proto](#state_addressbook_node-proto)
    - [Node](#proto-Node)
  
    - [NodeStatus](#proto-NodeStatus)
  



<a name="state_addressbook_node-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## state/addressbook/node.proto



<a name="proto-Node"></a>

### Node
Representation of a Node in the network Merkle tree

A Node is identified by a single uint64 number, which is unique among all nodes.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The unique id of the Node. |
| account_id | [AccountID](#proto-AccountID) |  | The account is charged for transactions submitted by the node that fail due diligence |
| description | [string](#string) |  | A description of the node, with UTF-8 encoding up to 100 bytes |
| gossip_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | Node Gossip Endpoints, ip address or FQDN and port |
| service_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A node's service Endpoints, ip address or FQDN and port |
| gossip_ca_certificate | [bytes](#bytes) |  | The node's X509 certificate used to sign stream files (e.g., record stream files). Precisely, this field is the DER encoding of gossip X509 certificate. |
| grpc_certificate_hash | [bytes](#bytes) |  | node x509 certificate hash. Hash of the node's TLS certificate. Precisely, this field is a string of hexadecimal characters which, translated to binary, are the SHA-384 hash of the UTF-8 NFKD encoding of the node's TLS cert in PEM format. Its value can be used to verify the node's certificate it presents during TLS negotiations. |
| weight | [uint64](#uint64) |  | The consensus weight of this node in the network. |





 <!-- end messages -->


<a name="proto-NodeStatus"></a>

### NodeStatus


| Name | Number | Description |
| ---- | ------ | ----------- |
| DELETED | 0 | node in this state is deleted |
| PENDING_ADDITION | 1 | node in this state is waiting to be added by consensus roster |
| PENDING_DELETION | 2 | node in this state is waiting to be deleted by consensus roster |
| IN_CONSENSUS | 3 | node in this state is active on the network and participating in network consensus. |


 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->


