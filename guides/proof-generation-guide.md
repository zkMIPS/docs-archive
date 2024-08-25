# Proof Generation Guide

{% embed url="https://github.com/zkMIPS/zkm" %}

## Minimum Requirements

| Cycles Per Segment | RAM |
| ------------------ | --- |
| 32768              | 13G |
| 65536              | 19G |
| 262144             | 27G |

## 1. Set the Variables

```bash
export RUST_LOG=info
export BASEDIR=/home/ubuntu/zkm # Replace with your folder location
export SEG_SIZE=65536 # See cycles above for exact value based on your RAM
export ARGS='[PUBLIC_VALUE] [PRIVATE_VALUE]'
export SEG_OUTPUT=/tmp/output
export SEG_FILE_DIR=/tmp/output
```

{% tabs %}
{% tab title="Golang" %}
## 2. Compile the Program into a MIPS Executable

<pre class="language-bash"><code class="lang-bash">cd prover/examples/add-go
<strong>GOOS=linux GOARCH=mips GOMIPS=softfloat go build .
</strong></code></pre>

This produces an ELF binary in `prover/examples/add-go/go-add`

```bash
export ELF_PATH=$BASEDIR/prover/examples/add-go/go-add
```

## 2. Split the ELF into Segments

{% hint style="info" %}
If you previously ran a program that generated segments, make sure to clear the segments with `rm -rf /tmp/output`
{% endhint %}

```bash
cd ../..
```

To generate a proof normally

```bash
cargo run --release --example zkmips split
```

To bypass the proof generation process (for faster development)

```bash
HOST_PROGRAM=add_example \
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
cd prover/examples/sha2
cargo build --target=mips-unknown-linux-musl
```

This produces an ELF binary in `prover/examples/sha2/target/mips-unknown-linux-musl/debug/sha2-bench`

```bash
export ELF_PATH=$BASEDIR/prover/examples/sha2/target/mips-unknown-linux-musl/debug/sha2-bench
```

## 2. Split the ELF into Segments

{% hint style="info" %}
If you previously ran a program that generated segments, make sure to clear the segments with `rm -rf /tmp/output`
{% endhint %}

```bash
cd ../..
```

```bash
ARGS='711e9609339e92b03ddc0a211827dba421f38f9ed8b9d806e1ffdd8c15ffa03d world!'\
    cargo run --release --example zkmips split
```
{% endtab %}
{% endtabs %}

## 3. Generate Proof for Each Segment

```sh
SEG_FILE="/tmp/output/0" \
    cargo run --release --example zkmips prove
```

## 4. Aggregate Proofs

```sh
SEG_FILE_NUM=$(ls /tmp/output | wc -l) \
    cargo run --release --example zkmips aggregate_proof_all
```

{% hint style="info" %}
`ls /tmp/output | wc -l` outputs how many segments are present in the output file directory.
{% endhint %}

After aggregating the proof, you should receive a result: `verifier/data/test_circuit` with the files:

```
common_circuit_data.json  proof_with_public_inputs.json  verifier_only_circuit_data.json
```

Once you generate this folder, you need to [Verify the Proof](proof-verification-guide/).
