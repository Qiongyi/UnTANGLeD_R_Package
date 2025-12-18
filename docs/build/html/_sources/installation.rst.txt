Installation
============

This section covers the installation and setup of the UnTANGLeD R package. The package requires several R dependencies for clustering analysis, data manipulation, and visualization.

.. _installation:

System Requirements
-------------------

- R version >= 4.0.0


R Dependencies
--------------

Install the required R packages from CRAN:

.. code-block:: R

    # Core dependencies for clustering and matrix operations
    install.packages("Seurat")
    install.packages("cluster")
    install.packages("dplyr")
    install.packages("data.table")
    install.packages("tidyr")
    install.packages("Matrix")

    
    # Network analysis functions required for conservation analyses
    install.packages("DirectedClustering")
    install.packages("RcppAlgos")
    
    # Parallel processing
    install.packages("parallel")
    install.packages("doParallel")
    

Bioconductor Dependencies
-------------------------

Some functions may require Bioconductor packages. Install Bioconductor if not already available:

.. code-block:: R

    if (!require("BiocManager", quietly = TRUE))
        install.packages("BiocManager")
    
    # Install any required Bioconductor packages
    BiocManager::install("your_bioc_package_name")


Installing UnTANGLeD R Package
------------------------------

1. Download the UnTANGLeD R package source file (e.g., untangled_1.0.0.tar.gz) from the repository

2. Install the package from the source file:

.. code-block:: R

    install.packages("path/to/untangled_1.0.0.tar.gz", repos = NULL, type = "source")
    library(untangled)


Data Preparation Requirements
-----------------------------

Before using UnTANGLeD, ensure your data meets these requirements:

**For General High-dimensional Data:**

- Numeric data matrix with features as rows and samples as columns stored as a Seurat Object
- Data should be normalized and scaled appropriately
- Missing values handled (removal or imputation)

**Memory Considerations:**

UnTANGLeD builds consensus matrices that scale quadratically with the number of samples. For large datasets:

- Ensure adequate RAM (consensus matrix for 5,000 samples requires ~1GB)
- Use appropriate ``max_clusters`` parameter to limit computational burden