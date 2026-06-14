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

## 2026-06-14

### ESMFold2 & ESMC-6B — Local Installation

#### Installed

- `esm 3.3.0` via `pip install esm@git+https://github.com/Biohub/esm.git@c94ed8d`
- ESMFold2 weights pulled from `biohub/ESMFold2` (1.3 GB) — `~/.cache/huggingface/hub/models--biohub--ESMFold2/`
- ESMC-6B weights pulled from `biohub/ESMC-6B` (24 GB) — `~/.cache/huggingface/hub/models--biohub--ESMC-6B/`

#### Working inference setup (Mac Apple Silicon)

MPS does not work — `scatter_reduce` and Metal `GatherND` ops unsupported. CPU + float32 works:

```python
from transformers.models.esmfold2.modeling_esmfold2 import ESMFold2Model
from esm.models.esmfold2 import ESMFold2InputBuilder, ProteinInput, StructurePredictionInput
import torch

model = ESMFold2Model.from_pretrained("biohub/ESMFold2").float().cpu().eval()

spi = StructurePredictionInput(sequences=[ProteinInput(id="A", sequence="YOUR_SEQUENCE")])
result = ESMFold2InputBuilder().fold(model, spi, num_loops=1, num_sampling_steps=5, num_diffusion_samples=1, seed=42)

print(result.ptm, result.plddt)
```

#### Extracting ESMC-6B from ESMFold2

ESMC-6B (6.35B params) is embedded as `model._esmc` — no separate load needed:

```python
esmc = model._esmc  # ESMCModel, 6345M params
```

ESMC-6B weights stored separately, referenced via `"esmc_id": "biohub/ESMC-6B"` in ESMFold2's `config.json`.

#### AirDrop sharing

To share weights with another Mac:
- ESMFold2: zip `~/.cache/huggingface/hub/models--biohub--ESMFold2` (1.3 GB)
- ESMC-6B: zip `~/.cache/huggingface/hub/models--biohub--ESMC-6B` (24 GB)
- Recipient: unzip both into `~/.cache/huggingface/hub/`

---

### Tool Landscape — Protein Binder Design on Mac

| Tool | Runnable locally? | Blocker |
|---|---|---|
| **ESMFold2** | Yes (CPU, slow) | MPS unsupported |
| **BindCraft** | No | Linux + CUDA only, 32 GB VRAM needed |
| **BoltzGen** | Possibly | macOS supported; GPU fallback unclear; `pip install boltzgen` |

---

### Storage Audit

Disk at 98% capacity (862/926 GB used, 25 GB free).

| Location | Size | Notes |
|---|---|---|
| `~/Desktop/videos_for_edit` | 359 GB | 373 MP4s — already compressed, move to external drive |
| `~/Desktop/Screenshots` | 67 GB | Cleanup candidate |
| `~/Library/Application Support` | 54 GB | Check for old Xcode device support files |
| `~/.cache/huggingface` | 48 GB | ESMFold2 + ESMC-6B weights — in use |
| `~/Movies` | 44 GB | Move to external drive |
| `~/Desktop/adaptyv_competition` | 37 GB | Archive if done |
| `~/Desktop/screenplays` | 28 GB | Investigate — text shouldn't be this large |
| `~/Desktop/important_docs` | 20 GB | Move to iCloud |
| `~/Library/Caches` | 9.6 GB | Safe to clear |
| `~/anaconda3` | 9 GB | Prune unused conda envs |

Quick wins: videos + movies + Library/Caches = ~475 GB recoverable.

---
