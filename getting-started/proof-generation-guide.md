# Proof Generation Guide

{% embed url="https://github.com/zkMIPS/zkm" %}

## Requirements

* UNIX machine with at least 64 GB of RAM

## 1. Compile the Go code to MIPS

Write your own go program, and compile with

```sh
export GOOS=linux
export GOARCH=mips
export GOMIPS=softfloat
go build [YOUR_PROGRAM_NAME].go
```

This produces an ELF binary.

```sh
export BASEDIR=/path/to/your/go/file/directory
export ELF_PATH=/path/to/your/go/binary
```

## 2. Split the ELF into Segments

{% hint style="warning" %}
This should take a **few seconds**
{% endhint %}

```sh
RUST_LOG=trace BLOCK_NO=13284491 SEG_OUTPUT=/tmp/output SEG_SIZE=262144 \
    cargo run --release --example zkmips split
```

## 3. Generate Proof for Each Segment

{% hint style="warning" %}
This should take about **10 minutes**
{% endhint %}

```sh
RUST_LOG=trace BLOCK_NO=13284491 SEG_FILE="/tmp/output/0" SEG_SIZE=262144 \
    cargo run --release --example zkmips prove
```

## 4. Aggregate Proofs

{% hint style="warning" %}
This should take **20 minutes to a few hours**, depending on the number of segments being aggregated
{% endhint %}

```sh
RUST_LOG=trace BLOCK_NO=13284491 SEG_FILE_DIR="/tmp/output" SEG_FILE_NUM=$(ls /tmp/output -1 | wc -l) SEG_SIZE=262144 \
    cargo run --release --example zkmips aggregate_proof_all
```

{% hint style="info" %}
`ls /tmp/output -1 | wc -l` outputs how many segments are present in the output file directory.
{% endhint %}