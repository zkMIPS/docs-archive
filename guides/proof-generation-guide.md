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
export BASEDIR=/path/to/your/file/directory
export SEG_SIZE=[SEE CYCLES ABOVE FOR EXACT VALUE BASED ON YOUR RAM]
export ARGS='[PUBLIC_VALUE] [PRIVATE_VALUE]'
export SEG_OUTPUT=/tmp/output
export SEG_FILE_DIR=/tmp/output
```

## 2. Compile the Program to MIPS

Write your own program, and compile with

{% tabs %}
{% tab title="Golang" %}
<pre class="language-bash"><code class="lang-bash"><strong>GOOS=linux GOARCH=mips GOMIPS=softfloat go build [YOUR_PROGRAM_NAME].go
</strong></code></pre>

This produces an ELF binary.

```bash
export ELF_PATH=/path/to/your/binary
```

## 2. Split the ELF into Segments

{% hint style="info" %}
If you previously ran a program that generated segments, make sure to clear the segments with `rm -rf /tmp/output`&#x20;
{% endhint %}

```sh
ARGS='[PUBLIC_VALUE] [PRIVATE_VALUE]' \
    cargo run --release --example zkmips split
```
{% endtab %}

{% tab title="Rust" %}
{% hint style="warning" %}
This currently only works on Linux, not on MacOS
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

This produces an ELF binary in `target/mips-unknown-linux-musl/debug/sha2-bench`

```bash
export ELF_PATH=/path/to/your/binary
```

## 2. Split the ELF into Segments

{% hint style="info" %}
If you previously ran a program that generated segments, make sure to clear the segments with `rm -rf /tmp/output`&#x20;
{% endhint %}

```bash
ARGS='711e9609339e92b03ddc0a211827dba421f38f9ed8b9d806e1ffdd8c15ffa03d world!'\
    cargo run --release --example zkmips bench
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
