---
description: A zkVM application is broken down into 2 programs
---

# Guest and Host

## Guest

The Guest program is responsible for executing the ELF binary which is proven inside of the zkVM, this involves:&#x20;

* Reading inputs provided by the host
* Writing private outputs to the host
* Committing public outputs to the journal.

{% hint style="info" %}
If there is an error when running a guest program, the proof should not be generated
{% endhint %}

### Example Guest Programs

{% tabs %}
{% tab title="Golang" %}
```go
package main

import (
	"bytes"
	"crypto/sha256"
	"log"
	"github.com/zkMIPS/zkm/go-runtime/zkm_runtime"
)

type DataId uint32

// use iota to create enum
const (
	TYPE1 DataId = iota
	TYPE2
	TYPE3
)

type Data struct {
	Input1  [10]byte
	Input2  uint8
	Input3  int8
	Input4  uint16
	Input5  int16
	Input6  uint32
	Input7  int32
	Input8  uint64
	Input9  int64
	Input10 []byte
	Input11 DataId
	Input12 string
}

func main() {
	a := zkm_runtime.Read[Data]()

	data := []byte(a.Input12)
	hash := sha256.Sum256(data)

	assertEqual(hash[:], a.Input10)

	zkm_runtime.Commit[Data](a)
}

func assertEqual(a []byte, b []byte) {
	if !bytes.Equal(a, b) {
		log.Fatal("%x != %x", a, b)
	}
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
#![no_std]
#![no_main]

use sha2::{Digest, Sha256};
extern crate alloc;
use alloc::vec::Vec;

zkm_runtime::entrypoint!(main);

pub fn main() {
    let public_input: Vec<u8> = zkm_runtime::io::read();
    let input: Vec<u8> = zkm_runtime::io::read();

    let mut hasher = Sha256::new();
    hasher.update(input);
    let result = hasher.finalize();

    let output: [u8; 32] = result.into();
    assert_eq!(output.to_vec(), public_input);

    zkm_runtime::io::commit::<[u8; 32]>(&output);
}
```
{% endtab %}
{% endtabs %}

## Host

The Host program is responsible for setting up the environment for the guest program and evaluating the results from the guest execution, this involves:

* Supplying the inputs to the guest program
* Evaluating the private outputs provided by the guest
* Evaluating the public journal output provided by the guest

### Example Host Program

{% tabs %}
{% tab title="Golang" %}
```rust
fn prove_sha2_go() {
    // 1. split ELF into segs
    let elf_path = env::var("ELF_PATH").expect("ELF file is missing");
    let seg_path = env::var("SEG_OUTPUT").expect("Segment output path is missing");
    let seg_size = env::var("SEG_SIZE").unwrap_or("0".to_string());
    let seg_size = seg_size.parse::<_>().unwrap_or(0);

    let mut state = load_elf_with_patch(&elf_path, vec![]);
    // load input
    let args = env::var("ARGS").unwrap_or("data-to-hash".to_string());
    // assume the first arg is the hash output(which is a public input), and the second is the input.
    let args: Vec<&str> = args.split_whitespace().collect();
    assert_eq!(args.len(), 2);

    let mut data = Data::new();
    
    // Fill in the input data
    data.input10 = hex::decode(args[0]).unwrap();
    data.input12 = args[1].to_string();

    state.add_input_stream(&data);
    log::info!(
        "enum {} {} {}",
        DataId::TYPE1 as u8,
        DataId::TYPE2 as u8,
        DataId::TYPE3 as u8
    );
    log::info!("public input: {:X?}", data);

    let (total_steps, mut state) = split_prog_into_segs(state, &seg_path, "", seg_size);

    let value = state.read_public_values::<Data>();
    log::info!("public value: {:X?}", value);

    let mut seg_num = 1usize;
    if seg_size != 0 {
        seg_num = (total_steps + seg_size - 1) / seg_size;
    }

    if seg_num == 1 {
        let seg_file = format!("{seg_path}/{}", 0);
        prove_single_seg_common(&seg_file, "", "", "", total_steps)
    } else {
        prove_multi_seg_common(&seg_path, "", "", "", seg_size, seg_num, 0).unwrap()
    }
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
fn prove_sha2_rust() {
    // 1. split ELF into segs
    let elf_path = env::var("ELF_PATH").expect("ELF file is missing");
    let seg_path = env::var("SEG_OUTPUT").expect("Segment output path is missing");
    let seg_size = env::var("SEG_SIZE").unwrap_or("0".to_string());
    let seg_size = seg_size.parse::<_>().unwrap_or(0);

    let mut state = load_elf_with_patch(&elf_path, vec![]);
    // load input
    let args = env::var("ARGS").unwrap_or("data-to-hash".to_string());
    // assume the first arg is the hash output(which is a public input), and the second is the input.
    let args: Vec<&str> = args.split_whitespace().collect();
    assert_eq!(args.len(), 2);

    let public_input: Vec<u8> = hex::decode(args[0]).unwrap();
    state.add_input_stream(&public_input);
    log::info!("expected public value in hex: {:X?}", args[0]);
    log::info!("expected public value: {:X?}", public_input);

    let private_input = args[1].as_bytes().to_vec();
    log::info!("private input value: {:X?}", private_input);
    state.add_input_stream(&private_input);

    let (total_steps, mut state) = split_prog_into_segs(state, &seg_path, "", seg_size);

    let value = state.read_public_values::<[u8; 32]>();
    log::info!("public value: {:X?}", value);
    log::info!("public value: {} in hex", hex::encode(value));

    let mut seg_num = 1usize;
    if seg_size != 0 {
        seg_num = (total_steps + seg_size - 1) / seg_size;
    }

    if seg_num == 1 {
        let seg_file = format!("{seg_path}/{}", 0);
        prove_single_seg_common(&seg_file, "", "", "", total_steps)
    } else {
        prove_multi_seg_common(&seg_path, "", "", "", seg_size, seg_num, 0).unwrap()
    }
}
```
{% endtab %}
{% endtabs %}

