# zkVM vs zkEVM

In the zkEVM framework, the EVM is treated as a computational unit, executing tasks and producing zero-knowledge proofs to validate their execution. Private transactions and the current state of the Ethereum blockchain serve as inputs to the EVM computation. Consequently, zkEVM produces both the result of the computation through the EVM and zero-knowledge proofs confirming the validity of the execution.

In contrast, the Zero-Knowledge Virtual Machine (zkVM) is a versatile virtual machine engineered to facilitate zero-knowledge proofs across diverse computational tasks. zkVM executes computations and creates ZKPs to prove the validity of their execution and the generated results.

Unlike zkEVM, zkVM seamlessly integrates with various computational tasks. Developers can harness zkVM's framework to create and execute zero-knowledge applications, irrespective of the underlying blockchain network. This adaptability broadens its utility beyond financial and privacy-oriented use cases. zkVM finds applications in various domains, such as supply chain management, healthcare, and the entertainment industry, where preserving data integrity is imperative.
