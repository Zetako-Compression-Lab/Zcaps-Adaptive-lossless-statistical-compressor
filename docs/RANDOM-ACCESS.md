# ZCaps selective / random access

ZCaps supports targeted range extraction from compressed data. An application can request a small byte range from a much larger compressed dataset without first reconstructing the complete source.

This repository uses two closely related terms:

- **selective access** describes the product capability: retrieving only the range a workload needs;
- **random access** describes the access pattern: the requested range can be located away from the beginning of the source rather than requiring a full sequential decode.

The public benchmark record measures byte-range extraction. It does not claim database query execution, record-aware filtering or format-specific indexing.

## Published validation

The large scientific-data campaign covered **30 datasets** at ZCaps levels 1, 6 and 12:

| Measure | Result |
|---|---:|
| Compressed archives tested | **90** |
| Targeted extractions | **270** |
| Requested output per extraction | **4 MiB** |
| Source dataset range | **1.65–98.90 GiB** |
| Observed extraction time | **0.11–0.75 s** |
| Validation | Exact byte-for-byte match |
| Benchmark system | Intel Core i9-13980HX, 16 GiB RAM, NVMe SSD, Windows 11 |

Every reported extraction was checked against the corresponding source range. Results span multiple structured scientific formats, including VCF, FASTA, FASTQ, SAM, GFF3, GTF, JSON, CSV, BED, MAF and protein datasets.

## Representative results

| Dataset | Source size | Level | Targeted 4 MiB extraction |
|---|---:|---:|---:|
| VCF chr21, 1000 Genomes phase 3 | 11.23 GiB | 1 | **0.11 s** |
| Human GFF3 | 1.65 GiB | 1 | 0.15–0.16 s |
| Human FASTA | 64.20 GiB | 1 | 0.12–0.36 s |
| GOA UniProt GAF | 98.90 GiB | 1 | 0.302–0.315 s |
| VCF chr21, 1000 Genomes phase 3 | 11.23 GiB | 12 | 0.16–0.17 s |
| Human FASTA | 64.20 GiB | 12 | 0.16–0.69 s |
| GBFF | 2.86 GiB | 12 | 0.42–0.75 s |

The full per-dataset tables are available for [level 1](GENOMICS-L1.md), [level 6](GENOMICS-L6.md) and [level 12](GENOMICS-L12.md).

## Why it matters

Large datasets are often written once and read in small pieces. Targeted extraction allows a system to preserve the storage and transfer benefits of compression while avoiding a full-file decode for every partial read.

That capability is relevant to:

- scientific and genomics pipelines that inspect regions of large files;
- telemetry, logs and observability systems that retrieve bounded time or byte ranges;
- data platforms that keep colder data compressed but still need partial retrieval;
- future storage architectures that coordinate compressed data across hot and cold tiers.

ZCaps is not presented here as a complete data engine. The validated capability is compressed byte-range extraction; broader indexing, query and tier-management layers remain product direction until implemented and measured.

## Interpreting the measurements

The reported times are end-to-end observations from one benchmark campaign, not universal latency guarantees. Extraction performance can vary with compression level, dataset structure, chunk layout, requested range, storage device, operating system and host load.

This campaign used a separate ZCaps build from the current ZMDC V12 comparison. Results from those campaigns should not be combined as though they were collected from the same build or environment.

See the [genomics campaign overview](GENOMICS.md) for aggregate compression results and the [benchmark index](BENCHMARKS.md) for the complete public record.
