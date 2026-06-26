# Unsupervised Signature Learning & Spatial Mapping of Astrocytes

<p align="left">
  <img src="https://img.shields.io/badge/Paradigm-Unsupervised%20Representation%20Learning-2E7D32?style=flat-square">
  <img src="https://img.shields.io/badge/Output-Whole--Section%20Spatial%20Atlas-1565C0?style=flat-square">
  <img src="https://img.shields.io/badge/Scale-300k%20%C3%97%20200k%20px-EF6C00?style=flat-square">
  <img src="https://img.shields.io/badge/Labels-Zero-7B1FA2?style=flat-square">
  <img src="https://img.shields.io/badge/code-private%20(institutional%20IP)-555?style=flat-square">
</p>

> A label-free pipeline that turns raw cell detections into a **spatial map of cell-morphology types across an entire brain section** — built at the **Sudha Gopalakrishnan Brain Centre, IIT Madras**. This repo documents the method and results; source is held under institutional IP.

---

## What this is (in one line)

Given hundreds of thousands of cells with no labels, it **discovers the distinct morphology types automatically** and **places every cell back on the tissue** — producing an interpretable, queryable atlas instead of an unlabeled pile of crops.

## Why a healthcare/ML team should care

- **Zero manual labels.** Phenotyping is normally the cost bottleneck. This learns the cell types directly from shape, so it scales to a **new brain or a new dataset with no re-annotation** — the difference between a one-off study and a pipeline.
- **It produces a *product*, not a plot.** The output is a spatial atlas: each cell tagged with a learned morphotype and its true tissue coordinate — exactly the substrate for downstream **biomarkers, disease/region association, and population-scale phenotyping**.
- **Interpretable by construction.** Cells are described by human-readable shape descriptors (not a black-box embedding), so a clinician or reviewer can see *why* a cell landed in a cluster.
- **Validated by the data itself** — the learned clusters are **spatially organized**, not random noise (see the atlas below), which is strong evidence the signatures capture real structure.

## Spatial morphology atlas

![GFAP morphology clusters across a whole brain section](./assets/GFAP_Cluster_TRS_color_changed.png)

<sub>Every detected astrocyte plotted at its true tissue coordinate (section spanning ~320,000 × 200,000 px), colored by its learned morphology cluster. The clusters form coherent spatial structures that track tissue architecture — the unsupervised signatures line up with real anatomy.</sub>

## How it works

![Pipeline](./assets/architecture.svg)

1. **Cell crops** come from the detection/segmentation stage ([sibling project](https://github.com/roshankumarvam-create/astrocyte-detection-segmentation)).
2. **Morphometric feature vector** — each cell is reduced to interpretable shape descriptors: soma area, branch count, total process length, branching complexity, convex-hull ratios, symmetry.
3. **Embedding** — features are projected with **UMAP** to expose structure in low dimensions.
4. **Unsupervised clustering** groups cells into morphology types with **no labels supplied**.
5. **Spatial mapping** — every cell's cluster is written back to its tissue coordinate to build the whole-section atlas above.

## Results

- **Four** distinct morphology clusters recovered fully unsupervised, with clear **spatial coherence** across the section.
- Clusters correspond to recognizable, interpretable shape families:

| Morphotype | Character |
|---|---|
| Stellate | star-shaped, many fine processes |
| Bipolar (simple) | elongated, two dominant processes |
| Bushy / complex | dense, highly branched |
| Reactive / hypertrophic | enlarged soma, thickened processes |

<sub>Per-cluster cell counts to be finalized.</sub>

## Tech stack

`scikit-image` · `NumPy / pandas` · `UMAP` · `scikit-learn` · `matplotlib`

## Why it matters

This is the **representation-learning + analytics layer** of an end-to-end cell-analysis program: it converts raw model outputs into a structured, spatial, label-free phenotype map that other systems can build on. The same pattern — *detect → describe → cluster → map* — generalizes to any tissue and any cell type, which is what makes it a reusable platform rather than a single experiment.

---

<sub>Code is private under IIT Madras / SGBC institutional agreements. A technical walkthrough is available on request.</sub>
