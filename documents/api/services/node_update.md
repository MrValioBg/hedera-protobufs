## Table of Contents

- [node_update.proto](#node_update-proto)
    - [NodeUpdateTransactionBody](#proto-NodeUpdateTransactionBody)
  



<a name="node_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## node_update.proto



<a name="proto-NodeUpdateTransactionBody"></a>

### NodeUpdateTransactionBody
Modify the attribute of a node. If a field is not set in the transaction body, the
corresponding node attribute will be unchanged.
For phase 2, this marks node to be updated in the merkle tree and will be used to write config.txt and
a-pulbic-NodeAlias.pem file per each node during prepare freeze.
The node will not be updated until the network is upgraded.
Original node account ID has to sign the transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The unique id of the Node to be updated. This must refer to an existing, non-deleted node. |
| account_id | [AccountID](#proto-AccountID) |  | If set, the new node account_id. |
| description | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | If set, the new description to be associated with the node. |
| gossip_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | If set, the new ip address and port. |
| service_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | If set, replace the current list of service_endpoints. |
| gossip_ca_certificate | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | If set, the new X509 certificate of the gossip node. |
| grpc_certificate_hash | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | If set, the new grpc x509 certificate hash. |





 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->


