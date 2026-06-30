---
title: Benchmarks
layout: default
---

# Benchmarks

This page summarizes the benchmark run captured in `benchmarks/fhe_scheme_comparison.md`.

The benchmark command used was:

```bash
cargo run -p fheanor --release --example fhe_scheme_bench
```

## Parameters

- polynomial degree: `8192`
- ciphertext RNS primes: `[35175245135873, 35156991246337]`
- plaintext modulus `p1`: `65537`
- auxiliary modulus `p2`: `3`
- timed iterations: encrypt `20`, add `100`, multiply + relin `10`
- warmup: `3`

## Results

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

## Notes

- BFV uses the requested two-prime ciphertext modulus for ciphertexts, and its multiplication path constructs an internal extended modulus for rescaling.
- CRT-FHE currently benchmarks coefficient-packed plaintexts.
- The explicit NTT/INTT batch encoder described in the paper is not yet implemented in `crt_fhe`.

