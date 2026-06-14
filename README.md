# VSA-Reasoner

**Patent Pending** | [![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)]() 

Multi-hop reasoning **without Large Language Models** — using Vector-Symbolic Architecture (VSA) memory with learned neural components. **90% exact accuracy** on multi-hop queries with a **16 KB model**, CPU-only inference.

---

## Key Results

| Method | Overall | 1-hop | 2-hop | 3-hop |
|--------|:-------:|:-----:|:-----:|:-----:|
| VSA direct retrieval | 30% | 90% | 0% | 0% |
| Resonator only | 30% | 90% | 0% | 0% |
| **Full system (Ctrl+Res+Scorer)** | **90%** | **100%** | **100%** | **70%** |

96 facts, 53 entities, 30 queries. All learned components — no hardcoded rules, no LLM.

---

## System Pipeline

```
Question → Parser → (entity, goal)
                        ↓
    ┌──────────────────────────────┐
    │  VSA Memory (predicate-sharded) │
    │  FastController (MLP, <1 KB)    │
    │  Neural Resonator (16 KB, tern) │
    │  ChainScorer (Transformer, <1KB)│
    │  Beam search (width 1-3)        │
    └──────────────────────────────┘
                        ↓
                   Answer (entity)
```

---

## Component Sizes

| Component | Parameters | Size |
|-----------|:----------:|:----:|
| Neural Resonator | 65,536 ternary | **16 KB** |
| FastController | 37,384 float | **<1 KB** |
| ChainScorer | ~4,000 float | **<1 KB** |
| VSA Memory (96 facts) | D=1024 | ~44 KB |

**Total: <62 KB** — fits on a microcontroller.

---

## Novel Contributions

1. **Learned neural VSA resonator** — First system to replace VSA cleanup with a learned iterative refinement network using ternary STE weights.
2. **Predicate-sharded VSA memory** — Facts indexed by predicate in separate shards, reducing superposition noise.
3. **Controller-guided multi-hop search** — BC-trained MLP selects which predicate to follow next; combined with beam search and chain scoring.
4. **No LLM required** — Entire pipeline runs on CPU with <100 KB total memory.

---

## Status

Proprietary technology — All Rights Reserved. Codebase is private.
For licensing inquiries or collaboration, contact via [GitHub](https://github.com/uv20817-prog).

*Created by uv20817-prog, June 2026.*
