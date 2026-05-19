Sequence Data Generation
===============================================

We simulate DNA sequences of a given length from :math:`\Sigma=\{A,T,G,C\}`. Right now, no other symbols are permitted. The probability of occurence of the symbols in the sequence is decided by either Uniform Sampling or by categorical sampling.


Uniform Sampling
----------------

Each nucleotide is drawn with equal probability:

:math:`p_A = p_T = p_G = p_C = 0.25`

This produces sequences with no compositional bias and serves as the default baseline.

Categorical Sampling
--------------------

A user-defined probability vector :math:`p = (p_A, p_T, p_G, p_C)` assigns an arbitrary weight to each base, subject to the constraint:

:math:`p_A + p_T + p_G + p_C = 1, \quad p_i \geq 0`

At every position in the sequence, a nucleotide is drawn from this categorical distribution. This allows simulation of GC-biased genomes or other non-uniform compositions.