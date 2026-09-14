# ZCaps parallel workload scaling and resource results

This page reports the public resource and scaling measurements collected alongside the ZMDC-1G campaign. The current ZCaps V12 build used for ZMDC is a single-process codec; the scaling result below measures **multiple independent files processed concurrently**, not internal multi-thread acceleration of one file.

## Batch scaling on ZMDC-1G

Dataset: 13 primary files, **1,000,037,807 bytes** total.

| Concurrent ZCaps processes | Ratio | Compression | Decompression | CPU comp. s/GB | CPU decomp. s/GB | Peak group RAM comp. | Peak group RAM decomp. |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | 6.66× | 35.79 MB/s | 41.03 MB/s | 27.60 | 23.91 | 43.83 MiB | 23.73 MiB |
| 4 | 6.66× | **113.25 MB/s** | **110.22 MB/s** | 28.52 | 28.92 | 167.39 MiB | 94.95 MiB |

Measured median batch speed-up, four processes versus one:

- **3.16× compression throughput**
- **2.69× decompression throughput**

The compression ratio stayed unchanged because each file was processed independently with the same codec settings.

## CPU and memory snapshot — API workload

The 150 MB ZMDC API workload was also used to compare compression density with CPU and memory cost.

| Configuration | Ratio | Compression CPU s/GB | Peak RAM compression |
|---|---:|---:|---:|
| **ZCaps** | **8.57×** | **26.24** | **43.8 MiB** |
| bzip2 | 7.85× | 50.68 | 5.8 MiB |
| xz | 7.61× | 173.50 | 91.6 MiB |
| 7-Zip LZMA2 | 7.15× | 176.76 | 371.6 MiB |
| Brotli | 6.72× | 8.20 | 182.8 MiB |
| zstd | 6.39× | 4.78 | 8.0 MiB |

On this workload, ZCaps combined the highest compression ratio in the table with substantially lower compression CPU cost than xz and LZMA2. zstd and Brotli remained significantly faster choices when throughput is prioritized over compression density.

## Decode performance

Decode remains a distinct trade-off in the current ZCaps build. The ZMDC aggregate result measured approximately **41 MB/s** single-process decode throughput, substantially below zstd and Brotli. This is therefore an active optimization area rather than a claim of universal speed leadership.

All figures above are measurements of the stated benchmark campaigns and should be read as workload- and platform-specific results.
