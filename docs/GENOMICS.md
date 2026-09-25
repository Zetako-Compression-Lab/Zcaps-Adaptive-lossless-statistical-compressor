# ZCaps genomics and scientific-data benchmark

A separate large-data campaign evaluated ZCaps on **30 scientific and bioinformatics datasets** across three compression levels.

Benchmark system: **Intel Core i9-13980HX, 16 GiB RAM, NVMe SSD, Windows 11**.

The campaign covered **90 compression runs** and **270 targeted 4 MiB extraction runs**. Extracted data was checked against the source for exact lossless reconstruction.

For a capability-focused explanation, methodology summary and scope boundaries, see [Selective / random access](RANDOM-ACCESS.md).

## Campaign totals

- Aggregate input across compression runs: **2.04 TiB**
- Aggregate compressed output: approximately **210 GiB**
- Aggregate compression ratio: approximately **10:1**
- Total compression time: **40 min 10 s**
- Average compression throughput: **888 MiB/s**
- Targeted extractions: **270 × 4 MiB**
- Measured extraction range: **0.11 s to 0.75 s**

## Headline results

| Dataset | Size | Level 1 | Level 6 | Level 12 |
|---|---:|---:|---:|---:|
| VCF chr21, 1000 Genomes phase 3 | 11.23 GiB | 119.5× | 117.6× | **134.6×** |
| GTF | 4.45 GiB | 38.36× | 55.76× | **73.07×** |
| CSV AllNuclMetadata | 7.83 GiB | 36.76× | 43.00× | **59.77×** |
| Human FASTA | 64.20 GiB | 32.50× | 36.44× | **36.78×** |
| Human GFF3 | 1.65 GiB | 22.16× | 30.91× | **36.08×** |
| dbSNP chrY JSON | 13.13 GiB | 17.87× | 26.39× | **33.31×** |
| OrthoXML Compara 116 | 28.85 GiB | 20.55× | 22.80× | **29.06×** |
| GOA UniProt GPA | 71.88 GiB | 14.47× | 18.96× | **20.14×** |
| GOA UniProt GAF | 98.90 GiB | 12.92× | 16.25× | **18.63×** |
| SAM | 30.93 GiB | 5.79× | 6.79× | **7.92×** |
| FASTQ | 6.34 GiB | 3.87× | 4.20× | **4.48×** |

## Targeted-access observations

Across the campaign, targeted 4 MiB extraction remained below one second for the tested datasets and levels, with the observed range spanning **0.11 s to 0.75 s**.

Selected level-1 examples:

| Dataset | Source size | Targeted extraction |
|---|---:|---:|
| Human GFF3 | 1.65 GiB | 0.15–0.16 s |
| GTF | 4.45 GiB | 0.15–0.16 s |
| VCF chr21 | 11.23 GiB | **0.11 s** |
| VCF chr19 | 23.50 GiB | 0.177–0.201 s |
| SAM | 30.93 GiB | 0.27–0.29 s |
| Human FASTA | 64.20 GiB | 0.12–0.36 s |
| GOA UniProt GPA | 71.88 GiB | 0.162–0.206 s |
| GOA UniProt GAF | 98.90 GiB | 0.302–0.315 s |

## Full tables

- [Level 1 — throughput-oriented](GENOMICS-L1.md)
- [Level 6 — balanced](GENOMICS-L6.md)
- [Level 12 — maximum-density profile](GENOMICS-L12.md)

The measurements show that compression ratio depends strongly on the structure of the actual dataset, even when two datasets share the same nominal file format.
