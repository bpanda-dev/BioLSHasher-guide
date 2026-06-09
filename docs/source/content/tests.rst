Available Tests
===============================================
BioLSHasher currently supports two tests for evaluating candidate LSH functions in the context of DNA sequences and genomic mutations. The two tests are Collision Curve Analysis and Threshold Based Similarity Search Performance Evaluation. Both these tests target a different aspect of the Hash Function under test. Below we give a concise description of the tests. 

.. note::
	For a brief introduction to relevant definitions, please refer to :doc:`Hash Functions, Hash Family and Locality Sensitive Hashing <hashfunctionHashfamilyandLSH>`.

1. Collision Curve Analysis
---------------------------

Collision curve analysis characterizes the relationship between pairwise sequence similarity and the empirical probability of hash collision. A well-behaved LSH family should exhibit a monotonically increasing collision probability between two inputs as the similarity between them increases. 

This test reports empirical collision rates stratified across similarity bins, together with their variance across independent trials, for each hash family under evaluation. This stratified collision probability values serves as the primary(and first) view for whether a given LSH scheme preserves the locality property under the mutation models and distance metrics used in the benchmark. Additionally, collision curve test also gives the :math:`\rho` value for a given :math:`s_1` and :math:`s_2` which represents the computational efficiency of the hash function. A lower :math:`\rho` represents higher computational efficiency. (See :ref:`_hashfunctionshashfamilyandLSH` for more details.)

.. figure:: ../media/CollisionCurveTheoriticalone.png
   :alt: Collision Curve
   :width: 400px
   :align: center

   Collision Curve

(See :ref:`_hashfunctionshashfamilyandLSH` for more details.)

2. Threshold Based Similarity Search Performance Evaluation
-----------------------------------------------------------
