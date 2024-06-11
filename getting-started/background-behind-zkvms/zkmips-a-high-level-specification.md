---
description: >-
  Following the recent release of the updated zkMIPS paper by ZKM Research, we
  hope to address the key questions that followed from our community. Here, we
  present a broad Q&A with insights into the pap
---

# zkMIPS: a High-Level Specification

**In simple terms, what is zkMIPS?**

zkMIPS is the first zero-knowledge virtual machine (zkVM) that supports the MIPS instruction set architecture. MIPS (Microprocessor without Interlocked Pipeline Stages) is a stable and widely adopted 32/64-bit computer architecture, particularly popular in IoT devices. zkMIPS leverages this stability to implement a zero-knowledge proof system for verifiable computing​​.\
\
**At a basic level, how does zkMIPS work?**\
\
zkMIPS uses Plonky2 to prove the correctness of any program compiled to MIPS. This is achieved by breaking the program into small segments and proving each of them using STARK. The generated proofs are recursively combined using PLONK, resulting in a constant-size proof that can be verified on or off-chain.\
\
**Why was there a need for an updated whitepaper?**

Deviations from the original paper were made during the development of zkMIPS. The new version corrects the discrepancies between the documentation and the codebase to more accurately reflect the current state of zkMIPS, providing clear and accurate information to developers.

#### What are the key updates in the new whitepaper?

The new whitepaper provides a comprehensive overview of the MIPS instruction set and how zkMIPS compiles a program to a ZK proof. It details the high-level specifics of the proving routine, including how general-purpose registers, memory states, and instruction are managed. Furthermore, the paper offers a detailed explanation of how computation steps are transformed into polynomial forms, which is essential for creating succinct proofs. This involves encoding the computation trace as polynomials, ensuring efficient verification through arithmetization, and the use of interactive oracle proofs (IOPs)​​ like the STARKs and LogUp used to efficiently verify polynomial properties​​. In the coming weeks, the paper will be updated with a detailed description of the polynomial codification of each MIPS instruction.\
\
**How can this paper be used, and by who?**

This whitepaper bridges the gap between theoretical concepts and practical implementation. It ensures transparency and accuracy in zkMIPS documentation, making it an invaluable resource for various audiences. For developers, it simplifies understanding and working with the codebase. The explanations and examples provided in the document make the codebase more accessible and less daunting​​. The paper also connects academic knowledge with real-world applications, serving as a comprehensive guide to cover everything from the basics of zero-knowledge proofs to the specific implementation details of zkMIPS​​. Overall, it provides a detailed yet accessible overview of zkMIPS in a way that is more complete and accessible than any material previously published by ZKM Research​​.\
\
**Why is zkMIPS significant?**

zkMIPS is significant because it is the first zkVM to support the MIPS instruction set, a widely adopted and stable CPU architecture. By providing a verifiable computing environment for a well-established CPU architecture, zkMIPS opens up new possibilities for developers and researchers to explore zero-knowledge proofs in practical applications.

#### Why did ZKM choose MIPS over RISC?

MIPS offers greater stability and compatibility with higher-level languages, making it a more reliable choice for zkMIPS. Its long-standing presence in the industry and unchanged instruction set provide a stable foundation for developing a zkVM. In contrast, RISC-V allows custom instructions and is less stable over time, making MIPS a preferable choice​​.\
\
**What can we expect from future updates?**

ZKM Research will strive to make periodic updates to ensure that the documentation remains aligned with the evolving zkMIPS codebase​​. Future updates will include information on the design of MIPS instructions as polynomials and will also explore practical decisions made during the zkVM's development.

**How can I contribute to zkMIPS?**\
\
ZKM Research is inviting collaborations to help improve this ever-evolving work. To offer your much appreciated feedback, please join the discussion in our [Discord Server](https://discord.com/channels/1125877344972849232/1246097911239016509), or to make a code contribution, proceed to the [zkMIPS GitHub](https://github.com/zkMIPS).[\
\
](https://github.com/zkMIPS)**Do I need substantial hardware resources to build with zkMIPS?**

Our freshly launched Prover Service is designed to address this exact concern by providing access to powerful servers and tools necessary for generating validity proofs. This infrastructure allows developers and projects to utilize zkMIPS without the need for high-cost, resource-intensive hardware setups. You can find more details about our proving service here: zkm.io/blog/zkms-proving-service-breaking-down-the-barriers-for-proof-generation

**Where can I read the zkMIPS whitepaper?**

Please refer to the following link for the full ‘zkMIPS: a high-level specification’ whitepaper: [https://whitepaper.zkm.io/new\_zkMIPS\_white\_paper.pdf](https://whitepaper.zkm.io/new\_zkMIPS\_white\_paper.pdf)

\
