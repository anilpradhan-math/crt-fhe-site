---
title: CRT-FHE
layout: default
---

<style>
.hero {
  padding: 3.5rem 1rem 2rem;
  border-radius: 18px;
  background: linear-gradient(135deg, #0f172a 0%, #164e63 52%, #1e3a8a 100%);
  color: #f8fafc;
  box-shadow: 0 20px 40px rgba(2, 6, 23, 0.25);
}
.hero h1 { margin: 0 0 0.75rem; font-size: clamp(2.3rem, 5vw, 4.4rem); }
.hero p { max-width: 60rem; font-size: 1.05rem; line-height: 1.7; color: #e2e8f0; }
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1rem;
  margin: 1.5rem 0;
}
.card {
  background: #fff;
  border: 1px solid #e5e7eb;
  border-radius: 16px;
  padding: 1.1rem 1.2rem;
  box-shadow: 0 10px 20px rgba(15, 23, 42, 0.04);
}
.eyebrow {
  text-transform: uppercase;
  letter-spacing: 0.12em;
  font-size: 0.78rem;
  font-weight: 700;
  color: #0f766e;
}
.muted { color: #475569; }
table { display: block; overflow-x: auto; width: 100%; }
</style>

<div class="hero">
  <div class="eyebrow">Project site</div>
  <h1>CRT-FHE</h1>
  <p>
    CRT-FHE is a novel homomorphic encryption construction and a Rust implementation of that construction built on
    the <code>fheanor</code> ecosystem. This site is the public home for the implementation, benchmark notes, and
    the accompanying paper.
  </p>
  <p>
    Paper: <a href="https://eprint.iacr.org/2024/1105" style="color:#bfdbfe;">https://eprint.iacr.org/2024/1105</a>
  </p>
</div>

## Overview

CRT-FHE is intended for research, experimentation, and reproducible implementation work around a CRT-style HE construction.
It is not positioned as an application-ready encryption library.

<div class="grid">
  <div class="card">
    <div class="eyebrow">Implementation</div>
    <p class="muted">Core code lives in `fheanor/src/crt_fhe` with a working end-to-end example.</p>
  </div>
  <div class="card">
    <div class="eyebrow">Paper</div>
    <p class="muted">The construction is described in the ePrint paper linked above.</p>
  </div>
  <div class="card">
    <div class="eyebrow">Scope</div>
    <p class="muted">This is research software: useful for evaluation, benchmarking, and further development.</p>
  </div>
</div>

## What is implemented

- CRT-FHE core module in `fheanor/src/crt_fhe`
- End-to-end usage example in `fheanor/examples/crt_fhe_basics`
- Supporting arithmetic and ring infrastructure through `feanor-math`

## Quick start

From the repository root:

```bash
cargo run -p fheanor --example crt_fhe_basics
```

The example demonstrates:

- parameter selection
- ciphertext ring construction
- key generation
- symmetric encryption
- public-key encryption
- homomorphic addition, multiplication, and squaring
- decryption validation

## Benchmarks

The benchmark note in this repository was generated with:

```bash
cargo run -p fheanor --release --example fhe_scheme_bench
```

Benchmark parameters:

- polynomial degree: `8192`
- ciphertext RNS primes: `[35175245135873, 35156991246337]`
- plaintext modulus `p1`: `65537`
- auxiliary modulus `p2`: `3`
- timed iterations: encrypt `20`, add `100`, multiply + relin `10`
- warmup: `3`

Benchmark results:

| Scheme | Operation | Iterations | Total ms | Latency ms/op | Throughput ops/s |
|---|---:|---:|---:|---:|---:|
| CRT-FHE | Encrypt | 20 | 31.663 | 1.583 | 631.66 |
| CRT-FHE | Add | 100 | 6.196 | 0.062 | 16139.77 |
| CRT-FHE | Multiply + Relin | 10 | 13.198 | 1.320 | 757.70 |
| BFV | Encrypt | 20 | 23.497 | 1.175 | 851.17 |
| BFV | Add | 100 | 6.773 | 0.068 | 14763.87 |
| BFV | Multiply + Relin | 10 | 118.590 | 11.859 | 84.32 |
| BGV | Encrypt | 20 | 32.105 | 1.605 | 622.95 |
| BGV | Add | 100 | 6.219 | 0.062 | 16079.43 |
| BGV | Multiply + Relin | 10 | 13.277 | 1.328 | 753.20 |

Notes from the benchmark run:

- BFV uses the requested two-prime ciphertext modulus for ciphertexts, and its multiplication path constructs an internal extended modulus for rescaling.
- CRT-FHE currently benchmarks coefficient-packed plaintexts.
- The explicit NTT/INTT batch encoder described in the paper is not yet implemented in `crt_fhe`.

See the [Benchmarks](benchmarks) page for the full summary.

## Repository layout

- `fheanor/src/crt_fhe` contains the CRT-FHE implementation
- `fheanor/examples/crt_fhe_basics` contains a minimal end-to-end demo
- `fheanor/Readme.md` documents the broader `fheanor` library
- `benchmarks/` contains benchmark notes and comparison material

## Caveats

- The construction is experimental.
- API and parameter choices may change as the implementation evolves.
- This code should be evaluated as research software, not production cryptography.

## Citation

If you use the construction or implementation in your work, cite the paper:

```text
@misc{eprint2024-1105,
  title = {CRT-FHE},
  year = {2024},
  howpublished = {Cryptology ePrint Archive},
  url = {https://eprint.iacr.org/2024/1105}
}
```

## License

This repository is licensed under the MIT license.

