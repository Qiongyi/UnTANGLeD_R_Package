clusterOptimiser
================

Description
-----------

``clusterOptimiser`` performs multi-resolution consensus clustering across a range of Seurat clustering granuarities. It constructs a consensus matrix from clustering assignments, automatically detects the optimal cluster number using plateau detection in silhouette analysis, and provides visualization tools to aid in cluster selection.


Methodology
-----------

``clusterOptimiser`` implements the following workflow:

1. **Multi-resolution Clustering:** Performs Seurat clustering across a user-defined range of resolutions (default 100 iterations: 0.2 to 20.0 in increments of 0.2)

2. **Consensus Matrix Construction:** For each pair of samples, calculates the frequency of co-clustering across all resolutions to build a consensus similarity matrix

3. **Silhouette Analysis:** Applies hierarchical clustering to the consensus matrix using various cluster numbers (2 to max_clusters) and computes average silhouette scores for each cluster number

4. **Automatic Plateau Detection:** Identifies the optimal cluster number by detecting when silhouette score improvements plateau using a rolling mean approach with configurable threshold

5. **Visualization:** Generates plots showing silhouette scores vs. cluster numbers with the detected optimal cluster number highlighted

Input and Output
----------------

**Input:** 
  - A preprocessed Seurat object containing normalized and scaled gene expression data
  - The Seurat object should have completed standard preprocessing (normalization, scaling, PCA)

**Output:** 
  The function generates several output files in the specified directory:
  
  - ``seurat_scaled_matrix.rds``: Saved Seurat object for downstream analysis
  - ``metadata_cluster_assignments.txt``: Table containing cluster assignments across all tested resolutions
  - ``consensus_matrix.rds``: Consensus matrix representing co-clustering frequencies
  - ``average_silhouette_scores.txt``: Silhouette scores for each tested cluster number
  - ``optimal_cluster_number.txt``: Automatically detected optimal cluster number
  - ``average_silhouette_scores_by_cluster_number.pdf``: Visualization plot with optimal cluster number highlighted

Parameters
----------

- ``seu``: A Seurat object containing normalized gene expression data and preprocessed through standard Seurat workflows (scaling, PCA). **Required**.

..

- ``outdir``: (Optional) A character string specifying the directory path where output files will be saved. The function will create the directory if it does not exist. Default is ``"UnTANGLeD_output"``.

..

- ``resolution``: (Optional) A numeric vector specifying the range of Seurat clustering resolutions to evaluate. The function iterates through these resolutions for consensus clustering analysis. Default is ``seq(0.2, 20, 0.2)``.

..

- ``algorithm``: (Optional) An integer from 1 to 4 specifying which Seurat modularity optimisation algorithm to run for clustering (1 = original Louvain algorithm; 2 = Louvain algorithm with multilevel refinement; 3 = SLM algorithm; 4 = Leiden algorithm). Default is ``1``.

..

- ``max_clusters``: (Optional) An integer specifying the maximum number of clusters to evaluate during consensus matrix optimization. This sets the upper limit for silhouette analysis. Default is ``300`` or the number of samples in the dataset, whichever is smaller.

..

- ``thresh_frac``: (Optional) A numeric value between 0 and 1 specifying the threshold fraction for plateau detection. This parameter defines how strict the plateau definition is by setting the cutoff as a fraction of the maximum rolling gain. For example, a threshold of 0.1 means the optimal cluster number is identified when the silhouette score no longer increases by more than 10% of the mean silhouette increase over the last three consecutive cluster numbers. Default is ``0.01``.


Optimal Cluster Selection
-------------------------

The ``clusterOptimiser`` function now **automatically detects** the optimal cluster number using plateau detection:

**Automatic Detection:**
- Calculates the rate of silhouette score improvement using a rolling mean (window size = 3)
- Identifies the plateau point where improvement falls below a threshold (default: 1% of maximum gain)
- Saves the detected optimal cluster number to ``optimal_cluster_number.txt``
- Highlights the optimal point with a red dashed line in the visualization plot

**Manual Review:**
While the automatic detection provides a data-driven recommendation, users should:
- Examine the silhouette plot (``average_silhouette_scores_by_cluster_number.pdf``)
- Verify the detected optimal cluster number aligns with biological expectations
- Consider adjusting ``thresh_frac`` if the detection seems too conservative or aggressive
- Select alternative cluster numbers if domain knowledge suggests otherwise

**Interpretation:**
- **X-axis:** Number of clusters (2 to max_clusters)
- **Y-axis:** Average silhouette score (0 to 1)
- **Red dashed line:** Automatically detected optimal cluster number
- **Selection criteria:** Balance between silhouette score and biological interpretability



Usage Examples
--------------

**Example 1: Basic Usage with Default Parameters**

.. code-block:: R

    # Assuming 'seu' is your preprocessed Seurat object
    clusterOptimiser(seu = seu,
                     outdir = "UnTANGLeD_output")
    
    # The function will automatically detect optimal cluster number
    # and display the recommended next step command

**Example 2: Custom Resolution Range**

.. code-block:: R

    # Use finer resolution increments for detailed analysis
    clusterOptimiser(seu = seu,
                     outdir = "my_analysis",
                     resolution = seq(0.1, 10, 0.1))

**Example 3: Limited Cluster Range for Smaller Datasets**

.. code-block:: R

    # Maximum number of clusters should not exceed the number of objects in the dataset
    clusterOptimiser(seu = seu,
                     outdir = "my_analysis",
                     resolution = seq(0.5, 15, 0.1),
                     max_clusters = 150)

**Example 4: Leiden Algorithm with Custom Plateau Threshold**

.. code-block:: R

    # Use Leiden algorithm with more stringent plateau detection
    clusterOptimiser(seu = seu,
                     outdir = "my_analysis",
                     algorithm = 4,
                     thresh_frac = 0.1)



Considerations and Troubleshooting
----------------------------------

**Computational Time:**
- Resolution range: more resolutions = longer runtime
- max_clusters: higher values = Extended silhouette computation
- Typical runtime: seconds to over 10 minutes for larger datasets

**Optimization Tips:**
1. Start with default parameters for initial exploration
2. Adjust resolultion when needed. Higher resolution means finer clustering granularity. 
3. Set reasonable max_clusters based on expected biological diversity (automatically capped at sample size)
4. Adjust ``thresh_frac`` if automatic detection seems too conservative (increase) or aggressive (decrease)

**Common Issues:**

1. **Long runtime:** Use coarser resolution range or fewer resolution iterations
2. **Flat silhouette plot:** Data may require additional preprocessing or different resolution range
3. **Missing Seurat object components:** Ensure necessary scaling and PCA are completed. 

**Plateau Detection Adjustment:**
- If optimal cluster number seems **too low**: decrease ``thresh_frac`` (e.g., from 0.01 to 0.005)
- If optimal cluster number seems **too high**: increase ``thresh_frac`` (e.g., from 0.01 to 0.1)
- Review the silhouette plot to verify the detection makes sense visually

**Quality Control:**
- Check that silhouette plot shows steady increase before plateau
- Verify the detected optimal cluster number is marked clearly on the plot
- Verify consensus matrix contains reasonable co-clustering patterns
- Ensure all output files are generated successfully

Next Steps
----------

After running ``clusterOptimiser``:

1. **Review the console output** which displays:
   - The automatically detected optimal cluster number
   - A recommended command for the next step with the detected optimal_clusters value

2. **Examine the silhouette plot** (``average_silhouette_scores_by_cluster_number.pdf``) to:
   - Verify the automatically detected optimal cluster number (marked with red dashed line)
   - Check if the plateau detection aligns with visual inspection
   - Consider alternative cluster numbers if manual inspection suggests otherwise

3. **Proceed to main clustering analysis** using the recommended command:

.. code-block:: R

    # Example output message:
    # =====================================
    # Recommended next step:
    # untangled(optimal_clusters = 115, outdir = "UnTANGLeD_output")
    # =====================================
    
    # Copy and run the recommended command
    untangled(optimal_clusters = 115, outdir = "UnTANGLeD_output")


The ``clusterOptimiser`` function provides the foundation of UnTANGLeD analyses by establishing the consensus matrix, automatically detecting the optimal cluster number, and guiding users toward the next analysis step.