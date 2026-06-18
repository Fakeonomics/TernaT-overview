[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20752580.svg)](https://doi.org/10.5281/zenodo.20752580) [![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)]() [![TernaT Site](https://img.shields.io/badge/TernaT-Live%20Site-228b22?style=for-the-badge)](https://fakeonomics.github.io/TernaT-overview/) [![GraphKAN](https://img.shields.io/badge/GraphKAN-Overview-blue?style=for-the-badge)](https://fakeonomics.github.io/graphkan-overview/)

**TernaT Live Site** · **GraphKAN Live Site**

---

# TernaT: CPU Neural Reasoner

First learned neural reasoner over Vector-Symbolic Architecture with **discrete ternary weights** {-1, 0, +1}.  
**90% exact multi-hop QA · 16 KB ternary resonator · <100 KB total · No LLM · No GPU**

## Overview

TernaT introduces a **learned neural pipeline** for multi-hop question answering over Vector-Symbolic Architecture (VSA) memory. Unlike prior algorithmic VSA work (Kanerva 1988, Plate 2003, Frady 2021), TernaT **learns** to reason — all components are trained end-to-end, achieving **90% exact match** on a 96-fact, 30-query benchmark:

| Method | Overall | 1-hop | 2-hop | 3-hop |
|--------|:-------:|:-----:|:-----:|:-----:|
| VSA direct query | 30% | 90% | 0% | 0% |
| Resonator only | 30% | 90% | 0% | 0% |
| Controller + Resonator | 73% | 80% | 60% | 80% |
| **TernaT (full pipeline)** | **90%** | **100%** | **100%** | **70%** |

96 facts, 53 entities, 30 queries. All learned components — no hardcoded rules, no LLM.

## Novel Contributions

1. **Learned neural VSA resonator** — First learned cleanup for VSA queries. Ternary weights: 16 KB, 1.1ms inference. Replaces O(n) brute-force (77s → 1.1ms).
2. **Negative training eliminates false positives** — Contrastive training drives FP rate from 1.3% to 0% without losing recall.
3. **Multi-hop without LLM** — Controller + ChainScorer + beam search achieve 90% exact match — first VSA system with learned multi-hop reasoning.
4. **VSA superposition noise solved** — ~80% ceiling proven independent of dimension. Hybrid exact+VSA store breaks the ceiling: 80% → 90%.

## Architecture

```
Question (NL) → NL Parser → (entity, predicate)
                                  ↓
                    ┌─ PredicateShardedStore (VSA memory, D=1024)
                    │   Facts sharded by predicate
                    │   Hybrid exact dict + VSA fallback
                    │
                    ├─ GraphKANResonator (16 KB, 65K ternary params)
                    │   Learned query cleanup, negative training
                    │
                    ├─ FastController (<1 KB, 37K float params)
                    │   BC-trained: (entity, goal) → next predicate
                    │
                    └─ ChainScorer (<1 KB, Transformer 2L/4H/128d)
                          Beam search (width 1-3) path scoring
                                  ↓
                           Answer (entity)
```

## Component Sizes

| Component | Parameters | Size | Deployment |
|-----------|:----------:|:----:|:----------:|
| GraphKANResonator | 65,536 ternary | **16 KB** | Any MCU |
| FastController | 37,384 float | **<1 KB** | Any MCU |
| ChainScorer | ~50,000 float | **<1 KB** | Any MCU |
| VSA Memory (96 facts) | D=1024 | ~44 KB | Cortex-M4+ |
| **Total pipeline** | | **<100 KB** | **$0.50 MCUs** |

## Training

4-phase QAT pipeline (same as GraphKAN):

```
Phase 1: Float clamp to range
Phase 2: Gradual ternarization (STE)
Phase 3: Hard clamp to {-1, 0, +1}
Phase 4: Finetune with gamma absorption
```

Controller trained via behavioral cloning on BFS-generated data.  
Negative training with contrastive examples for resonator precision.

## Comparison with Prior Art

| Method | Learned? | Ternary? | Multi-hop? | Year |
|--------|:--------:|:--------:|:----------:|:----:|
| Kanerva (VSA) | ✗ | ✗ | ✗ | 1988 |
| Plate (HRR) | ✗ | ✗ | ✗ | 2003 |
| Frady (Resonators) | ✗ | ✗ | ✗ | 2021 |
| **TernaT (ours)** | **✓** | **✓** | **✓** | **2026** |

First learned neural VSA reasoner — all prior work is algorithmic, not learned.

## Hardware Targets

| Target | Memory | Suitability |
|--------|:------:|:-----------:|
| Cortex-M0+ ($0.50 MCU) | 16 KB L1 | ✅ Resonator fits |
| ESP32-S3 | 512 KB SRAM | ✅ Full pipeline |
| RISC-V GD32V | 32 KB | ✅ Core components |
| Smartwatch DSP | 128 KB | ✅ Full pipeline |

## Status

Proprietary technology — All Rights Reserved.  
Codebase is private. [Full paper](https://fakeonomics.github.io/TernaT-overview/) available.  
For licensing inquiries, contact via GitHub.

*Created by Yuriy Venediktov, June 2026.*
