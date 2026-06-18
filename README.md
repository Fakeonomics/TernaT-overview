# TernaT: CPU Neural Reasoner

[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)]() 

First learned neural reasoner over Vector-Symbolic Architecture. 90% exact multi-hop QA · <100 KB total · No LLM · No GPU · TernaT

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
    ┌─────────────────────────────────────┐
    │  Hybrid exact+VSA memory            │
    │  FastController (MLP, <1 KB)        │
    │  ChainScorer (Transformer, <1 KB)    │
    │  Beam search (width 1-3)             │
    │  Resonator (optional,               │
    │    use_resonator=False)              │
    └─────────────────────────────────────┘
                        ↓
                   Answer (entity)
```

---

## Component Sizes

| Component | Parameters | Size |
|-----------|:----------:|:----:|
| Controller + Scorer | ~41,000 | **<2 KB** |
| VSA Memory (96 facts) | D=1024 | ~44 KB |
| Resonator (optional) | 65,536 ternary | **16 KB** |

**Total: <62 KB** — fits on a microcontroller.

---

## Novel Contributions

1. **Learned controller over VSA memory** — Adaptive beam search replaces algorithmic VSA cleanup. Controller + exact store handles multi-hop reasoning without resonator dependency.
2. **Predicate-sharded VSA memory** — Facts indexed by predicate in separate shards, reducing superposition noise. Hybrid exact+VSA store breaks the ~80% accuracy ceiling.
3. **Negative training eliminates false positives** — Contrastive training drives FP rate from 1.3% to 0%.
4. **Multi-hop without LLM** — Entire pipeline runs on CPU with <100 KB total memory, no GPU required.

---

## Status

Proprietary technology — All Rights Reserved. Codebase is private.
For licensing inquiries or collaboration, contact via [GitHub](https://github.com/Fakeonomics).

*Created by Yuriy Venediktov, June 2026.*
