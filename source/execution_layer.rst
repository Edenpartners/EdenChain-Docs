.. image:: images/Architecture/Execution_Layer.png
    :width: 750px

Execution Layer
===============

The Execution Layer (EL) executes, processes, and verifies
transactions, providing important functions for the smart
contracts running on the Eden platform.

Transactions must be used in order to update the data in
Eden's distributed ledger. The EL provides a Trusted
Execution Environment (TEE) in which the transaction can be
safely protected from data extortion or data forgery by
hacker attacks. All transactions and key logic including
smart contracts are executed in the TEE.

The EL has Transaction Execution Scheduling (TES) which
specifies how transactions are to be executed and on which
node. TES is an important technical factor that affects
performance and can show different processing performances
with the same computing resources depending on how
transactions are managed and executed. TES also includes an
Ethereum Virtual Machine (EVM) to run smart contracts
written in Solidity. If a transaction being executed is a
smart contract running on Ethereum, the EL executes the EVM.