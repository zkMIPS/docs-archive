# Quick Start Example

{% embed url="https://github.com/zkMIPS/zkm" %}

## Minimum Requirements

| Cycles Per Segment | RAM |
| ------------------ | --- |
| 32768              | 13G |
| 65536              | 19G |
| 262144             | 27G |

## 1. Set the Default Variables

```bash
export RUST_LOG=info
export BASEDIR=/home/ubuntu/zkm # Replace with your folder location
export SEG_SIZE=65536 # See cycles above for exact value based on your RAM
export ARGS='2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824 hello'
export SEG_OUTPUT=/tmp/output
export SEG_FILE_DIR=/tmp/output
```

{% tabs %}
{% tab title="Golang" %}
## 2. Compile the Go Program into a MIPS Executable

<pre class="language-bash"><code class="lang-bash"><strong>GOOS=linux GOARCH=mips GOMIPS=softfloat go build -C prover/examples/sha2-go
</strong></code></pre>

This produces an ELF binary in `prover/examples/sha2-go/sha2-go`

```bash
export ELF_PATH=$BASEDIR/prover/examples/sha2-go/sha2-go
```

## 3. Generate the Proof from the ELF

```bash
HOST_PROGRAM=sha2_go \
    cargo run --release --example zkmips prove_host_program
```
{% endtab %}

{% tab title="Rust" %}
## 2. Compile the Rust Program into a MIPS Executable

{% hint style="warning" %}
The compilation of the program only works on Linux, not on MacOS
{% endhint %}

Download and install toolchain for MIPS

```bash
wget http://musl.cc/mips-linux-muslsf-cross.tgz
tar -zxvf mips-linux-muslsf-cross.tgz
```

Modify `~/.cargo/config`

```bash
[target.mips-unknown-linux-musl]
linker = "<path-to>/mips-linux-muslsf-cross/bin/mips-linux-muslsf-gcc"
rustflags = ["--cfg", 'target_os="zkvm"',"-C", "target-feature=+crt-static", "-C", "link-arg=-g"]
```

Build the sha2 example

```bash
cd prover/examples/sha2-rust
cargo build --target=mips-unknown-linux-musl
```

This produces an ELF binary in `prover/examples/sha2/target/mips-unknown-linux-musl/debug/sha2-bench`

```bash
export ELF_PATH=$BASEDIR/prover/examples/sha2/target/mips-unknown-linux-musl/debug/sha2-rust
```

## 3. Generate the Proof from the ELF

```bash
cd ../..
```

```bash
HOST_PROGRAM="sha2_rust" \
    cargo run --release --example zkmips prove_host_program
```
{% endtab %}
{% endtabs %}

After generating the proof, you should receive a result: `../verifier/data/test_circuit` with the files:

```
common_circuit_data.json  proof_with_public_inputs.json  verifier_only_circuit_data.json
```

After this step you will [Verify the Proof](../proof-verification-guide/).
