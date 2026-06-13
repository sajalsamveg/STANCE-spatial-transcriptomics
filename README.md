# STANCE: Spatial Transcriptomics Analysis via Nuclear Norm Completion

Graph-regularised matrix completion framework for recovering missing gene 
expression in 10x Genomics Visium spatial transcriptomics data.


---

## The problem

10x Visium capture spots have ~10–30% mRNA capture efficiency — a large 
fraction of expressed genes register as zero. Standard imputation (mean fill, 
k-NN) ignores two key structural facts: expression matrices are approximately 
low-rank, and spatially adjacent spots share gene regulation programs.

## What STANCE does

Solves a single strictly convex objective with three terms:

| Term | Effect |
|------|--------|
| Frobenius fidelity | Stay faithful to observed measurements |
| Nuclear norm (α = 2.5) | Enforce global low-rank biological structure |
| Graph Laplacian (β = 0.6) | Enforce spatial smoothness across adjacent spots |

Optimised by **FISTA** (O(1/k²) convergence) with **Singular Value 
Thresholding** as the proximal operator. Graph built from a k=6 nearest-neighbour 
graph on spot coordinates, matching Visium's hexagonal geometry.

## Results (10x Genomics Visium Mouse Brain, 30% held-out masking)

| Model | RMSE | Pearson r |
|-------|------|-----------|
| FIST (nuclear norm only, β=0) | 0.350 | 0.604 |
| **STANCE (+ Graph Laplacian)** | **0.239** | **0.795** |

## How to run

**Option A — Colab (recommended, no setup)**  
Open the notebook directly: [stance_edges.ipynb on Colab](https://colab.research.google.com/drive/1Nj5hxrTq3sDp38iSdCsPGc-XY_-i2GVa?usp=sharing)

**Option B — Local**

```bash
pip install -r requirements.txt
jupyter notebook stance_edges.ipynb
```

The notebook auto-downloads the Visium dataset via Squidpy. 
Synthetic experiment: ~30 seconds. Real Visium experiment: 2–5 minutes on CPU.

## Repo structure

STANCE-spatial-transcriptomics/
├── stance_edges.ipynb   
├── requirements.txt
└── README.md

## References

- Beck & Teboulle (2009) — FISTA
- Candès & Recht (2009) — Matrix completion via convex optimisation  
- Palla et al. (2022) — Squidpy
- Wolf et al. (2018) — Scanpy
