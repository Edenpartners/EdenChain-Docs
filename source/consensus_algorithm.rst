.. image:: images/EdenChain_Introduction/Consensus_Alogorithm.png
    :width: 750px

Consensus Algorithm
========================

POET
------

The consensus algorithm plays an important role in a
blockchain technique. There are two approaches. The first is
"Nakamoto Consensus," which is a way to conduct a leader
selection through a lottery process. When selected as a
leader, one has the right to authenticate a previous block
and to create a new block. In case of Bitcoin, a node that
solves a hash puzzle first is selected as the leader. The
second method uses "BFT (Byzantine Fault Tolerance)." This
method does not select a leader and a final agreement is
reached through several stages of voting.

Eden uses Proof-of-Elapsed-Time (PoET) as a consensus
algorithm. PoET is a "Nakamoto Consensus" method, which uses
a CPU command to select a leader randomly without using
enormous levels of energy to solve a hash problem like
Bitcoin currently does. PoET provides an opportunity to
become a leader with block generation authority for all
nodes participating in a blockchain network with a
probability similar to of other leader selection algorithms
(Foundation, 2017). PoET is implemented in an SGX enclave so
as to defend against hacker attacks and to allow the leader
selection process to proceed safely. At each node, PoET uses
a CPU command in the SGX enclave to obtain a wait time that
follows an exponential distribution as a random number and
selects the node that has the smallest wait time as the
leader.

| 

| 

PoET is designed to follow the Poisson distribution, which
is a form of discrete probability distribution that follows
the exponential distribution shown below and expresses how
many times a certain number of events occur within a unit
time if the event is independent.
