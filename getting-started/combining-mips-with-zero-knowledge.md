---
description: A High-Level Overview
---

# Combining MIPS with Zero-Knowledge

### What is MIPS?

MIPS stands for Microprocessor without Interlocked Pipeline Stages. It’s a minimalistic architecture for microprocessors that aims for simplicity. Any code written for that architecture gets compiled into a small set of different instructions, making it ideal for further refinement.

### What is ZK-MIPS

ZK-MIPS means adding ZK to MIPS. Because the MIPS architecture is so minimal, converting code written in MIPS into ZK-MIPS is relatively straightforward. Easy conversion results in fewer bugs and issues.

The converted code is then placed inside a Virtual Machine (VM). This is a specialized VM that utilizes the ZK architecture, making it a ZK-VM. Because we also utilize the MIPS architecture, this ZK-VM is called ZK-MIPS.

The point of ZK is to verify certain computations or information without revealing the underlying data. This is essentially what also happens when we add ZK to MIPS: we can prove that the computation was performed correctly inside the VM, while potentially hiding the inputs.

In reality, this happens by transforming all of the computation into a different format that is suitable for processing with ZK. Every line of code you write needs to be transpiled into something else before it can be processed by ZK-MIPS. The underlying formats are basically mathematical polynomials and various types of commitments.

### What are some potential uses of ZK-MIPS?

1\. Privacy. Hide information, but prove that you have it.\
2\. Succinctness. Create a proof for a large amount of data. The proof is a lot smaller than the original data.\
3\. Verifiable computation.

The first use of ZK-MIPS is verifiable computation, but it can potentially support all of the mentioned use cases.

### About Verifiable Computation

The basic idea is quite straightforward: perform regular computation, but on top of that, generate a ZK proof which proves that the computation was performed correctly.

Compared to using a regular Virtual Machine (typically a cloud server provider), the main benefit is removing the trust component. When running anything in a regular VM, you trust that:

1\. The VM is configured correctly.\
2\. The program you run does what you think it does.\
3\. The results given to you are correct.

All of these assumptions are eliminated with a ZK-VM because a ZK proof can’t be generated for an invalid computation.

You can write most types of programs inside a ZK-VM. There are certain limitations due to determinism: all of the used data needs to be directly input into the program. Problems present in more traditional non-VM ZK environments, such as restrictions on looping, recursion, branching, and execution depth, are not an issue in a ZK-VM.

### Uses of Verifiable Computation

Many scenarios where multiple parties rely on some execution can benefit from verifiable computation.

Some examples are:

1. Cloud computation. You can outsource heavy computation to a cloud provider and verify the result easily with the accompanying ZK proof.
2. Blockchains. In decentralized blockchains, the participants don’t typically (need to) trust each other. If a computation is performed in a ZK-VM it can be easily verified by anyone without trust assumptions. For example, verifying that a transaction is correct and has followed all of the consensus rules.
3. Machine learning. Verifiable computation can help in making sure that the used models provide valid results.

### About Verification

### The ingredients for verification are:

1. The generated computation proof
2. The actual MIPS code used for the execution
3. Possible public inputs for the execution
4. A verifier program

The verifier program can be somewhere on-chain or in another trusted location. A single verifier program can be used to verify any ZK-VM execution. Once you have the needed data, you give it to the verifier program. The program then tells you whether the execution was performed correctly or not. A negative answer from the verifier means something went wrong.&#x20;

### Turning MIPS into ZK-MIPS

If you already have a MIPS program, it can be converted into a verifiable ZK-MIPS program. The written code is first compiled into low-level MIPS instructions. Since MIPS only has a small set of different instructions, it is relatively easy to convert these instructions into a format suitable for ZK proof generation.

![](https://lh7-us.googleusercontent.com/I7La0v9\_JSMjaZlBFy3wJsVDZCwsV1q5V81jc4CtR0Y5hqqKo\_c8SS-fqglnRAr8\_CGLL0P-W2D7kcIR-bqVCSx6VGgZ0rFKKsor6nWw2PqpZ5mY8c5JHJH-ZhrmQsI801aQOprRFpMNsDEjH3VZ6Vg)

_The flow of turning MIPS into ZK proofs_

The instructions are then turned into mathematical polynomials. If your program is, for example, calculating x = 5 + 3, it would be turned into a polynomial P(x) = x — 8. This polynomial evaluates to zero when x = 8. The next step is creating a proof that you know x so that the polynomial evaluates to zero. This way, the actual input value is not revealed, but it can be proven that you know a valid input value.

A similar process is performed for all of the MIPS instructions in your program. The eventual proof attests that:

1. You know the right inputs.
2. The computation is performed correctly; all of the polynomials are evaluated as intended.

Note that this description is very simplified.

### Present drawbacks of a ZK-VM

The main disadvantages of a ZK-VM are:

1. The required technological stack is complicated. This can result in bugs.
2. Since a ZK proof is generated for any computation, the total computation time is longer than without ZK.
3. ZK-VMs are nascent, so a lot of research, education, tooling, and innovation is still required to make them completely practical.

There is a lot of work going into ZK-VM’s and they will inevitably mature to become completely practical and bug free.

\
