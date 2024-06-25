---
description: Proofs of Knowledge in Verifiable Computation
---

# zkMIPS: Step by Step

<figure><img src="../.gitbook/assets/zkMIPS-Diagram.webp" alt=""><figcaption><p>Overview of the zkMIPS architecture for proving the execution of a program</p></figcaption></figure>

[Proofs of knowledge](https://docs.zkm.io/getting-started/background-behind-zkvms) in verifiable computation involve a Prover that performs a computation and produces a proof to show this computation was correctly executed, both of which are sent to the Verifier.&#x20;

The proof should be short or “succinct” so that the Verifier has minimal work to do. This is however at the cost of putting the burden on the Prover.&#x20;

Proofs of knowledge within blockchains are 1) succinct 2) non-interactive (between the Prover and Verifier) 3) ZK (this is optional). Common computational problems that are usually proven are \[3]:&#x20;

1. Transition functions for L2s – as seen in zkRollups.&#x20;
2. The execution of smart contracts – as seen in zkEVMs. Smart contracts are typically written in Solidity and the EVM verifies the execution of the contract.&#x20;
3. The execution of off-chain programs – as seen in zkVMs. In ZKM’s case, an off-chain MIPS program is given and zkMIPS verifies the execution of that program on-chain.&#x20;

### **zkMIPS in Verifiable Computation**

zkMIPS verifies the correctness of the execution of a MIPS program in the form of a proof. This is equivalent to the sequence of overall CPU states that represent a program’s execution.&#x20;

In zkMIPS, the computational problem that we want to verify is the program. The solution comes in the form of an [execution trace](https://docs.zkm.io/getting-started/background-behind-zkvms).&#x20;

An execution trace is a complete record of the computation executed from running the program. The execution trace is commonly represented through a trace record, where each column is a list representing the states of a fixed CPU variable and the rows showing each step of the computation \[1].&#x20;

The program is verified by checking whether each row of the execution trace matches the respective instruction of a program.&#x20;

A solution to this is to simply rerun the program. The problem is that this is not succinct. Similar to what we’ve discussed with the [trivial proofs ](https://docs.zkm.io/getting-started/background-behind-zkvms)in proofs of knowledge, the state sequences would be too large for the smart contract to verify \[2].&#x20;

## Techniques For Succinct zkMIPS Proving&#x20;

The following sections describe techniques that zkMIPS uses for improving the succinctness during the proof generation process. This includes arithmetization, FRI, method continuation, lookups and polynomial commitments.&#x20;

More details can be found in [“Getting to Know zkMIPS Proving Architecture”](https://www.zkm.io/blog/getting-to-know-zkmips-proving-architecture) written by ZKM Researcher Lucas Fraga and the [zkMIPS whitepaper](https://whitepaper.zkm.io/new\_zkMIPS\_white\_paper.pdf) written by the ZKM research team.&#x20;

**How can we complete the verification of a MIPS program more efficiently?**&#x20;

With [succinct proofs](https://docs.zkm.io/getting-started/background-behind-zkvms), we can verify a program in a time that is polynomial on the logarithm of the size of the program. For example, a program of size n can be verified in around a time that is polynomial on log(n).&#x20;

To enable greater succinctness, zkMIPS uses 2 main methods:&#x20;

1. Compressing the number of verification steps&#x20;
2. Moving the verification complexity from the Verifier to the Prover (hence there is a sophisticated Prover and a simple Verifier).&#x20;

### **Arithmetization**&#x20;

In moving the verification complexity to the Prover, we can use polynomials. Using arithmetization, we can represent the entire trace record as a single polynomial. &#x20;

The constraints (verification steps) and witnesses (the values in each step) can be encoded as polynomials. The constraint and witness polynomials can be combined together to form a single composition polynomial.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdpbUq9En8x5zIKFO_Mzh3JSZCMVIi7SLorD_nTQ52-RWZp7PWaHh1KlddQmyuQ9cyfCLqR7O8qW_u-Cug6CKX5SuLtBUXZAjmnRa1IOwB7VB2m69xXsLoCWC9jQnXAIqD1MEZDbogMXmDMlsN6Cii3unKW?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>From “<a href="https://www.youtube.com/watch?v=wru-kVPLkEs">Implementing a Modular zkVM Using Lookups</a>”, presented by ZKM research</p></figcaption></figure>

In the above diagram, the MIPS instructions and registers are encoded as polynomials. Constraints are applied in each row of the trace table. Each row is then converted into a constraint polynomial.&#x20;

We can now:&#x20;

1. Combine the different steps of the program and compress them&#x20;
2. Combine the constraint and witness polynomials into a single function where the result should still be a polynomial if the constraint and witness polynomials match.&#x20;

The program is verified through polynomial evaluations on the composition polynomial. What specific evaluations should be performed on the composition polynomial? &#x20;

### FRI Protocol&#x20;

The **FRI** protocol is one such method for determining this, specifically used to verify whether the composition polynomial has a proper degree.

The way the composition polynomial is defined guarantees that, if it is indeed a polynomial, the witness and constraints must match, and the program and its execution trace as a result are compatible. This is sufficient enough to verify the program.&#x20;

There are, however, problems to creating a composition polynomial for the entire program:&#x20;

1. The polynomial input to FRI can be as large as the MIPS program&#x20;
2. FRI proofs are heavily based on hash functions, which are expensive to compute on-chain&#x20;

Using FRI alone is not succinct enough for on-chain verification.&#x20;

### **Lookup Protocols**

A lookup protocol shows that a target table is contained in a reference table, i.e., that the segment is contained in the modular tables.&#x20;

The lookup protocol, if successful, shows that every entry in the original set of internal CPU states appears in the table of one of the operation groups. The lookup proof will be equivalent to the FRI-based continuation proof.&#x20;

Lookups are used because:&#x20;

1. They are another technique to improve succinct proving by decreasing the proof size
2. Some operations (e.g., logic operations) cannot easily be expressed as polynomials&#x20;

Different tables are created for different types of operations and table lookups are used to show the original segment table corresponds to the smaller operation tables proved with FRI. Different programs will also produce different tables. The entries of the tables and lookups into the tables are encoded as polynomials.&#x20;

In zkMIPS, the segment polynomials are broken down into four main modular tables: arithmetic, logic, memory and control. FRI is used to prove each operation group in parallel and joins them together in a lookup protocol.&#x20;

There are two types of tables used:&#x20;

* **Target table** – a table representing the entire segment&#x20;
* **Reference table** – a table combining all four modular tables&#x20;

### **Continuation**&#x20;

To counter the computational expense of creating a single composition polynomial, method continuation can be used by breaking down the witness and constraint polynomials into pieces.&#x20;

This process includes:&#x20;

1. Dividing the program into smaller segments &#x20;
2. Proving each of the segments in parallel&#x20;
3. Running the FRI protocol for each segment&#x20;
4. Combining all segments proofs to get a program proof

We will end up with one FRI proof for each segment. The continuation protocol will show that the FRI proofs continue onto one another, which is done recursively until there is one single continuation proof. The final proof will be around the size of one segment proof and is sufficient enough to prove the correctness of the entire program.&#x20;

Because the resulting continuation proof is FRI-based, it is not adequate for on-chain computation. We can further improve the succinctness by using lookups.&#x20;

### **Polynomial Commitments**

Polynomial commitments are used to further decrease the overall final proof size and proving time by adjusting the FRI-produced proofs.&#x20;

Using polynomial commitments, larger proofs can be produced faster and smaller proofs can be produced slower. The last continuation proof uses slow and small commitments (to decrease the final proof size) before being converted into Groth16. The other FRI-based phases use fast and large commitments.&#x20;

Another note about polynomial commitments: zkMIPS uses Merkle trees to commit to all possible evaluations of certain polynomials in a given domain. The Verifier can use the Merkle root to query the polynomial evaluation from the Prover \[1] and check certain polynomial properties using the queried values.&#x20;

## zkMIPS Step-by-Step Process

### **Proof Generation & Verification with zkMIPS – Step-By-Step Overview**&#x20;

zkMIPS is ZKM’s general-purpose zkVM verifying on-chain that a MIPS program was correctly executed off-chain. zkMIPS is based on MIPS32 microarchitecture.&#x20;

zkMIPS is composed of: a sophisticated prover, a simple verifier and an equivalent verifier smart contract \[2].

Components:&#x20;

* MIPS Compiler&#x20;
* ELF Loader&#x20;
* MIPS VM&#x20;
* Prover&#x20;
* Verifier Contract&#x20;

Steps I-III describe program execution in zkMIPS.&#x20;

Steps IV 1-6 describe the proof generation in zkMIPS.&#x20;

Step V describes the proof verification in zkMIPS.&#x20;

1. MIPS Compiler&#x20;

A program written in Go or Rust is compiled into a MIPS ELF binary file using the MIPS Compiler. The ELF file is the executable format for the MIPS instruction set.&#x20;

2. ELF Loader&#x20;

The MIPS ELF binary file is loaded into the MIPS VM using the ELF Loader.&#x20;

3. MIPS VM&#x20;

The MIPS VM runs the program and generates an execution trace for the Prover.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdQwunBvcZedqhkf5piYVeOfgFoq-XUV8anW6HQZAhf9qYLyTOsyT0JAakOx0fYT9ZjTPosh-FsySYp1P8rvhdm5wNKihf5Adqfs9f9Hr9mP1GLifID9bYFowhWcr6uzoAGC9bKVR-pHz08BWzXzPJs-gk?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>From “<a href="https://www.youtube.com/watch?v=wru-kVPLkEs">Implementing a Modular zkVM Using Lookups</a>”, presented by ZKM research</p></figcaption></figure>

Any high-level language can compile down to MIPS. We get a sequence of the MIPS instructions as a result.&#x20;

For example for row i, the described data operation is addition. In the instruction, the value in register 5 (13) is added to the value in register 6 (21). The result of the computation is stored in register 7 (34). For row i+1, the described data operation is addition (using an immediate operand). In the instruction, the value in register 5 (21) moves to register 6.&#x20;

After each instruction is executed (left side table), we will get a value for each register. After going through all the instructions, we will get the trace table (right side table).&#x20;

4. Prover&#x20;

The Prover executes the MIPS program and uses the execution trace to generate a proof.&#x20;

As part of the Prover, the following are steps in the proof generation process:&#x20;

1. The program and trace are divided into segments.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXc84O5BQ-U0NrCxxqx1-OL3765x6f4mCeJjMdzDcwbciDG8CBH-NHKdESFCqP_0kZQj4HsiDk4yegX7Mv29R9ZT1JGu3UISDWGUM4V9jyhXoJU1pSkPxi2N9hdG4Cfnlbb5_yFxfg-j7T2dTn7CHSofovpX?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>From “<a href="https://www.youtube.com/watch?v=wru-kVPLkEs">Implementing a Modular zkVM Using Lookups</a>”, presented by ZKM research</p></figcaption></figure>

After getting the trace table, the first step of continuation can be applied by splitting the table into segments. Segments are small sequential executions from the program execution.&#x20;

2. The segments are divided into modules. The instructions of each segment are divided into groups of four module tables: arithmetic, logic, memory and control.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXceYmjZ_ey1RggVckiFSL5G3V97Y1gWfZ5TlgFMTprNB25HOAozvuJdaTU0eWUqO4F8Uzn9vf2Ms0wxIuMDPg7dfPoq7Y2QAbnEbVGGN3XGeMTf5lK34ZhRoPPnVqn98H24HK2BiUcemwOZ5oV2XAhpsnYt?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>From “<a href="https://www.youtube.com/watch?v=wru-kVPLkEs">Implementing a Modular zkVM Using Lookups</a>”, presented by ZKM research</p></figcaption></figure>

Modules are smaller non-sequential executions from the segments.&#x20;

Each instruction, depending on the category it falls under, will be divided into its corresponding table. For example, instructions with the add operation (rows i - i+3) are placed in the arithmetic table.&#x20;

3.  STARK – The modules are proven independently and in parallel with FRI. These proofs are FRI-based modular proofs.&#x20;

    <figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXeMkdOQjWDzDQzq3hgcbsGdoyzfuUfsPYc0j-dddRl7GM1G8yOqio11w2pReM49t5iKrKDyFgvah181EaPRZMup1w5kj1LlBF5yYS_uhaCUsEdWKAYgoJvzQaliDoNdVUTzDUQVK2pHljBrNMLHW6_yHN4Q?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>From “<a href="https://www.youtube.com/watch?v=wru-kVPLkEs">Implementing a Modular zkVM Using Lookups</a>”, presented by ZKM research</p></figcaption></figure>

Each module execution that combines all segment instructions is independently proved using STARK proofs. In this layer, arithmetic, memory, logic and control proofs are generated. Traces proved in this layer are referred to as module traces.&#x20;

4. LogUp (STARK) – The modular proofs from the previous step are combined into one single proof for each segment using the LogUp lookup protocol. These segment proofs are LogUp proofs written using Starky.&#x20;

LogUp is the lookup scheme used by zkMIPS to prove that the program instructions were correctly verified in their own specific modules.&#x20;

Traces proved in this layer are referred to as segment traces. Segment proofs can be produced before module proofs, and as a result, this layer can run in parallel with the previously described layer \[1].&#x20;

5.  PLONK  – All segment proofs from the previous layer are recursively combined using the continuation protocol into one single continuation proof. This proves the correctness of the entire program trace. The continuation proofs are written using Plonky.&#x20;

    <figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXc_kHRtWFKUzmfoktJ0DrxwkecKrVpRwIVrF29HEmBpta85zN1rv2TeX7FwHSZ1QPqB--QbwjZXeaXTLF2UYI_3uH9g5HbR60FI9uXxiLwzAWFWJ_jnxHwhn__9KqigFY1Fb6pYBylYpkbkaQ72o8vH9SI5?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>From “<a href="https://www.youtube.com/watch?v=wru-kVPLkEs">Implementing a Modular zkVM Using Lookups</a>”, presented by ZKM research</p></figcaption></figure>
6. Groth16 – The Groth16 proving system is used to enable on-chain computation by converting the proof to an EVM-friendly format. The continuation proof is converted into an on-chain Groth16 (SNARK-based) proof.&#x20;

This enables the final proof to be verified on L1. Groth16 uses bilinear functions over elliptic curves that are natively implemented in EVM which also enables zkMIPS to verify the proof faster on-chain.&#x20;

The final proof is the zkMIPS proof representing the proof of the correct execution of the program.&#x20;

5. Verifier Contract&#x20;

After generating the zkMIPS proof, the verifier contract verifies the proof on-chain.&#x20;

zkMIPS implements a Plonky2 verifier in Gnark for the proof verification.&#x20;

## High Level Overview&#x20;

The following is a high-level overview of the interactions involved:&#x20;

1. **End User Action** – the end user wants to prove the correct execution of a program. Some examples include proving the correctness of blockchain transactions, validating new rollup states, verifying smart contract function calls, etc.&#x20;
2. **Program Execution** – the program runs in zkMIPS’ execution environment.&#x20;
3. **Proof Generation** – a proof for the program’s correct execution is generated by zkMIPS and can be outsourced to ZKM’s Prover Service.&#x20;
4. **Proof Verification** – once the zkMIPS proof is generated, a verifier contract will be run on a destination chain to verify the proof.&#x20;
5. **Settlement** – in the case of proving the correctness of blockchain transactions, once the proof is verified on-chain, the transaction is considered to be settled.&#x20;

### Example – Asset Transfer with Entangled Rollups

The L2s in the Universal L2 structure serve as interoperability mechanisms to facilitate cross-chain transfers without the implementation of a bridge.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdMiO-1_GhklBv2Fs0JJ6dahJXuGQMnk45E0V_5u4M-qyqX1yPl6RpdU0T7mrMY1zFucTIBSPXJZ52IH6wS6hFulQeoHQWbZN4_7ZgmgLXxpQRTzHRCQ3Os_UwoJFqNOyBF_yd58FwBh0DHQywotDBJulsi?key=mLtoVUSPd7DodgiPO6S3Ig" alt=""><figcaption><p>Diagram from “<a href="https://settlement.network/docs/native/entangled-rollup/universal-l2">Universal L2 Process</a>” in <a href="https://settlement.network">settlement.network</a></p></figcaption></figure>

In this example, 100 USDT is sent from the source chain (ETH L1) to the destination chain (AVA L1). A proof that the 100 USDT was burned on the L2 (ETH UL2)  is generated off-chain. The proof is verified by a verification contract on the destination chain. Once verified, the user will receive 100 USDT on AVA L1.&#x20;

1. &#x20;[zkMIPS whitepaper](https://whitepaper.zkm.io/new\_zkMIPS\_white\_paper.pdf)
2. [“Getting to Know zkMIPS Proving Architecture”](https://www.zkm.io/blog/getting-to-know-zkmips-proving-architecture)
3. “[Implementing a Modular zkVM Using Lookups](https://www.youtube.com/watch?v=wru-kVPLkEs)
