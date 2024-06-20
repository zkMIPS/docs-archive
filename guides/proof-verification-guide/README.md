# Proof Verification Guide

Before you begin, make sure you have generated the proof in the previous page.

{% embed url="https://github.com/zkMIPS/gnark-plonky2-verifier" %}

## Convert plonky2 proof to groth16 proof

Copy the three files generated in the previous step to the `gnark-plonky2-verifier/testdata/mips` directory. Convert the aggregated proof to a groth16 proof and export `testdata/mips/verifying.key` and `testdata/mips/proof.proof`.

```sh
git clone https://github.com/zkMIPS/gnark-plonky2-verifier.git
cd gnark-plonky2-verifier
cp verifier/data/test_circuit/* testdata/mips
go build benchmark.go
./benchmark
```

Generate the on chain verification contract.

```sh
cd gnark-plonky2-verifier
go build gnark_sol_caller.go
./gnark_sol_caller generate --outputDir hardhat/contracts
```

Using verifier contract to verify proof (requires installing hardhat).

```bash
cp testdata/mips/snark_proof_with_public_inputs.json hardhat/test/
npx hardhat test
```

## Deploy Verifier Contract

```sh
npx hardhat ignition deploy ./ignition/modules/Verifier.js --network sepolia
```

Output the contract address as follows:

```
verifierAddress: [ADDRESS] 
txHash: [HASH]
```

Call the contract to verify the proof.

```sh
./gnark_sol_caller verify [ADDRESS]
```

Upon success, it will output the following information:

```
proofInputs[0]:16938917044215216036790430797642791383920795117631897310451186928034381626050
……
proofCommitmentX:21494175448761027309703698464680493202018688476655994006880780472639477125857,proofCommitmentY:4962139455858354758170525479413131443741348412676425864558923303531752088581
verify proof txHash: 0x9fd5e351171da578617db428926d39c4d48e02c8b173c3867126412c182c7a99
```

You can view the transaction details for the hash.

Congrats! You have just deployed your first proof. You can try using your proof integrated with the [use-cases](https://github.com/zkMIPS/use-cases) of ZKM.
