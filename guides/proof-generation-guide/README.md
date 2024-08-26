# Proof Generation Guide

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
export ARGS='[PUBLIC] [PRIVATE]'
export SEG_OUTPUT=/tmp/output
export SEG_FILE_DIR=/tmp/output
```

{% tabs %}
{% tab title="Golang" %}
## 2. Compile the Go Program into a MIPS Executable

<pre class="language-bash"><code class="lang-bash"><strong>GOOS=linux GOARCH=mips GOMIPS=softfloat go build -C /your/program/location
</strong></code></pre>

This produces an ELF binary

```bash
export ELF_PATH=/your/program/location
cd $BASEDIR
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
rustflags = ["--cfg", 'target_os="zkvm"',"-C", "target-feature=+crt-static", "-C"
```

Compile your program to MIPS, this produces an ELF Binary

```bash
cd path/to/rust/code
cargo build --target=mips-unknown-linux-musl
```

```bash
export ELF_PATH=/your/elf/location
cd $BASEDIR
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you previously ran a program that generated segments, make sure to clear the segments with `rm -rf /tmp/output`
{% endhint %}

## 3. Split the ELF into Segments

```sh
cargo run --release --example zkmips split
```

## 4. Prove and Aggregate the Individual Segments

```sh
SEG_FILE_NUM=$(ls /tmp/output | wc -l) \
    cargo run --release --example zkmips prove_segments
```

{% hint style="info" %}
`ls /tmp/output | wc -l` outputs how many segments are present in the output file directory.
{% endhint %}

After aggregating the proof, you should receive a result: `verifier/data/test_circuit` with the files:

```
common_circuit_data.json  proof_with_public_inputs.json  verifier_only_circuit_data.json
```

After this step you will [Verify the Proof](../proof-verification-guide/).
