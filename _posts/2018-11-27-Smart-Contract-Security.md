---
layout: post
title: Smart Contract Security 
author: laszkaaron
author_name: "Áron Lászka"
author_web: ""
featured-img: SmartContractSecurity
seo_tags: "smart contracts, secure smart contracts, solidity, solidity vulnerabilities"
---

Blockchain-based platforms are becoming increasingly popular due to their ability to maintain a public distributed ledger, providing reliability, integrity, and auditability for transactions without a trusted entity. **Early blockchain platforms, such as Bitcoin**, focused solely on creating cryptocurrencies and payment systems. In contrast, more recent platforms, such as **Ethereum, can also act as distributed computing platforms**, enabling the deployment and execution of software code in the form of smart contracts. The key advantage of smart contract based distributed applications is that the security guarantees of the underlying platform safeguard the state and execution of smart contracts. 

<!--excerpt-->

----

However, the trustworthiness of execution does not prevent or mitigate the occurrence of software bugs and vulnerabilities in smart contracts. Risks posed by smart-contract vulnerabilities are exacerbated by the typical design of blockchain based platforms, which does not allow the code of a contract to be updated (e.g., to fix a vulnerability) or a malicious transaction to be reverted. In practice, a significant number of contracts suffer from software vulnerabilities, as illustrated by a recent automated analysis of contracts that are deployed on the public Ethereum blockchain. [The report found that 8,333 out of 19,336 contracts suffer from at least one security issue](https://github.com/melonproject/oyente). While not all of these issues lead to security vulnerabilities, some of them enable stealing cryptocurrencies and other digital assets. For example, $50 million worth of cryptocurrency was stolen in the [“DAO attack”](https://www.wired.com/2016/06/50-million-hack-just-showed-dao-human/), and $280 million worth of cryptocurrency was lost in the [2017 hack of the multisignature Parity Wallet library](https://www.wired.com/story/280m-worth-of-ethereum-is-trapped-for-a-pretty-dumb-reason/).

Contract vulnerabilities are often introduced due to the semantic gap between the assumptions that contract developers make about the underlying execution semantics and the actual semantics of smart contracts. For example, the **syntax of Solidity, the most widely used high-level language for developing Ethereum contracts, resembles JavaScript, but its semantics are quite peculiar**. As a result, developers who are not familiar with the intricacies of smart contract development tend to make common mistakes, which result in security vulnerabilities. Below, we discuss some of the most common types of vulnerabilities.

## Common Vulnerability Types

Motivated by a large number of smart-contract vulnerabilities, many researchers have provided surveys and taxonomies of common [vulnerability types](https://github.com/melonproject/oyente) [4]. Here, we list some of the most typical vulnerabilities in Ethereum smart contracts, focusing on Solidity.

## Re-Entrancy

Re-entrancy is one of the most well-known culprits behind contract vulnerabilities. On Ethereum, when a contract calls a function in another contract, the execution of the caller is blocked until the call returns. This allows the callee, who may be malicious, to take advantage of the intermediate state in which the caller is, e.g., by invoking a function in the caller. For example, in a cryptocurrency-withdrawal transaction, a malicious callee may be able to initiate another withdrawal before the caller decreases its balance. A similar re-entrancy vulnerability was exploited in the infamous “The DAO” attack.

## Transaction-Ordering Dependence

If multiple users invoke functions in the same contract, the order in which their calls are executed cannot be predicted. As a result, users have uncertain knowledge of the state in which a contract will be when their calls are executed. For example, in a contract that implements an online marketplace, one user might be able to change the price of an item before another user’s purchasing transaction is executed.

## “Denial of Service”
If a function involves sending ether using `transfer` or making a high-level function call to another contract, then the recipient contract can “block” the execution of this function by always throwing an exception.

## Deadlock
A contract may end up in a “deadlock” state (either accidentally or through adversarial action), in which it is no longer possible to withdraw or transfer currency from the contract. This means that the currency stored in the contract is practically lost, similar to what happened to the Parity multisignature wallet contracts.

## Timestamp Dependence

In Ethereum, a malicious miner can alter the clock that is used in the execution of smart contracts by several minutes. More specifically, a miner can choose the timestamp of a block arbitrarily, but other miners will accept the block only if its timestamp is within a certain range of their clocks. As result, contracts whose correctness depends on the accuracy of the clock may be vulnerable. For example, a contract that uses the clock for generating random numbers may be exploited by a miner who chooses the block timestamp to generate the random number in its favor.

## Mishandled Exceptions

Depending on how a function is called, an exception in the callee may or may not be propagated to the caller. Due to the inconsistency of the propagation policy, developers can quickly forget to properly handle exceptions in callee contracts.

## Contract Verification and Vulnerability Discovery Tools

To find and patch contract vulnerabilities before deployment, developers can employ contract verification and vulnerability discovery tools [5]. 

* [**Oyente**](https://github.com/melonproject/oyente) is an analysis tool for Ethereum contracts that can detect certain typical security vulnerabilities. 

* [**Securify**](https://securify.chainsecurity.com/) is a security analyzer for Ethereum contracts, which symbolically encodes the dependence graph of a contract in stratified Datalog and then uses off-the-shelf solvers to check the satisfaction of desired properties.

* [**EthIR**](https://github.com/costa-group/ethIR) is an analysis tool built on Oyente, which builds control-flow graphs of Ethereum bytecode and uses it to search for vulnerabilities. EthIR generates a high-level, rule-based representation of contract, which enables the application of existing analysis to infer properties.

* [**MAIAN**](https://github.com/MAIAN-tool/MAIAN) is a tool for the automatic detection of three types of vulnerable contracts, called prodigal, suicidal, and greedy, by analyzing Ethereum bytecode.

* [**Vandal**](https://github.com/usyd-blockchain/vandal) is a security analysis framework for Ethereum smart contracts, which converts EVM bytecode to semantic relations, which are then analyzed to detect vulnerabilities described in the Soufflé language.

* [**Mythril**](https://github.com/ConsenSys/mythril-classic) [10] is a security analysis tool for Ethereum smart contracts with a symbolic execution backend.

Besides contract verification and vulnerability discovery, developers may also use correct-by-design frameworks and runtime verification.

* [**VeriSolid**](http://aronlaszka.com/papers/mavridou2019verisolid.pdf) is a [framework](https://github.com/anmavrid/smart-contracts) for the correct-by-design development of Ethereum smart contracts. VeriSolid allows developers to specify contracts using a transition-system based model with rigorous semantics, supported by an open-source, web-based, graphical IDE, to reason about and verify contract behavior at a high level of abstraction, and to generate Solidity code from verified models.

* [ContractLarva](https://github.com/gordonpace/contractLarva) is a tool that enables extending contracts 
(a) to detect violations at runtime and 
(b) to offer monetary reparations in response to a violation.

## References 

[4] Atzei, N., Bartoletti, M., Cimoli, T.: A survey of attacks on Ethereum smart contracts (SoK). In: Proceedings of the 6th International Conference on Principles of Security and Trust (POST). pp. 164–186. Springer (April 2017)

[5] Parizi, R.M., Dehghantanha, A., Choo, K.K.R., Singh, A.: Empirical vulnerability analysis of automated smart contracts security testing on blockchains. In: 28th Annual International Conference on Computer Science and Software Engineering (CASCON) (2018)


