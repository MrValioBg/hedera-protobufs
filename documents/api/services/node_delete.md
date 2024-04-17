## Table of Contents

- [node_delete.proto](#node_delete-proto)
    - [NodeDeleteTransactionBody](#proto-NodeDeleteTransactionBody)
  



<a name="node_delete-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## node_delete.proto



<a name="proto-NodeDeleteTransactionBody"></a>

### NodeDeleteTransactionBody
Delete the given node. After deletion, it will be marked as deleted.
But information about it will continue to exist for a year.
For phase 2, this marks the node to be deleted in the merkle tree and will be used to write config.txt and
a-pulbic-NodeAlias.pem file per each node during prepare freeze.
The deleted node will not be deleted until the network is upgraded.
Such a deleted node can never be reused.
The council has to sign this transaction. This is a privileged transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The unique id of the node to be deleted. If invalid node is specified, transaction will result in INVALID_NODE_ID. |





 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->


