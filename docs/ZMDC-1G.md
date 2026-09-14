# ZCaps on ZMDC-1G v1

Frozen corpus: **1,000,037,807 primary bytes**, 13 primary files and 9 workload families. Benchmark system: **Apple M4 / macOS 26.6.2**. Exact lossless reconstruction was verified for every output.

## Aggregate comparison

| Configuration | Size | Ratio | Compression | Decompression |
|---|---:|---:|---:|---:|
| **ZCaps V12 -6** | **150.05 MB** | **6.66×** | 36.25 MB/s | 41.34 MB/s |
| xz -6 | 159.53 MB | 6.27× | 6.29 MB/s | 154.24 MB/s |
| bzip2 -6 | 165.08 MB | 6.06× | 19.65 MB/s | 78.61 MB/s |
| 7-Zip LZMA2 -5 | 168.27 MB | 5.94× | 6.29 MB/s | 230.60 MB/s |
| Brotli -6 | 174.09 MB | 5.74× | 131.02 MB/s | 882.86 MB/s |
| zstd -6 | 182.47 MB | 5.48× | 241.32 MB/s | 2142.66 MB/s |
| gzip -6 | 199.88 MB | 5.00× | 125.93 MB/s | 534.81 MB/s |
| LZ4 -6 | 237.03 MB | 4.22× | 166.72 MB/s | 3068.59 MB/s |

ZCaps produced the smallest aggregate output among the tested configurations.

## ZCaps by workload

| Family | ZCaps size | ZCaps ratio | Position |
|---|---:|---:|---:|
| API | 17.51 MB | 8.57× | **1st** |
| Structured binary | 12.03 MB | 6.24× | 2nd |
| Collaboration | 8.32 MB | 9.02× | **1st** |
| Database / CDC | 13.07 MB | 9.56× | **1st** |
| High-entropy control | 50.02 MB | 1.00× | 8th |
| Logs | 11.45 MB | 10.92× | **1st** |
| Observability | 12.97 MB | 11.57× | **1st** |
| Telemetry | 14.27 MB | 10.51× | **1st** |
| Transactions | 10.41 MB | 9.60× | **1st** |

ZCaps led **7 of 9** workload families. The high-entropy control remained essentially incompressible, as intended.

The ZMDC corpus was frozen before this comparison. Corpus identity and release assets are public in the [ZMDC repository](https://github.com/Zetako-Compression-Lab/zetako-modern-data-corpus).
