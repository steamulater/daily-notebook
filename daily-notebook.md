# Daily Notebook

---

## 2026-06-13

### ESMFold2 — New Release Summary

Released **May 27, 2026** by Chan Zuckerberg Biohub. A major leap forward from the original ESMFold, directly competing with AlphaFold 3.

#### What was released (3 components)

| Component | What it is |
|---|---|
| **ESMC** | Foundation language model trained on 2.8 billion protein sequences from across all of life |
| **ESMFold2** | Structure prediction engine built on top of ESMC |
| **ESM Atlas** | Navigable atlas of 6.8 billion protein sequences + 1.1 billion predicted structures |

#### Key technical details

- **No MSA required** — unlike AlphaFold 2/3 which rely on multiple sequence alignments, ESMFold2 uses ESMC's learned sequence representations directly. Big deal because MSAs are slow and fail for sequences with few homologs.
- **World model approach** — ESMC trained unsupervised on massive sequence diversity, learning abstract evolutionary patterns rather than hand-crafted inductive biases.
- **Sparse Autoencoders (SAEs)** — used to extract interpretable biological features (e.g. membrane integration, disordered regions).
- **Inference-time scaling** — more compute = better predictions, validated across 5 therapeutic targets in cancer/immunology.

#### Performance

- Outperforms AlphaFold 3 on general protein-protein interactions and **antibody-antigen complexes**
- Particularly strong on antibodies — which have poor MSA coverage, making them hard for AlphaFold-based methods
- When given equivalent MSA data, beats AlphaFold 3 on standard benchmarks

#### Relevance — Antibody Discovery (OAS + Yeast Display)

ESMFold2 can be used to model library variants in complex with adenosine conjugates before synthesis, helping prioritize which sequences to actually build.

#### License

MIT — fully open source, no commercial use restrictions.

#### Sources

- [Biohub releases a world model of protein biology](https://biohub.org/news/world-model-of-protein-biology/)
- [ESMFold2: The Bitter Lesson is Coming for Proteins](https://www.latent.space/p/esmfold2)
- [Move over, AlphaFold: open-source model predicts shape of 1 billion proteins](https://www.nature.com/articles/d41586-026-01686-3)

---
