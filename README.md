# ZCaps

### Adaptive lossless statistical compression research by Zetako Compression Lab

> **ZCaps is a general-purpose lossless compression architecture based on adaptive contextual prediction and statistical entropy coding.**
>
> Instead of primarily searching for repeated byte sequences and replacing them with dictionary references, ZCaps continuously learns from the data stream, predicts likely upcoming symbols from context, and encodes prediction outcomes through an adaptive statistical model.

[![Lossless](https://img.shields.io/badge/compression-lossless-0A7D5A)](#lossless-by-design)
[![Research](https://img.shields.io/badge/status-active%20research-4B5DFF)](#research-status)
[![Implementation](https://img.shields.io/badge/core-proprietary-555555)](#public-research-private-implementation)

---

## Why ZCaps exists

Most widely deployed general-purpose compressors belong to families built around **dictionary matching**, **sequence matching**, or transformations designed to expose repeated patterns.

These approaches are extremely successful. DEFLATE, LZMA and Zstandard are examples of highly engineered codecs built on decades of research around repeated strings, match finding, dictionaries and entropy coding.

ZCaps explores a different question:

> **What if the compressor focuses less on finding a previous copy of a sequence, and more on continuously learning what is statistically likely to come next?**

That is the research direction behind ZCaps.

ZCaps is not intended as a reimplementation of gzip, Zstandard, Brotli, LZMA or another existing codec. It is an independent lossless compression architecture developed by Zetako Compression Lab.

---

## Classical dictionary compression vs. ZCaps

A simplified dictionary-based compressor often behaves conceptually like this:

```text
input bytes
    │
    ▼
search previous data
    │
    ├── repeated sequence found ──► encode match / distance / length
    │
    └── no useful match ──────────► encode literal
    │
    ▼
entropy coding
    │
    ▼
compressed stream
```

ZCaps takes a different route:

```text
input bytes
    │
    ▼
observe context
    │
    ▼
adaptive prediction
    │
    ▼
statistical confidence / model update
    │
    ▼
entropy coding
    │
    ▼
compressed stream
```

The distinction matters.

| | Traditional LZ / dictionary family | ZCaps research direction |
|---|---|---|
| Primary question | "Where did this sequence appear before?" | "Given this context, what is likely to appear next?" |
| Main representation | matches, distances, lengths, literals | prediction outcomes and literals |
| Learning | usually dominated by match history / dictionary state | continuously adaptive statistical state |
| Data view | repeated sequences | evolving byte-level context |
| Core research focus | match finding, parsing, dictionary efficiency | prediction quality, adaptation, entropy efficiency |
| Lossless | yes | yes |

This is intentionally a high-level comparison. Modern compressors are complex systems and often combine several techniques. The table describes the **primary architectural difference ZCaps is investigating**, not every mechanism used by every existing codec.

---

## High-level ZCaps architecture

The production implementation is proprietary, but its public conceptual model can be described in three stages.

### 1. Context observation

ZCaps derives compact state from recently observed data and uses that state to identify previously learned behaviour associated with similar contexts.

The goal is not simply to locate an earlier identical string. The goal is to estimate which symbol is most plausible next.

### 2. Adaptive statistical prediction

Prediction confidence evolves while the stream is processed.

The model is **online and adaptive**: encoder and decoder update equivalent state deterministically as symbols are processed. No external model or pre-trained dictionary is required for the standard codec.

### 3. Entropy coding

Prediction decisions and literal information are converted into the final compressed representation using statistical entropy coding.

Because encoder and decoder evolve the same model from the same history, the original byte stream can be reconstructed exactly.

```mermaid
flowchart LR
    A[Input bytes] --> B[Context observation]
    B --> C[Adaptive prediction]
    C --> D{Prediction outcome}
    D -->|likely symbol| E[Compact statistical event]
    D -->|new / unexpected symbol| F[Literal representation]
    E --> G[Entropy coder]
    F --> G
    G --> H[ZCaps compressed stream]

    H -. deterministic reconstruction .-> I[Lossless decode]
```

---

## Lossless by design

For ZCaps, "better compression" is never allowed to mean "approximately correct".

Every production candidate must preserve the source byte-for-byte.

Our benchmark workflow therefore treats compression ratio and throughput as secondary to the first requirement:

> **decode(encode(data)) == data**

Validation uses cryptographic hashes of the source and reconstructed output. Experimental variants that improve speed or size but fail exact round-trip validation are rejected from the production line.

---

## Research status

ZCaps is under active development.

The current validated implementation has been tested across well-known lossless-compression corpora including:

- Calgary Corpus
- Canterbury Corpus
- Canterbury Large
- Canterbury Artificial
- ENWIK
- Silesia Corpus

A recent validation pass covered **50/50 corpus files successfully**, with exact SHA-256 round trips.

### Current compression snapshot

The following figures are examples from the current research build. `Ratio` means compressed size divided by original size: lower is better.

| Corpus | Current ratio | Space reduction |
|---|---:|---:|
| Canterbury | 21.67% | 78.33% |
| Canterbury Large | 27.39% | 72.61% |
| Silesia | 29.40% | 70.60% |
| ENWIK | 30.44% | 69.56% |
| Calgary | 31.84% | 68.16% |

These values are research measurements, not universal promises. Compression behaviour varies substantially with input structure, entropy, file size, compiler, platform and measurement method.

### Current throughput snapshot

On an Apple Silicon M4 development system, a core-only benchmark of the current implementation measured approximately:

| Corpus | Encode | Decode |
|---|---:|---:|
| Silesia | **81.38 MiB/s** | **44.42 MiB/s** |
| ENWIK | **54.28 MiB/s** | **42.13 MiB/s** |

These are **raw compression-core measurements**, not end-to-end archive/application throughput. Integrity checks, container handling, storage and filesystem behaviour add additional cost in a complete product path.

---

## What we are researching now

ZCaps development is deliberately separated into different optimization goals rather than forcing every workload into one compromise.

### Balanced

The reference profile.

Research focus:

- preserve the current validated compression behaviour;
- reduce implementation overhead;
- improve compiler and architecture-specific code generation;
- reduce end-to-end container cost without changing compressed-data semantics.

### Fast

A throughput-oriented research profile.

The objective is to trade a small amount of compression density for lower latency, smaller working state and higher encode/decode throughput.

Early experiments indicate that this is a promising direction, particularly for applications where CPU time or decode latency matters more than the final few percent of size.

### Max

A compression-density research profile.

Instead of simply changing tuning constants, this work investigates richer predictive capacity while preserving deterministic lossless decoding.

Potential research areas include broader prediction sets, additional contextual signals and more expressive model selection. These experiments remain internal until they pass the same lossless validation requirements as the reference implementation.

---

## Why adaptive prediction is interesting

Dictionary compressors are excellent when the data contains reusable sequences that can be represented efficiently as references.

A predictive statistical model attacks redundancy from another angle.

Consider structured data where many symbols are not necessarily part of one long repeated phrase but are highly predictable from local context:

```text
{"type":"event","status":"ok","time":...}
{"type":"event","status":"ok","time":...}
{"type":"event","status":"error","time":...}
```

A dictionary codec can exploit repeated strings.

A contextual predictor can additionally learn that after a particular evolving context, some symbols are much more probable than others. The entropy coder can then assign very little information to highly expected outcomes and more information to surprising ones.

This principle is not unique to ZCaps; statistical and context-modeling compression has a long research history. What is proprietary to Zetako is the specific ZCaps architecture, state representation, adaptation strategy, implementation and optimization work used to make that approach practical as a general-purpose codec.

---

## Benchmark philosophy

Compression benchmarks are easy to make misleading.

For public ZCaps research we follow a few rules:

1. **Lossless validation comes first.** Every reported candidate must reconstruct the original exactly.
2. **Known corpora are preferred.** Public benchmark sets make results easier to reproduce and compare.
3. **Ratio and speed are reported together.** A codec that is smaller but dramatically slower is a different engineering choice, not automatically "better".
4. **Core and product measurements are separated.** Raw codec throughput must not be confused with archive/container, hashing or filesystem overhead.
5. **Failed experiments are useful research.** A fast experimental branch that fails exact reconstruction is considered a failed codec candidate, not a successful benchmark.
6. **Hardware and methodology matter.** Results should always identify the platform, compiler and benchmark scope.

---

## What ZCaps is not

ZCaps is **not**:

- a lossy compressor;
- a wrapper around gzip, Zstandard, Brotli or LZMA;
- a file-type-specific compressor;
- a pre-trained machine-learning model;
- a public-source implementation at this stage.

It is a native general-purpose lossless compression research project centered on adaptive statistical prediction.

---

## Research evolution

The project has evolved through multiple internal generations. Across validated generations, an important observation has been the stability of the fundamental compression model while implementation throughput improved.

That distinction guides current development:

```text
                 ZCaps research
                       │
          ┌────────────┴────────────┐
          │                         │
   model research             implementation research
          │                         │
 prediction quality          CPU / cache / I/O
 adaptation                  architecture-specific paths
 ratio profiles              product-envelope overhead
          │                         │
          └────────────┬────────────┘
                       │
                 validated codec
```

The goal is not to publish a sequence of version numbers for their own sake. Each public milestone should represent a measurable improvement in one of three dimensions:

**compression density · throughput · robustness**

without sacrificing exact reconstruction.

---

## Public research, private implementation

This repository is intentionally a **public research and benchmark record**.

It may contain:

- architecture explanations at a non-proprietary level;
- benchmark methodology;
- benchmark results;
- research notes;
- version/milestone history;
- reproducibility information;
- comparisons with established compression families.

It does **not** contain the production compression source code, proprietary state structures, internal tuning data or implementation details required to reproduce the ZCaps engine.

This allows Zetako Compression Lab to document the work transparently while preserving the intellectual property of the codec itself.

---

## Planned public sections

As the public research record grows, this repository will add dedicated material for:

- benchmark methodology and reproducibility;
- corpus-by-corpus results;
- evolution of validated ZCaps generations;
- Fast / Balanced / Max research profiles;
- comparisons with established general-purpose codecs;
- architecture and platform performance notes.

---

## About Zetako Compression Lab

ZCaps is developed by **Zetako Compression Lab**, part of Zetako's research into lossless compression architectures for general-purpose data, constrained systems and high-volume infrastructure.

Other Zetako compression research includes specialized work for blockchain data and embedded/constrained environments. ZCaps is the laboratory's general-purpose adaptive statistical compression line.

---

## Contact

**Zetako**  
Luxembourg  
contact@zetako.ai

Research project: **ZCaps — Adaptive Lossless Statistical Compressor**

---

<sub>Benchmark figures in this repository represent specific research runs and may change as the implementation, compiler toolchain and methodology evolve. All performance claims should be interpreted together with their benchmark environment and validation scope.</sub>
