Welcome to BioLSHasher!
===================================

**BioLSHasher** is a C++ benchmarking framework for evaluating locality-sensitive hashing (LSH) families against biologically relevant mutation models and genomic sequence datasets. It is derived from `SMHasher3 <https://gitlab.com/fwojcik/smhasher3>`_, extended and adapted for sequence similarity search.

You might try BioLSHasher if you want to -
   - Evaluate the locality-sensitive properties of your hash family.
   - Analyse amplification behaviour via AND-OR sweeps.
   - Sweep across hash-specific hyperparameters to find the optimal operating points.
   - Benchmark against biologically realistic mutation models to understand to understand which mutation types degrade hashing performance the most.
   - Evaluate approximate nearest-neighbour (ANN) search performance.
   - Compare hash families on a common footing.

.. note::

   This project is under active development.


Contents
--------

.. toctree::

   testCollisionCurve
   testSimilaritySearch
   MutationModels
