# Example Execute Operations

{% embed url="https://github.com/zkMIPS/gnark-plonky2-verifier/blob/main/hardhat/contracts/verifier.sol#L267" %}

```solidity
function verifyTx(Proof memory proof, uint[65] memory input, uint[2] memory proof_commitment) public returns (bool r) {
        if (verify(input, proof , proof_commitment) == 0) {
            emit VerifyEvent(msg.sender);
            // Replace with code
            return true; 
        } else {
            // Replace with code
            return false; 
        }
}
```

