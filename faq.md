# 💡 FAQ

## About ZKM

### What challenges does ZKM aim to address?

The project aims to address several challenges, including the elimination of the 7-day Withdrawal Window, achieving Unified Security for all Layer 2 (L2) solutions and alternative Layer 1 (Alt L1) blockchain networks, as well as enabling tamper-proof Internet of Things (IoT) and cloud computing. By tackling these challenges, the project seeks to provide faster and more convenient fund withdrawals, establish a standardized security framework across diverse L2 and Alt L1 platforms, and enhance the security and integrity of IoT devices and cloud computing systems.

### What are the main features of ZKM?

The main features of ZKM (Zero-Knowledge Proof MIPS) include:

* One ZKP for All: By securing the CPU/MIPS beneath every VM, framework, or app above, all software above the CPU is secured. This allows everyone to claim Zero-Knowledge Proof (ZKP) security without needing to perform ZK proofing themselves.
* Work with All VMs: ZKM sits beneath every Virtual Machine (VM), enabling compatibility with various blockchain smart contract engine VMs, such as MoveVM (zkMVM), WASM (zkWASM), and RustVM (zkRVM).
* Plug and Play Adoption: Developers can adopt ZKM without changing or adapting their existing codebase for ZKP. It offers zero- or low-cost adoption and technology-agnostic adoption, allowing the use of different smart contract languages and even traditional programming languages.
* Long-Term Stability: Leveraging the stability of the MIPS instruction set, ZKM eliminates concerns about constantly changing EVM instruction sets, providing a more stable environment for development.
* Free Security for All: ZKM leverages the large, decentralized security base of Ethereum to validate all transactions, ensuring security for both blockchain and non-blockchain applications.
* Better L2 Rollup User Experience: With ZKM-enabled Hybrid Rollup, users can enjoy an improved experience. Former Optimistic Rollup users benefit from instant confirmation and finality without waiting for a 7-day withdrawal period from Layer 2 to Layer 1. Depository users experience doubled security through ZKP verification and a fraud-proof challenge window that can be extended well beyond the standard 7-day window for safe transfers of large amounts.
* Going Far Beyond L2s, and Beyond Blockchain: ZKM expands the scope of Optimistic Rollups' potential market. It extends its reach to alternative Layer 1 blockchains (such as BNB, Celo, etc.), providing the benefits of ZKM security. Additionally, ZKM enables tamper-proof Internet of Things (IoT) devices and enhances security in cloud computing environments.

## About Hybrid Rollups

### What is a Hybrid Rollup?

Hybrid Rollup is a Rollup protocol that combines two existing Rollup solutions, Optimistic Rollup and ZK Rollup, into a single cohesive protocol.

### What are Optimistic and ZK Rollups?

Optimistic Rollup and ZK Rollup are two distinct Layer 2 scaling solutions for blockchain networks. Optimistic Rollup relies on optimistic execution and fraud proofs, while ZK Rollup utilizes zero-knowledge proofs for scalability.

### What advantages does Hybrid Rollup offer?

Hybrid Rollup aims to give users the option to withdraw with a less expensive but longer 7-day withdrawal time of a Optimistic Rollup or a shorter, but more expensive withdrawal option of a ZK Rollup.

## About MIPS

### What is MIPS?

MIPS is a computer architecture that stands for Microprocessor without Interlocked Pipeline Stages. It is a reduced instruction set computing (RISC) architecture developed by MIPS Technologies.

### What does the term "Interlocked Pipeline Stages" mean in MIPS?

"Interlocked Pipeline Stages" refers to a feature in some computer architectures where pipeline stages can be stalled or delayed due to dependencies between instructions. MIPS, on the other hand, does not have interlocked pipeline stages, which allows for simpler and more efficient pipeline execution.

### What are the advantages of using MIPS?

Some advantages of MIPS include a simplified instruction set, high performance, low power consumption, and good code density. It is often used in embedded systems, digital signal processing, and other applications that require efficient execution of instructions.

### Which companies have used MIPS in their products?

MIPS has been used by various companies, including Sony, Toshiba, Cisco Systems, and many others. It has been employed in gaming consoles, networking equipment, routers, and other devices.

### What are some common instructions in MIPS?

Common MIPS instructions include arithmetic operations (add, subtract, multiply, divide), logical operations (and, or, not), memory access (load, store), control flow (branch, jump), and data movement (move, copy).

### Can I run MIPS programs on a personal computer?

Yes, you can run MIPS programs on a personal computer using an emulator or simulator that emulates the MIPS architecture. There are various MIPS simulators available that allow you to execute and test your MIPS programs on a PC. One example of a MIPS emulator is the "MARS" (MIPS Assembler and Runtime Simulator) emulator. MARS is a Java-based emulator that provides a complete development environment for MIPS assembly language programming. It offers a graphical user interface (GUI) and supports a range of MIPS instruction sets.

### Is MIPS an open-source architecture?

Yes, MIPS is now available as an open-source architecture. In 2018, MIPS Technologies was acquired by Wave Computing, which subsequently released the MIPS instruction set architecture (ISA) as open source.

### Can I run operating systems like Linux on MIPS-based devices?

Yes, MIPS-based devices can run operating systems like Linux. Linux has been ported to MIPS architecture, and there are distributions available that support MIPS-based systems.

### Is MIPS a 32-bit or 64-bit architecture?

MIPS can support both 32-bit and 64-bit architectures. The original MIPS architecture was 32-bit, but later versions and extensions introduced support for 64-bit processing.

### Can I develop software for MIPS architecture using popular programming languages like C or C++?

Yes, you can develop software for MIPS architecture using popular programming languages like C or C++. MIPS compilers are available that can translate high-level code written in languages like C or C++ into MIPS assembly language or machine code.

### Does MIPS support multi-core processors?

Yes, MIPS architecture supports multi-core processors. Multi-core MIPS processors have been developed, allowing for parallel execution of instructions and improved performance in multi-threaded applications.

### Is MIPS suitable for real-time systems?

Yes, MIPS is often used in real-time systems. Its simplified instruction set and deterministic execution make it well-suited for applications that require real-time responsiveness, such as automotive systems, industrial control, and medical devices.

### Can I use MIPS to develop software for embedded systems?

Yes, MIPS is commonly used for developing software for embedded systems. Its low power consumption, compact code size, and efficient execution make it well-suited for various embedded applications, including consumer electronics, automotive systems, and industrial devices. This was one of our motives when designing architecture for ZKM.

### What is the difference between MIPS and ARM architectures?

MIPS and ARM are both RISC architectures but have some architectural differences. MIPS has a fixed instruction size of 32 bits, while ARM supports variable instruction sizes (16-bit and 32-bit). MIPS uses delayed branching, whereas ARM uses conditional execution. Additionally, the instruction sets and registers of MIPS and ARM architectures differ.

### Can I use MIPS to develop software for embedded systems?

Yes, MIPS is commonly used for developing software for embedded systems. Its low power consumption, compact code size, and efficient execution make it well-suited for various embedded applications, including consumer electronics, automotive systems, and industrial devices.

## About Zero Knowledge Proofs

### What is a zero-knowledge proof (ZKP)?

A zero-knowledge proof is a cryptographic protocol that allows one party (the prover) to prove to another party (the verifier) the validity of a statement without revealing any additional information beyond the truth of the statement itself.

### What is the purpose of zk proofs?

Zero-knowledge proofs are used to demonstrate the truthfulness of a claim or statement without disclosing any sensitive or confidential information. They enable secure and private interactions where trust and verification are required.

### How does a zk proof work?

In a zero-knowledge proof, the prover convinces the verifier of the truth of a statement without revealing any underlying information. This is accomplished by constructing an interactive protocol that demonstrates knowledge of information that could only be known if the statement were true.

### Are zk proofs secure?

Zero-knowledge proofs are designed to be secure and provide strong cryptographic guarantees. However, the security of zero-knowledge proofs relies on the underlying cryptographic assumptions and the proper implementation of the protocols.

### Can zk proofs be verified without knowing the underlying secret?

Yes, zero-knowledge proofs can be verified by the verifier without knowing the underlying secret or information. The verifier can validate the proof's integrity and correctness based on the interactions and responses received from the prover.

### What is a zk-STARK?

zk-STARK (Zero-Knowledge Scalable Transparent Argument of Knowledge**)** is a cryptographic proof system that enables the creation of zero-knowledge proofs for demonstrating knowledge of certain statements without revealing any sensitive information. It stands for Zero-Knowledge Scalable Transparent Arguments of Knowledge.

### How does STARK differ from other zero-knowledge proof systems?

STARKs are different from other zero-knowledge proof systems like zkSNARKs (Zero-Knowledge Succinct Non-Interactive Arguments of Knowledge) in terms of transparency and scalability. STARKs offer transparent setups, which means they do not require a trusted setup, and they are designed to be scalable for larger computations.

### What are the advantages of STARKs?

STARKs offer several advantages, including transparency, which eliminates the need for a trusted setup ceremony. They also provide scalability, allowing for efficient verification of complex computations without sacrificing security or privacy.

### What are STARKs used for?

STARKs have various applications, including privacy-preserving transactions, secure multiparty computation, verifiable computation, and proving integrity of large datasets. They can be used in blockchain technologies, ensuring privacy and security while verifying the validity of transactions or computations.

### How does the verification process work with zkSTARKs?

In STARKs, the verifier can efficiently and publicly verify the correctness of the prover's claim without knowledge of any underlying information. The prover constructs a STARK proof that convinces the verifier that the statement is true without revealing the secret inputs.

### Are STARKs post-quantum secure?

STARKs are believed to be post-quantum secure, which means they are resistant to attacks by powerful quantum computers. Their security relies on mathematical problems that are considered hard to solve, even with quantum algorithms.

### Are STARKs computationally efficient?

STARKs are generally more computationally intensive compared to some other zero-knowledge proof systems. The proof generation and verification processes involve complex computations. However, ongoing research aims to optimize STARKs for improved efficiency.

### What is Arithmetic Intermediate Representation (AIR) in the context of zero-knowledge proofs?

Arithmetic Intermediate Representation (AIR) is an intermediate language or format used to express arithmetic computations and constraints in a standardized manner within zero-knowledge proof systems.

### How does AIR facilitate the generation of zero-knowledge proofs?

AIR provides a structured and standardized representation for arithmetic computations, making it easier to optimize and transform them into more efficient forms for generating zero-knowledge proofs.

### What are the benefits of using AIR in zero-knowledge proofs?

Using AIR allows for the separation of the computation from the proof generation process, enhancing the analysis, optimization, and validation of zero-knowledge proofs. AIR also enables the efficient expression of arithmetic constraints in a higher-level format.
