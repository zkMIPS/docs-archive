# zkVM vs zkEVM

In the zkEVM framework, the EVM is treated as a computational unit. zkEVM executes the EVM and produces zero-knowledge proofs to validate their execution. The private transactions and the current state of the Ethereum blockchain are the inputs to the EVM computation. Consequently, zkEVM produces both the result of the computation through the EVM and a zero-knowledge proof confirming the validity of the execution.

In contrast, the Zero-Knowledge Virtual Machine (zkVM) is a versatile virtual machine engineered to facilitate zero-knowledge proofs across diverse computational tasks. zkVM executes computations and creates a ZKP to prove the validity of their execution and the generated result.&#x20;

Unlike zkEVM, zkVM seamlessly integrates with various computations. Developers can harness zkVM's framework to create and execute zero-knowledge applications, irrespective of the underlying blockchain network. This adaptability broadens its utility beyond financial and privacy-oriented use cases. zkVM finds application in various domains, such as supply chain management, healthcare, and the entertainment industry, where preserving data integrity is imperative.
