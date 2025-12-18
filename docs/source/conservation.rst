Conservation Analysis
=====================

Description
-----------

The conservation analysis functions in UnTANGLeD evaluates clustering results across different datasets, conditions, or biological contexts. It assesses whether clusters identified in one dataset are preserved in another, identifying reproducible patterns and clusters for further investigation



gcc - Global Clustering Coefficient Analysis
---------------------------------------------

**Description:**
``gcc`` calculates the global clustering coefficient to measure the degree to which samples from query cluster also cluster together in the reference dataset. For each cluster in reference dataset, gcc identifies all possible combinations of triplets and count how many of these triplets remain in the query cluster. The conservation score is the ratio of preserved triplets to total possible triplets. Bootstrapping is used to determine statistical significance on whether observed clustering patterns exceed random expectations.



**Parameters:**

- ``query``: A string path to an RDS file containing subgroup assignments derived from UnTANGLeD analysis. This serves as the query clustering to be evaluated for conservation.

..

- ``reference``: A string path to an RDS file containing reference cluster assignments, also derived from UnTANGLeD analysis. This provides the reference clustering framework for comparison.

..

- ``output``: (Optional) A character string specifying the output file path. Default is ``"GCC_results.txt"``.

**Returns:**
Data frame with comprehensive GCC statistics:
- ``cluster``: Cluster identifier from query dataset
- ``nconditions``: The number of conditions or samples within each cluster.
- ``topclus``: The cluster in reference with the highest similarity to the current cluster in query. This identifies the closest matching cluster in the reference framework, indicating the degree of conservation or divergence between query and reference.
- ``gcc``: The global clustering coefficient for the cluster (0 to 1).
- ``gcc_av``: The average GCC obtained from bootstrap samples.
- ``gcc_sd``: The standard deviation of the GCC from bootstrap samples.
- ``zscore``: The z-score evaluating the deviation of observed GCC from bootstrap expectations.
- ``pval``: The p-value assessing the significance of the observed GCC compared to bootstrap samples.

**Usage Example:**

.. code-block:: R

    # Assess conservation of cell line A clusters in cell line S
    gcc_results <- gcc(
        query = "Dataset1_UnTANGLeD_Output/_subgrp.rds",
        reference = "Dataset2_UnTANGLeD_Output/dataset2_subgrp.rds",
        output = "conservation_Dataset1_in_Dataset2.txt"
    )
    

    


cmc - Consensus Matrix Conservation
-----------------------------------

**Description:**
``cmc`` (Consensus Matrix Conservation) directly compares cluster assignments against the consensus matrix of another dataset. This method assesses whether samples that cluster together in one dataset show high consensus values in another dataset's consensus matrix. Bootstrapping is used to determine statistical significance on whether observed clustering patterns exceed random expectations.

**Parameters:**

- ``query``: Path to RDS file containing the consensus matrix from the query dataset

..

- ``reference``: Path to RDS file containing reference cluster assignments

..

- ``output``: (Optional) Output file path. Default is ``"CMC_results.txt"``

**Returns:**
Conservation scores for each cluster:
- ``cluster``: Cluster identifier.
- ``score``: Mean conservation score within the cluster.
- ``expected``: Expected score from bootstrap analysis.
- ``exp_sd``: Standard deviation of expected scores.
- ``zscore``: Standardized conservation score.
- ``pval``: Statistical significance of conservation.

**Usage Example:**

.. code-block:: R

    # Consensus matrix conservation analysis
    cmc_results <- cmc(
        query = "dataset1/consensus_matrix.rds",
        reference = "dataset2/gene_subgrp.rds",
        output = "consensus_conservation.txt"
    )
    



