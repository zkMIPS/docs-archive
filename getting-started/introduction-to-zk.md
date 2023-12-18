# Introduction to ZK

To get started within ZKM, let's explore the basics of ZK Proofs

{% embed url="https://www.youtube.com/watch?v=C0uJs8hwaRY&list=PLJ_r35m80nKhImOYtUj1MsVhcepHH4x_Z" %}

## In Summary

* A Prover generates a SNARK Proof which proves correctness and (if intended) privacy of the computation.
  * **S**uccinct - short in size
  * **N**on-interactive - Verifier does not need to interact with the Prover to Verify the Proof
  * **AR**gument - "Proof"
  * **K**nowledge
* Properties of a SNARK:
  * **Completeness** - If both parties are honest, the protocol gives the correct result
  * **Soundness** - If the Prover is trying to cheat, the Verifier will accept with negligible probability
  * **Succinctness** - The proof is short AND verification is easy
  * **Privacy (Optional)** - The protocol does not leak useful information to the Verifier
* How a zkRollup works:
  * Outsource all expensive computations off Ethereum to a Layer 2
  * Send the results with a proof (SNARK) to Layer 1

