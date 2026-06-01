BioLSHasher Documentation
==================================

.. note::

   This project is under active development.

**BioLSHasher** is a C++ benchmarking suite for evaluating locality-sensitive hashing (**LSH**) families under genomic mutation models. It is build on a modified and trimmed down SMHasher3 framework. It currently supports two tests, collision curve analysis and threshold-based similarity search performance evaluation.

You might try BioLSHasher if you want to perform the following evaluations under biologically realistic mutation models-
   - Evaluate the locality-sensitive properties of your candidate LSH.
   - Analyse amplification behaviour across AND-OR sweeps.
   - Sweep across hash-specific parameters to find the optimal operating points.
   - Understand how and which mutation types affect hashing performance of your Hash. 
   - Evaluate the candidate LSH family for its similarity search performance. 
   - Compare hash functions; both within and across LSH families on a common similarity metric.

Contents
--------

.. toctree::
   :maxdepth: 2

   content/gettingStarted
   content/addingHash
   content/runtimeParameters
   content/tests
   content/sequenceDataGeneration
   content/mutationModels
   content/helpFAQ