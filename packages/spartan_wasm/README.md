### Compile

Install wasm-pack

```
curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh
```

Run compile script

```
cd ../.. && sh ./scripts/build_wasm.sh
```

# Testing new circuits

Install circom-secq as `circom-secq`.

Compile your circuit and generate the witness like so:
```
circom-secq <circuit-name>.circom --r1cs --wasm --prime secq256k1 -o .
node <circuit-name>_js/generate_witness.js <circuit-name>_js/<circuit-name>.wasm <circuit-name>_inputs.json witness.wtns
```
Make sure all circom files are version `2.1.2` or lower, and poseidon is done with the implementation in this repo

Create a new dir named `<circuit-name>` with `<circuit-name>.r1cs` and `witness.wtns`

Adapt `src/wasm.rs` to test the new file

Run:
```
cd ../..
cargo run --release --bin gen_spartan_inst packages/spartan_wasm/<circuit-name>/<circuit-name>.r1cs packages/spartan_wasm/<circuit-name>/<circuit-name>.circuit <num-public-inputs>
cd packages/spartan_wasm
cargo test <function-name>
```

`<num-public-inputs>` doesn't count the outputs