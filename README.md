# ZCaps

### Adaptive lossless compression for modern data infrastructure

> **ZCaps is Zetako's proprietary lossless compression engine for structured, machine-generated and scientific data.**

ZCaps is developed by **Zetako Compression Lab** as a general-purpose compression layer focused on compression density, exact reconstruction, selective and random access, and modern server workloads.

[![Lossless](https://img.shields.io/badge/compression-lossless-0A7D5A)](#lossless)
[![Benchmarks](https://img.shields.io/badge/benchmarks-public-4B5DFF)](docs/BENCHMARKS.md)
[![Implementation](https://img.shields.io/badge/core-proprietary-555555)](#public-results-private-implementation)

**Website:** https://zetako.ai/products/zcaps  
**Benchmark index:** [docs/BENCHMARKS.md](docs/BENCHMARKS.md)  
**Modern data corpus:** https://github.com/Zetako-Compression-Lab/zetako-modern-data-corpus

---

## What ZCaps is built for

Modern infrastructure continuously creates data that must be stored, moved and retrieved: API events, telemetry, logs, observability streams, database changes, enterprise records and scientific datasets.

ZCaps is designed as a compression layer for that environment.

Publicly validated capabilities include:

- **lossless compression** — exact source reconstruction;
- **adaptive behavior** across heterogeneous data rather than one file-type-specific path;
- **high compression density** on structured and machine-generated workloads;
- **selective / random range access** demonstrated in a separate large scientific-data campaign;
- **parallel workload scaling** across independent files and processes;
- **bounded deployment footprint** suitable for integration into private infrastructure.

The current ZCaps product line is integrated into Zetako's own software stack, including ZNode compression workflows.

---

## Selective / random access

ZCaps can extract a requested range from a compressed dataset without requiring the application to reconstruct the entire source first. This makes compression useful not only for archival storage, but also for workflows that repeatedly retrieve small regions from much larger datasets.

The published scientific-data campaign validated:

- **270 targeted extractions** across 90 compressed archives;
- a fixed requested range of **4 MiB** per extraction;
- observed extraction times from **0.11 s to 0.75 s** on datasets as large as **98.90 GiB**;
- exact byte-for-byte validation of every extracted range.

These are measured campaign results, not universal latency guarantees. Performance depends on the dataset, compression level, chunk layout, storage and host system.

[Selective / random access overview →](docs/RANDOM-ACCESS.md)

### Direction: compressed-data infrastructure

ZCaps is being developed as a building block for data systems that keep data compressed while supporting targeted retrieval and partial access. This creates a path toward storage workflows that can place and access data according to operational needs, including future hot/cold tiering scenarios.

This is a product direction, not a claim that ZCaps is already a complete database or tiered-storage engine. Public claims in this repository remain limited to implemented and measured capabilities.

---

# Benchmark highlights

## 1 GB modern-data corpus → 150.05 MB

On the frozen **ZMDC-1G v1** corpus, ZCaps produced the smallest aggregate output among the tested configurations.

| Configuration | Compressed size | Ratio |
|---|---:|---:|
| **ZCaps V12 -6** | **150.05 MB** | **6.66×** |
| xz -6 | 159.53 MB | 6.27× |
| bzip2 -6 | 165.08 MB | 6.06× |
| 7-Zip LZMA2 -5 | 168.27 MB | 5.94× |
| Brotli -6 | 174.09 MB | 5.74× |
| zstd -6 | 182.47 MB | 5.48× |
| gzip -6 | 199.88 MB | 5.00× |
| LZ4 -6 | 237.03 MB | 4.22× |

ZCaps led **7 of 9** ZMDC workload families: API, collaboration, database/CDC, logs, observability, telemetry and transactions.

The high-entropy control remained essentially incompressible at approximately **1:1**, providing an important control against artificial compression claims.

[Full ZMDC results →](docs/ZMDC-1G.md)

The ZMDC corpus was frozen before the codec comparison and is published independently with its reference release and hashes.

---

## 41.1 GiB heterogeneous corpus

A broader campaign covered **1,663 files across 25 corpora**, including genomic data, JSON, structured text, medical imaging, object meshes, text, binary data, audio and raw video.

| ZCaps level | Aggregate ratio | Space reduction | Encode | Decode |
|---|---:|---:|---:|---:|
| Default | 6.00× | 83.33% | 86.56 MiB/s | 64.63 MiB/s |
| 6 | 6.40× | 84.39% | 61.79 MiB/s | 55.45 MiB/s |
| 12 | **6.76×** | **85.20%** | 47.74 MiB/s | 34.44 MiB/s |

At level 12, **41.1 GiB was reduced to approximately 6.09 GiB**.

[Full 25-corpus results →](docs/GENERAL-41G.md)

---

## Genomics and scientific data

A separate scientific-data campaign covered **30 large datasets**, three ZCaps levels, **90 compression runs** and **270 targeted 4 MiB extraction runs** on an Intel Core i9-13980HX laptop with NVMe storage.

Selected maximum-density results:

| Dataset | Source size | ZCaps ratio |
|---|---:|---:|
| VCF chr21 — 1000 Genomes phase 3 | 11.23 GiB | **134.6×** |
| GTF | 4.45 GiB | **73.07×** |
| CSV AllNuclMetadata | 7.83 GiB | **59.77×** |
| Human FASTA | 64.20 GiB | **36.78×** |
| Human GFF3 | 1.65 GiB | **36.08×** |
| dbSNP chrY JSON | 13.13 GiB | **33.31×** |
| OrthoXML Compara 116 | 28.85 GiB | **29.06×** |
| GOA UniProt GAF | 98.90 GiB | **18.63×** |

Across the documented campaign, targeted **4 MiB** extraction was observed between **0.11 s and 0.75 s**, with exact extracted output validation.

This campaign used a separate ZCaps build from the current ZMDC V12 comparison and is reported independently to avoid mixing build generations or hardware environments.

[Genomics overview →](docs/GENOMICS.md) · [Selective / random access →](docs/RANDOM-ACCESS.md)

[Level 1 table →](docs/GENOMICS-L1.md) · [Level 6 table →](docs/GENOMICS-L6.md) · [Level 12 table →](docs/GENOMICS-L12.md)

---

## Parallel workload scaling

On the frozen ZMDC-1G batch, processing independent files concurrently increased aggregate throughput without changing the compression ratio.

| Concurrent processes | Compression | Decompression | Ratio |
|---:|---:|---:|---:|
| 1 | 35.79 MB/s | 41.03 MB/s | 6.66× |
| 4 | **113.25 MB/s** | **110.22 MB/s** | 6.66× |

Measured median batch scaling from one to four independent ZCaps processes:

- **3.16× compression throughput**
- **2.69× decompression throughput**

This is **multi-file workload scaling**, not a claim that the current V12 build internally accelerates one file with four threads.

[CPU, memory and scaling results →](docs/PARALLELISM.md)

---

## Historical reference

Zetako Compression Lab also maintains a reconstructed historical ZCaps reference as a research baseline. It completed a 50-file suite with exact round trips and achieved **71.34% aggregate space reduction**, but at much lower throughput than the modern line.

[Historical reference benchmark →](docs/HISTORICAL-REFERENCE.md)

---

## Lossless

Every benchmark result published here is expected to reconstruct the original data exactly. Public benchmark campaigns use exact output verification before a result is accepted.

```text
decode(encode(data)) == data
```

Compression density never takes precedence over exact recovery.

---

## Benchmark record

The benchmark documentation is organized by campaign so that results from different machines and ZCaps generations are not presented as if they came from one identical build.

- [Benchmark index](docs/BENCHMARKS.md)
- [ZMDC-1G modern data](docs/ZMDC-1G.md)
- [41.1 GiB broad corpus](docs/GENERAL-41G.md)
- [Selective / random access](docs/RANDOM-ACCESS.md)
- [Genomics and selective access](docs/GENOMICS.md)
- [Parallel scaling and resources](docs/PARALLELISM.md)
- [Historical reference](docs/HISTORICAL-REFERENCE.md)

Measured results are workload- and platform-specific. They are not universal guarantees for arbitrary inputs or hardware.

---

## Public results, private implementation

This repository is a **public research and benchmark record**.

It publishes:

- measured compression results;
- workload and platform context;
- lossless-validation status;
- public research milestones;
- capability-level product information.

The production ZCaps source code and proprietary implementation details remain private.

---

## About Zetako

**Zetako Compression Lab** develops original technologies for compression, data infrastructure and sovereign software.

- Website: https://zetako.ai
- ZCaps: https://zetako.ai/products/zcaps
- ZMDC: https://github.com/Zetako-Compression-Lab/zetako-modern-data-corpus
- Contact: contact@zetako.ai

© Zetako SARL
