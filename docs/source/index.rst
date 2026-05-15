BioLSHasher Documentation
==================================

.. note::

   This project is under active development.

**BioLSHasher** is a C++ benchmarking framework for evaluating locality-sensitive hashing (LSH) families against biologically relevant mutation models and genomic sequence datasets.

<TODO: adding the image of plotS here like the deeptools along with a logo? if possible.>

You might try BioLSHasher if you want to - 
   - Evaluate the locality-sensitive properties of your hashing scheme.
   - Analyse amplification behaviour via AND-OR sweeps.
   - Sweep across hash-specific hyperparameters to find the optimal operating points.
   - Benchmark against biologically realistic mutation models to understand to understand which mutation types degrade hashing performance the most.
   - Evaluate approximate nearest-neighbour (ANN) search performance.
   - Compare hash families on a common footing.


Contents
--------

.. toctree::
   :maxdepth: 1

   content/gettingStarted
   content/addingHash
   content/tests
   content/mutationModels
   content/sequenceDataGeneration
   content/runtimeParameters.rst
   content/helpFAQ
   