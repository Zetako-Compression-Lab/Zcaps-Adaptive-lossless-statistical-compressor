# ZCaps benchmark record

This repository collects measured ZCaps results from several independent benchmark campaigns.

Results from different campaigns are kept separate because hardware, workloads and ZCaps generations differ.

## Benchmark index

| Campaign | Scope | Headline |
|---|---|---|
| [ZMDC-1G modern data](ZMDC-1G.md) | 1.00 GB, 9 modern workload families | **150.05 MB** aggregate output; ZCaps led **7 of 9** workload families |
| [Broad 41.1 GiB corpus](GENERAL-41G.md) | 1,663 files, 25 corpora, 3 levels | **41.1 GiB → 6.09 GiB** at level 12 |
| [Genomics and selective access](GENOMICS.md) | 30 large scientific datasets, 3 levels | Up to **134.6:1** on VCF chr21; measured 4 MiB targeted extraction between **0.11 s and 0.75 s** |
| [Parallel workload scaling](PARALLELISM.md) | Frozen ZMDC-1G batch | **3.16×** aggregate compression throughput and **2.69×** aggregate decode throughput with 4 independent processes vs 1 |

## ZMDC-1G v1 comparison snapshot

Frozen corpus: **1,000,037,807 primary bytes**, 13 primary files, 9 workload families.

| Compressor configuration | Compressed size | Ratio | Compression | Decompression |
|---|---:|---:|---:|---:|
| **ZCaps V12 -6** | **150.05 MB** | **6.66×** | 36.25 MB/s | 41.34 MB/s |
| xz -6 | 159.53 MB | 6.27× | 6.29 MB/s | 154.24 MB/s |
| bzip2 -6 | 165.08 MB | 6.06× | 19.65 MB/s | 78.61 MB/s |
| 7-Zip LZMA2 -5 | 168.27 MB | 5.94× | 6.29 MB/s | 230.60 MB/s |
| Brotli -6 | 174.09 MB | 5.74× | 131.02 MB/s | 882.86 MB/s |
| zstd -6 | 182.47 MB | 5.48× | 241.32 MB/s | 2142.66 MB/s |
| gzip -6 | 199.88 MB | 5.00× | 125.93 MB/s | 534.81 MB/s |
| LZ4 -6 | 237.03 MB | 4.22× | 166.72 MB/s | 3068.59 MB/s |

The ZMDC corpus was frozen before this comparison. The corpus and its reference hashes are public in the [Zetako Modern Data Corpus repository](https://github.com/Zetako-Compression-Lab/zetako-modern-data-corpus).

All reported ZCaps candidates were validated as lossless using exact output verification. Measured results describe the stated campaigns and are not universal guarantees for arbitrary data or hardware.
