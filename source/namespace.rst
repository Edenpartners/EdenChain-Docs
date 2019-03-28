.. image:: images/EdenChain_Introduction/Namespace.png
    :width: 750px

Namespace
===============


Eden uses a Radix Merkle Tree to store a current state of
the blockchain. Validator nodes that check conformity of
blocks all contain Radix Merkle Tree. Radix Merkle Tree
displays some data with optimal space. If there is only one
child node, it unites the nodes into one, so it can
effectively use memory.

In a leaf node of the Radix Merkle Tree, a node address is
included, and thus it is possible to identify a sibling or a
parent of the node by the node address value. A validator
node examines a node address included in a transaction
within a block and a batch to verify the transaction.

Node Address = Namespace + Node Path

| 

| 

A namespace is a form of identification value for
ascertaining the type of transaction and all transactions in
Eden must contain namespace information. Validator nodes can
use the namespace information to group transactions into
blocks of related transactions. For example, for a
transaction that contains simple transactional information,
the namespace "EDN" is used, and for smart contract XYZ, a
namespace "XYZ" is used. The validator node can distinguish
XYZ-related transactions from EDN-related transactions by
simply checking a namespace contained in the transaction.
Since EDN and XYZ are different types of transactions there
is no data consistency problem and both transactions can be
executed in parallel. As a result, it is no longer necessary
to execute one transaction at a time due to data consistency
issues as is the case for many existing solutions in the
blockchain space.

| 

| 