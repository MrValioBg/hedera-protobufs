## Table of Contents

- [address_book_service.proto](#address_book_service-proto)
    - [AddressBookService](#proto-AddressBookService)
  



<a name="address_book_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## address_book_service.proto


 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->


<a name="proto-AddressBookService"></a>

### AddressBookService
Transactions for the AddressBook Service, those HAPI APIs facilitate changes to the nodes used across the Hedera network.
All those transactions needs to be signed by the Hedera Council. Steps needed below:
1. The node operator creates and signs the transaction with their key (the key on the node operator account)
2. The node operator hands this transaction to Alex, who then gives it to the council to sign
3. When signed and submitted, the server will verify that account 2 keys have signed, and the keys on the operator account have signed.
Hedera council should have ability to make all edits in addition to add/remove nodes

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| createNode | [Transaction](#proto-Transaction) | [TransactionResponse](#proto-TransactionResponse) | Prepare to add a new node to the network. When a valid council member initiates a HAPI transaction to add a new node, then the network should acknowledge the transaction and update the network’s Address Book within 24 hours. The added node will not be active until the network is upgraded. |
| deleteNode | [Transaction](#proto-Transaction) | [TransactionResponse](#proto-TransactionResponse) | Prepare to delete the node to the network. The deleted node will not be deleted until the network is upgraded. Such a deleted node can never be reused. |
| updateNode | [Transaction](#proto-Transaction) | [TransactionResponse](#proto-TransactionResponse) | Prepare to update the node to the network. The node will not be updated until the network is upgraded. |
| getNodeInfo | [Query](#proto-Query) | [Response](#proto-Response) | Retrieves the node information by node Id. |

 <!-- end services -->


