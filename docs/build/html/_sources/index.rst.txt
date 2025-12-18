Welcome to UnTANGLeD R Package's documentation!
===============================================

This guide provides instructions on using the UnTANGLeD R package for unsupervised consensus clustering analysis of high-dimensional biological data. 

Contents:
---------

.. toctree::
   :maxdepth: 2

   installation
   clusterOptimiser
   untangled
   conservation  


About UnTANGLeD
===============

UnTANGLeD is an unsupervised consensus clustering and prioritisation pipeline that:

- Performs iterative clustering across multiple granularity levels 
- Builds consensus matrices from co-clustering frequencies 
- Determines optimal cluster numbers using silhouette analysis
- Evaluates cluster quality through intra cluster correlation and robustness metrics
- Assesses conservation across biological contexts



Quick Start
===========

1. Install and load the package
2. Run ``clusterOptimiser()`` on your preprocessed data
3. Select optimal cluster number from the silhouette plot
4. Run ``untangled()`` with the chosen cluster number
5. Evaluate clusters using conservation analysis functions (cmc or gcc)


Indices and tables
==================

* :ref:`genindex`
* :ref:`search`
