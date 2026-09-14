# ZCaps broad corpus benchmark — 41.1 GiB

Campaign scope: **41.1 GiB**, **1,663 files**, **25 corpora**, three ZCaps levels. The data spans genomic files, JSON, structured text, DICOM, object meshes, text, binary data, audio and raw video. Exact lossless reconstruction was checked throughout the campaign.

## Global result

| Level | Compressed bytes | Ratio | Space reduction | Encode | Decode |
|---|---:|---:|---:|---:|---:|
| Default | 7,362,535,704 | 6.00× | 83.33% | 86.56 MiB/s | 64.63 MiB/s |
| 6 | 6,896,091,260 | 6.40× | 84.39% | 61.79 MiB/s | 55.45 MiB/s |
| 12 | 6,534,277,835 | **6.76×** | **85.20%** | 47.74 MiB/s | 34.44 MiB/s |

At level 12, **41.1 GiB was reduced to about 6.09 GiB**.

## Compression ratio by corpus

| Corpus | Default | Level 6 | Level 12 |
|---|---:|---:|---:|
| 1000 Genomes | 55.29× | 60.66× | **66.47×** |
| Artificial | 3.83× | 3.84× | 3.85× |
| Calgary | 3.40× | 3.56× | 3.64× |
| Calgary Large | 3.36× | 3.52× | 3.59× |
| Canterbury | 5.56× | 5.85× | 6.06× |
| Cellosaurus & DBpedia mappings | 16.18× | 20.07× | **24.91×** |
| DCM aggregate | 2.83× | 2.86× | 2.96× |
| DCM series 000001 | 2.76× | 2.74× | 2.85× |
| DCM series 000002 | 2.42× | 2.42× | 2.49× |
| DCM series 000003 | 2.80× | 2.78× | 2.88× |
| DCM series 000004 | 2.84× | 2.83× | 2.93× |
| DCM series 000005 | 2.11× | 2.11× | 2.14× |
| JSON | 14.09× | 16.91× | **21.16×** |
| Large text / reference data | 3.79× | 4.20× | 4.30× |
| Lukas medical images | 3.17× | 3.22× | 3.33× |
| Miscellaneous | 2.17× | 2.33× | 2.33× |
| OBJ meshes | 6.71× | 6.98× | **8.17×** |
| Other / control datasets | 2.02× | 2.04× | 2.06× |
| Pickles | 4.02× | 4.17× | 4.39× |
| Pizza & Chili | 3.27× | 3.64× | 3.86× |
| Protein | 1.84× | 1.90× | 1.91× |
| Silesia | 3.72× | 3.89× | 4.14× |
| Windows system files | 2.28× | 2.27× | 2.32× |
| WAV | 1.25× | 1.25× | 1.26× |
| YUV / Y4M | 1.93× | 1.98× | 2.04× |

## Selected individual observations

| Data | Result |
|---|---:|
| 1000 Genomes VCF, ~11.23 GB file | ~99.18% space reduction at the default level |
| 1 GB formatted JSON | >92% space reduction at the default level |
| DBpedia mapping TTL | ~94.9% space reduction at the default level |
| 1 GB random control | approximately 1:1 |
| 1 GB zero-filled control | ~99.998% space reduction |
| WAV corpus | ~20% aggregate reduction |

The campaign demonstrates that ZCaps behavior varies strongly with the structure of the data rather than simply with the file extension.
