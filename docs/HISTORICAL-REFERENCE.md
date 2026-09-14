# Historical ZCaps reference benchmark

Zetako Compression Lab reconstructed an earlier ZCaps research generation as an executable lossless reference. It is maintained as a historical comparison point, not as the current product build.

The reconstructed reference completed a 50-file validation suite with exact SHA-256 round trips.

## Aggregate result

| Measurement | Result |
|---|---:|
| Original data | 315.15 MiB |
| Compressed data | 90.32 MiB |
| Compressed/original | **28.66%** |
| Space reduction | **71.34%** |
| Encode throughput | **2.12 MiB/s** |
| Decode throughput | **1.89 MiB/s** |
| Exact round trips | **50 / 50** |

Selected results:

| Corpus | Compressed/original | Encode | Decode |
|---|---:|---:|---:|
| ENWIK | **31.53%** | 2.08 MiB/s | 1.82 MiB/s |
| Silesia | **27.58%** | 2.15 MiB/s | 1.93 MiB/s |

This historical generation demonstrates a different density/compute trade-off from the current ZCaps line. It remains useful as a research baseline, while current product and benchmark claims are identified separately elsewhere in this repository.
