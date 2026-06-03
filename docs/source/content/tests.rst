Available Tests
===============================================
BioLSHasher currently supports two tests for evaluating candidate LSH functions in the context of DNA sequences and genomic mutations. The two tests are Collision Curve Analysis and Threshold Based Similarity Search Performance Evaluation. Both these tests target a different aspect of the Hash Function under test. Below we give a concise description of the tests. 

.. note::
	For a brief introduction to relevant definitions, please refer to :doc:`Hash Functions, Hash Family and Locality Sensitive Hashing <hashfunctionHashfamilyandLSH>`.

1. Collision Curve Analysis
---------------------------

Collision curve analysis characterizes the relationship between pairwise sequence similarity and the empirical probability of hash collision. A good LSH family should exhibit a monotonocally increasing collision probability as similarity increases. 

wherein highly similar sequences should frequently collide, while dissimilar sequences should rarely collide. This section reports empirical collision rates stratified across similarity bins, together with their variance across independent trials, for each hash family under evaluation. These curves serve as the primary diagnostic for whether a given LSH scheme preserves the locality property under the mutation models and distance metrics used while running the benchmark.

2. Threshold Based Similarity Search Performance Evaluation
-----------------------------------------------------------


temp