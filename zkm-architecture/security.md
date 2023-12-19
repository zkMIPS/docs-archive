# Security

Ethereum is a very complex technology and Zero-Knowledge technologies are perhaps even more so. These involve several layers of abstraction, with explanations often consisting of vague descriptions and terminology that implicitly assume certain prior knowledge the reader may not have.

Fuzzy and vague descriptions are the enemy of any rigorous security analysis, so when analyzing the security of a system, we should always ask two questions:

* Question 1: What, exactly, is the setting? Which are the parties? Which components of the system are trusted? What is the exact data?
* Question 2: What, exactly, are the security properties we want to maintain? Is it confidentiality? Integrity? Correctness? non-repudiation? Other properties.

Below we address the first question; the second question will be dealt with in Part 2.

## Question 1: About the setting

In the context of our [white paper](https://zkm.io/whitepaper), Zero Knowledge actually deals with a scenario of Verifiable Computation between two parties, called V and P. Here, V has few computational resources (the case of a simple wearable device, a cell phone, or Ethereum Layer 1) while P is very strong (a cluster of computers, the cloud). In this setting, it is natural that V wishes to outsource a computation; so say that V is asking P to  compute the result y of running program F on input x, i.e. y=F(x). However, V doesn’t trust P since it can send a wrong result y', while claiming it is correct, cash the money for the work done, and disappear.

<figure><img src="https://lh7-us.googleusercontent.com/ODFvKate3xv32MzFJBWnq51TvCitJagEEO82QmK9omRZIRp7M67uGeqZ8TDhHf7CCg0otBdbv6mR3In7qfdnGa6AdKUxoraTjzm-yEl0q45r0jnaaqY-sWFCoXEXhOxDlwvZ5SO6GQC0HGOMPaEu4h8VJ0YlnWS2PeXF8cPB0wlo8qnUm7MUoAW0aUY6tA" alt=""><figcaption><p>V wants to outsource the computation of program F with input x</p></figcaption></figure>

To avoid this scenario, V and P agree adding a verification to the outsourced computation. In addition to the result y, party P is also going to provide a proof, Z, that y is indeed the result of running program C on x. Moreover, P will be doing lots of extra computations to make sure that 1) this proof Z is short, and 2) that verification of the correctness of Z is a very efficient algorithm, meaning that even a V with few resources can easily perform this check.

There exists various techniques for realizing this scenario of Verifiable Computation, starting historically with probabilistically checkable proofs (PCP) and, more recently, using SNARKs and STARKs. All these techniques have in common that they transform a computation into a very high degree and complex polynomial, and that V only needs to perform simple verifications on this polynomial to be convinced that P performed correctly. How is this possible?  We will be exploring that in a future blog post - stay tuned. ,. .

So the overall setting is that the prover P wants to prove to the verifier V that it performed the computation correctly, i.e. that y=F(x). Observe that F, x, y are the inputs to the protocol and are known to both P and V. Please make sure not to confuse the input to the program F, namely x, with the inputs to the protocol, namely C, x and  y.

So if all these values are public, you may ask: what, then, is P's secret? Or, using ZK terminology, what is the witness, i.e. the data that the prover owns which allows him to prove the correctness of his claim? In the context of Verifiable Computation this witness is data proving that V actually performed the whole computation of F on input x resulting in y, usually called the execution trace T.

<figure><img src="https://lh7-us.googleusercontent.com/8kV3H4nAvJV4zwJIwTtcV6DQ4HGwCoY3twfKPSrr9rwsRMJfjYVpFdeMom9IMNkAqmQqJaX9EFStj6ivlYf9BTmSVj4-7LeEya_x4KAFiQP0OjDmw_tbGrOYHs7jHuIEeflA_HjJJXcgnriS8nz9XJdqo4BmEs1t2yEcdU6XnwgjNqrnLOmuQVMcHp5CPw" alt=""><figcaption><p>P proves to V that it computed F(x) correctly by showing it knows a corresponding execution trace which leads to the answer y</p></figcaption></figure>

So what is an execution trace? A ZKM computation happens in steps, and its overall state can be defined by the values of a finite list of variables. A valid computation is a sequence of states, from a well-defined initial state to some final state, in which each state transition represents a valid computation step. It can be represented as a table whose columns represent the list of variables and whose rows represent each step of the computation. This table is known as the execution trace; the following diagram shows a toy example.\


| opcode       | state | R0 | R1 | R2 | R3 | R4 |
| ------------ | ----- | -- | -- | -- | -- | -- |
| <p><br></p>  | 0     | 20 | 25 | 45 | 0  | 0  |
| ADD R3 R3 R0 | 1     | 20 | 25 | 45 | 20 | 0  |
| ADDI R4 R4 1 | 2     | 20 | 25 | 45 | 20 | 1  |
| ADD R3 R3 R0 | 3     | 20 | 25 | 45 | 45 | 1  |
| ADDI R4 R4 1 | 4     | 20 | 25 | 45 | 45 | 2  |
| ADD R3 R3 R0 | 5     | 20 | 25 | 45 | 90 | 2  |
| ADDI R4 R4 1 | 6     | 20 | 25 | 45 | 90 | 3  |
| DIV R3 R4    | 7     | 20 | 25 | 45 | 30 | 3  |

Toy example of an execution trace for a fictitious computation F of 7 instructions on 5 registers.&#x20;
