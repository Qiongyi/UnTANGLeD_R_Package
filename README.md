# UnTANGLeD R Package

## Overview
**UnTANGLeD** is an unsupervised consensus clustering and prioritisation pipeline designed for the analysis of high-dimensional biological data. It enables robust identification of stable clusters across multiple resolutions and provides quantitative metrics to evaluate clustering quality and biological conservation.

UnTANGLeD is suitable for researchers working on:
- Unsupervised clustering of high-dimensional biological datasets
- Identification of robust and reproducible cluster structures
- Evaluation of cluster stability, quality, and biological relevance
- Comparative analysis of clustering patterns across biological contexts

---


## Workflow

```text
Seurat object (normalized and scaled)
        |
        v
1. clusterOptimiser()
  - run Seurat clustering across a range of resolutions
  - build co-clustering consensus matrix
  - silhouette scan + plateau detection -> optimal k
        |
        v
2. untangled(optimal_clusters = k)
  - hierarchical clustering on consensus-derived distance
  - final cluster assignment + QC summary (silhouette, within-cluster correlation)
        |
        v
3. Conservation across contexts (optional)
  - cmc(): consensus matrix conservation
  - gcc(): global clustering coefficient
```

---


## Quick Start
```r
# 1) Optimise clustering and get recommended k
clusterOptimiser(seu = seu, outdir = "UnTANGLeD_output")

# 2) Run clustering using the recommended k (replace with your detected value of the optimal clusters)
untangled(optimal_clusters = 150, outdir = "UnTANGLeD_output")

# 3) Optional: conservation analyses across contexts
# Using the cmc method
cmc_results <- cmc(query = "path/to/query_consensus_matrix.rds", reference = "path/to/reference_sub_grp.rds")
# Using the gcc method
gcc_results <- gcc(query = "path/to/query_sub_grp.rds", reference = "path/to/reference_sub_grp.rds")
```

---


## Download the Latest Release
You can download the latest release of UnTANGLeD [here](https://github.com/Qiongyi/UnTANGLeD_R_Package/releases/latest/download/untangled_1.0.0.tar.gz).

## Documentation
The complete UnTANGLeD R Package manual is available via [readthedocs](https://untangled-r-package.readthedocs.io/en/latest/).

## Citing UnTANGLeD R Package
If you use or discuss the UnTANGLeD R Package in your research, please cite the corresponding publication:

***Coming soon***

