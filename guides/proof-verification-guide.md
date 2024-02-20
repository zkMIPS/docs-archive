# Proof Verification Guide

Before you begin, make sure you have generated the proof in the previous page.

{% embed url="https://github.com/zkMIPS/gnark-plonky2-verifier/tree/verifier" %}

## Convert plonky2 proof to groth16 proof

Copy the three files generated in the previous step to the `gnark-plonky2-verifier/testdata/mips` directory. Convert the aggregated proof to a groth16 proof and export `testdata/mips/verifying.key` and `testdata/mips/proof.proof`.

```sh
git clone https://github.com/zkMIPS/gnark-plonky2-verifier.git
cd gnark-plonky2-verifier
git checkout -b verifier origin/verifier
cp verifier/data/test_circuit/* testdata/mips
go build benchmark.go
./benchmark
```

Print verifying.key information. This information will be needed later to modify the contract's verification key.

{% code title="gnark-plonky2-verifier" %}
```sh
go build gnark_sol_caller.go
./gnark_sol_caller printvk
```
{% endcode %}

It will output information similar to the following:

```solidity
vk.alpha = Pairing.G1Point(uint256( 1294408955923622594591725983762070642378777529397747490329012940649339990249 ), uint256( 15055448664659765045433013056797302099072693492335778897044080823510094653496 ));
......
vk.gamma_abc[ 65 ] = Pairing.G1Point(uint256( 3034197166604929635959063693106443562503713417717817201702314253957207425558 ), uint256( 17696664884911750669939728139940642052511847578723212348786773570916456946833 ));
```

Replace the vk in the verifyingKey function with the printed information as follows:

{% code title="testdata/mips/verifier.sol" %}
```solidity
function verifyingKey() pure internal returns (VerifyingKey memory vk) {
        vk.alpha = Pairing.G1Point(uint256( 1294408955923622594591725983762070642378777529397747490329012940649339990249 ), uint256( 15055448664659765045433013056797302099072693492335778897044080823510094653496 ));
        vk.beta = Pairing.G2Point([uint256( 20879129812265967977654457346990792345826680255313020164667451271722214353675 ), uint256( 18513167028708009191762138213791443417765490687920657420917787670413655882129 )], [uint256( 4204284630035407477687589152447974477101370904798037125745122462798582097003 ), uint256( 10381416552874558668320753387005511736229582168955720272430706214871791274152 )]);
        vk.gamma = Pairing.G2Point([uint256( 15374003566975740287113075106363823155507938014460623587154394111252097087200 ), uint256( 10565797873394641304726457054800840391949051977145312445325689670160995925666 )], [uint256( 1411575822771164473947698404191977645754501988171559778118392142971549428226 ), uint256( 9114070495093862029079719384228219586557500522589528347280079044243078884345 )]);
        vk.delta = Pairing.G2Point([uint256( 20485235070954623769574744154112975817791974873883675366848978859417191626566 ), uint256( 7183343957641466840709230898421986181429516639529201387537096313740083166317 )], [uint256( 6189874267936442423764253628088886137966468185177358593950501365913801667934 ), uint256( 8879254720040774128262527078805481737022950844380203733394391637832288322914 )]);
        ...
        vk.gamma_abc[ 65 ] = Pairing.G1Point(uint256( 3034197166604929635959063693106443562503713417717817201702314253957207425558 ), uint256( 17696664884911750669939728139940642052511847578723212348786773570916456946833 ));

    }
```
{% endcode %}

Compile the contract and generate ABI and bytecode&#x20;

{% hint style="info" %}
Installing solc: [https://docs.soliditylang.org/en/latest/installing-solidity.html](https://docs.soliditylang.org/en/latest/installing-solidity.html)
{% endhint %}

```sh
solc --abi --bin ./testdata/mips/verifier.sol -o testdata/mips --overwrite
```

Generate contract invocation code

{% hint style="info" %}
Installing abigen: [https://goethereumbook.org/smart-contract-compile](https://goethereumbook.org/smart-contract-compile/)
{% endhint %}

```sh
abigen --abi ./testdata/mips/Verifier.abi --bin ./testdata/mips/Verifier.bin --pkg verifier --type contract --out ./verifier/sol_verifier.go
```

## Deploy Verifier Contract

```sh
./gnark_sol_caller deploy
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
