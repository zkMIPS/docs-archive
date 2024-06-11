# Background Behind zkVMs

## Proof of Knowledge

A proof of knowledge is a two-party protocol allowing a Prover to convince a Verifier that it knows a valid solution. The Prover is typically a more privileged party in the sense that it is computationally more powerful or possesses some secret information.&#x20;

This is present in all computational problems. In this setting, the Prover knows the solution to the target problem. The Verifier does not know the solution. For example, the Prover can say “I know the prime factorization of a given number”. The Prover will provide a proof for the Verifier to check its validity.&#x20;

Note that this specific statement is put into a computational model to show that the Prover knows something, not necessarily that the statement is true.&#x20;

Cryptographic proofs have the following properties: Completeness, Soundness, Succinctness

## Completeness&#x20;

If the Prover correctly follows the protocol (both parties are honest), the proof execution is successful and gives the correct result.&#x20;

## Soundness&#x20;

If the Prover incorrectly follows the protocol (the Prover is cheating), the Verifier will accept with negligible property and the proof execution fails.&#x20;

## Succinctness&#x20;

As size N increases, the Prover and Verifier times increase polynomially on log(N). Proofs that possess this property are succinct proofs.&#x20;

## Zero-Knowledge&#x20;

The protocol does not leak useful information about the witness possessed by the Prover to the Verifier. Proofs that possess this property are zero-knowledge (ZK) proofs.&#x20;

## Verifiable Computation&#x20;

Verifiable computation enables the outsourcing of a computation to the computationally more powerful party. The Prover evaluates a function and returns the result with a proof that its computation was correctly executed to the Verifier.&#x20;

What would convince the Verifier is a dump of the entire execution of the program describing the function. This is inefficient as it is essentially asking the Verifier to replicate the computation done by the Prover.

## Trivial Proof&#x20;

For every problem, a trivial proof exists. This is the answer or solution itself. We cannot always use the trivial proof for two main reasons: 1) the Prover may want to keep the solution a secret 2) the resulting proof can be extremely long for the Verifier to check (e.g., with the entire execution dump).&#x20;

We are now presented with two extensions of the proofs of knowledge to address the two challenges within trivial proofs: 1) zero-knowledge proofs 2) succinct proofs.&#x20;

## Succinct Proof

A succinct proof is a type of proof of knowledge that can be verified much faster than the equivalent trivial proof in a way that optimizes both the Prover and Verifier.&#x20;

Our goal is to make the proof short by having the Verifier do little work with the help of the Prover running the complicated parts of the program. To do this, the work of the Verifier is simplified at the cost of putting the burden on the Prover. In shifting the verification complexity from the Verifier to the Prover, we have a sophisticated Prover and a simple Verifier.&#x20;

The reason why this is done is because in some scenarios, the Prover is more powerful than the Verifier (e.g., the Prover can be a service provider and the Verifier can be a smartphone). The Verifier may not have the computational capacity to process and verify the proof. In succinct proofs, the Verifier does minimal work to verify the solution: flip random coins, perform checks on the succinct proof that was sent, and accept or reject the proof.&#x20;

In verifiable computation, the role of the Prover is to compute the result and to generate a proof showing that the value of the result is correct. The result and proof are sent to the Verifier.&#x20;

## Zero-Knowledge Proof&#x20;

A zero-knowledge (ZK) proof is a type of proof of knowledge using ZK. ZK proofs guarantee 3 main properties: 1) completeness 2) soundness 3) ZK – the proof does not reveal any information about the solution for a given problem.&#x20;

In blockchains, proofs of knowledge are 1) succinct 2) non-interactive (between the Prover and Verifier) 3) ZK (this is an optional property). Some examples for the types of problems that are usually proven include transition functions for Layer 2s as seen in ZK rollups and the execution of smart contracts as seen in zkEVMs. ZKM is specifically focused on the execution of off-chain programs with zkVMs.&#x20;

## Zero-Knowledge Virtual Machines (zkVMs)&#x20;

ZKM uses a zkVM based on MIPS for verifiable computation. A zkVM enables the Verifier to outsource computation to a computationally more powerful Prover. The Prover computes the proof that the result was correctly computed to the Verifier.&#x20;

zkVMs verify the correct execution of a program. Note that the “zk” in zkVMs often mean succinctness instead of privacy. In zkVMs, the computational problem that we want to verify is the program. The solution comes in the form of an execution trace. The zkVM will execute the program and create the execution trace.&#x20;

## Execution Trace

An execution trace is a snapshot record of the entire sequence of operations that is executed by a running program.&#x20;

Execution traces are typically written as a table to represent the valid computation. A valid computation is defined as a sequence of states from an initial state to a final state. This table is the trace record, with its columns showing a list of the CPU variables defining the overall state and the rows showing each step of the computation.&#x20;

Verifying the state transitions enables us to verify the correctness of the computation. In other words, the program is verified by checking whether each row of the execution trace matches the respective instruction of a program.&#x20;

The inputs of a zkVM consist of the initial state (input commitment), the program, and witness data. The witness is the set of all values that are needed to verify a program, taking the form of a table where the rows represent the program’s instructions and the columns represent the registers. The outputs consist of the final state (output commitment) and a proof of the execution.&#x20;

The VM generates an execution trace for the Prover. The Prover will then be able to generate ZK proofs based on the execution trace of the program.&#x20;

## List of Glossary Terms&#x20;

* Proof of Knowledge&#x20;
* Completeness&#x20;
* Soundness&#x20;
* Succinctness&#x20;
* Zero-knowledge&#x20;
* Verifiable computation
* Trivial proof&#x20;
* Succinct proof&#x20;
* Zero-knowledge proof&#x20;
* zkVM
* Execution trace&#x20;
* Trace record&#x20;
* State transition&#x20;
* Witness&#x20;
* Interactive Oracle Proof (next section)&#x20;
* Arithmetization (next section)&#x20;
