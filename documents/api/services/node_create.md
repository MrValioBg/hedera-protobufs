## Table of Contents

- [node_create.proto](#node_create-proto)
    - [NodeCreateTransactionBody](#proto-NodeCreateTransactionBody)
  



<a name="node_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## node_create.proto



<a name="proto-NodeCreateTransactionBody"></a>

### NodeCreateTransactionBody
A transaction body to add a new node to the network.
After the node is created, the node_id for it is in the receipt.

This transaction body SHALL be considered a "privileged transaction".

This message supports a transaction to create a new node in the network.
The transaction, once complete, enables a new consensus node
to join the network, and requires governing council authorization.

A `NodeCreateTransactionBody` MUST be signed by the governing council.<br/>
The newly created node information will be used to generate config.txt and
a-pulbic-NodeAlias.pem file per each node during phase 2,<br>
not active until next freeze upgrade.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [AccountID](#proto-AccountID) |  | Node account id, mandatory field, ALIAS is not allowed, only ACCOUNT_NUM. If account_id does not exist, it will reject the transaction. Multiple nodes can have the same account_id. |
| description | [string](#string) |  | Description of the node with UTF-8 encoding up to 100 bytes, optional field. |
| gossip_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | Ip address and port, mandatory field. Fully qualified domain name is not allowed here. Maximum number of these endpoints is 10. The first in the list is used as the Internal IP address in config.txt, the second in the list is used as the External IP address in config.txt, the rest of IP addresses are ignored for DAB phase 2. |
| service_endpoint | [ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A node's grpc service IP addresses and ports, IP:Port is mandatory, fully qualified domain name is optional. Maximum number of these endpoints is 8. |
| gossip_ca_certificate | [bytes](#bytes) |  | The node's X509 certificate used to sign stream files (e.g., record stream files). Precisely, this field is the DER encoding of gossip X509 certificate. This is a mandatory field. |
| grpc_certificate_hash | [bytes](#bytes) |  | Hash of the node's TLS certificate. Precisely, this field is a string of hexadecimal characters which translated to binary, are the SHA-384 hash of the UTF-8 NFKD encoding of the node's TLS cert in PEM format. Its value can be used to verify the node's certificate it presents during TLS negotiations.node x509 certificate hash, optional field. |





 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->

 <!-- end services -->


