# Use Cases

At its core, ZKM's initial use cases are focused on verifiable and succinctness of computation. Other privacy focused use-cases can be achieved, but it is not the direct focus of ZKM.

## Hybrid Rollup

ZKM has valuable applications in Layer 2 (L2) blockchain solutions. L2 solutions aim to alleviate the scalability limitations of the underlying Layer 1 blockchain by processing transactions off-chain. Using ZKM, participants can cryptographically prove the validity of off-chain transactions in the Ethereum blockchain without revealing the specific details of each transaction on the public blockchain. This ensures the integrity of transactions.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

In addition, multiple transactions can be aggregated into a single proof, reducing the computational overhead required for verification and enhancing scalability. The provided proof guarantees the correctness of the transaction execution and hence reduces the withdrawal time of the processed fund.

With a Hybrid Rollup, merging the cost effectiveness of Optimisic Rollups, with the succinctness and verifiability of Validity Proofs allows for the ability to choose between an immediate, slightly expensive withdrawal, or a longer, cost-effective Optimistic withdrawal. The ability for the user to choose their preferred method of withdrawal gives the end user more control over their costs of using a Layer 2, and enables more flexible finalization methods for bridging operations.

### Bitcoin L2

Using an Optimistic Challenge Process and Bitcoin Taproot, we can verify an arbitrary computation which enables to prove computation directly on Bitcoin, which leverages Bitcoin to directly secure transactions for any L2 on Bitcoin both EVM and non-EVM.

## Entangled Rollup

The Entangled Rollup structure allows for trustless communication between two rollups. This mechanism allows for communicating between two incompatible blockchains using Layer 2s, which enables transactions to be executed, proved, and executed again on a destination L2 while leveraging the security and cost-effectiveness of a Rollup.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

The ability to connect two incompatible blockchains enables additional use-cases and optimizations, such as the Universal L2 Extension.

### Universal L2 Extension

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

The Universal L2 extends the Entangled Rollup structure to focus on solving the issue of fragmented liquidity across ecosystems. Using a proof-of-burn mechanism and the native mint function of a Rollup, a user is able to burn and mint native assets to any Layer 2 that implements the Universal L2 structure. More details of this structure can be found [here](https://settlement.network/docs/native/entangled-rollup/universal-l2).

## zkML

In the machine learning (ML) world, the use of Validity proofs has the potential to verify the application of a given ML model. This is referred to as zero-knowledge machine learning (zkML). Since the weight values within an ML model are considered proprietary information held by service providers, clients should have the means to verify the correctness of the results without accessing the trained model. Moreover, when a third party examines the results, it should be possible to confirm their accuracy even without the user’s input data. The figure below shows a scenario in medical systems where a patient sends their ECG to an ML Service Provider, and the processed result is sent back to their physician. As depicted, the physician requires a means to verify the correctness of the process.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

The utilization of ZKM can be expanded across various applications and scenarios. Consequently, it has become crucial for users to take advantage of ML computations without revealing the input values in order to preserve users’ privacy.
