Mutation Models
===============================================

This document explains the different mutation models supported by BioLSHasher and explains the macros defined in ``LSHGlobals.h`` which are useful in configuring the models.

.. note::
  We will be adding additional mutation models in future. 

The current version of BioLSHasher contains two mutation models:

1. **Substitution Only mutation model**: Performs only point substitutions across the input sequence based on the mutation rate :math:`e_{s} \in [0,1]`. BioLSHasher samples :math:`e_{s}` uniformly from the domain :math:`[0,1]`.
 
2. **SubIndel mutation model**: Performs substitutions and deletions as point mutations (length 1) with the error rates :math:`e_s` and :math:`e_d`, respectively, satisfying :math:`e_s \ge 0,\ e_d < 1 \text{ and } e_s + e_d \le 1`. In contrast, for every position in sequence, insertion events are performed by sampling the length of the insertion from a geometric distribution with mean :math:`\mu \ge 0`. If the sampled insertion length at a position of the sequence is :math:`0`, there is no insertion.
 
 .. note::

	BioLSHasher enforces a compatibility constraint: the SubIndel mutation model with non-zero probabilities is incompatible with LSH families that preserve Hamming Similarity, as indels  alter sequence length and Hamming Similarity is undefined between sequences of unequal length Selecting this combination will cause BioLSHasher to terminate execution.
  
  TODO:About how indels can increase or decrease the input sequence in comparision to a parameter. Eg: for minimizers, the window length cannot be less than kmer length.

The SubIndel mutation model inherently contains three effective degrees of freedom. To streamline testing and avoid the complexity of exploring the entire parameter space, we reduce the degrees of freedom by coupling the mutation rates via fixed functional relationships (we call these *mutation expressions*).

Baseline Parameter Derivation
------------------------------
 
The core parameter for all configurations is the **mean insertion length**, denoted as
:math:`\mu`.
 
1. We draw :math:`\mu` uniformly at random from the interval :math:`[0, 0.03]`. Users can change the upper bound.
2. This :math:`\mu` serves as the mean for the geometric distribution used to sample
   insertion lengths.
3. We define the success probability :math:`p` for this distribution as:
 
   .. math::
 
      p = \frac{1}{1+\mu}
 
4. We then calculate a baseline reference rate, :math:`e_i`, defined as the probability
   of generating an insertion of exactly length 1:
 
   .. math::
 
      e_i = (1-p) \cdot p
 
Using this baseline :math:`e_i`, we define the substitution rate (:math:`e_s`) and
deletion rate (:math:`e_d`) for five distinct experimental scenarios.

Mutation Expressions
---------------------
 
1. **BALANCED**: Substitution, deletion, and length-1 insertion rates are all equal,
   i.e. :math:`e_s = e_i` and :math:`e_d = e_i`.
 
2. **DELETION-lite**: Deletion rate is :math:`\tfrac{1}{5}` of :math:`e_i` and
   :math:`e_s = e_i`. Sequences tend to grow slightly after mutation with new
   nucleotides.
 
3. **INSERTION-lite**: Both substitution and deletion rates are 5× higher,
   i.e. :math:`e_s = 5 \cdot e_i` and :math:`e_d = 5 \cdot e_i`, meaning insertions
   are relatively less impactful. Sequences tend to shrink.
 
4. **SUBSTITUTION-lite**: Substitutions occur at :math:`\tfrac{1}{5}` of :math:`e_i`
   and :math:`e_d = e_i`.
 
5. **SUBSTITUTION-only**: Unlike the standard **Substitution Only** model where the
   rate is drawn from :math:`[0, 1]`, here :math:`e_s` is constrained to the range
   :math:`[0,\ (1-p)p]` where :math:`p = \tfrac{1}{1 + 0.03}`. *Note:* The sole
   purpose of :math:`e_i` here is to set the scalar value for :math:`e_s`. For general
   use, we recommend the standard **Substitution Only** mutation model over this
   constrained version.
 
----

Configuring
-----------
 
To set and configure the mutation models, various global variables are defined in the
``LSHGlobals.h`` file.
 
Setting the Mutation Model
~~~~~~~~~~~~~~~~~~~~~~~~~~
 
The mutation model is set via the global variable ``g_mutation_model`` in
``LSHGlobals.cpp``.
 
.. list-table::
   :header-rows: 1
   :widths: 35 10 55
 
   * - Macro
     - Value
     - Description
   * - ``MUTATION_MODEL_SIMPLE_SNP_ONLY``
     - 0
     - **Substitution-only model.** Each position in the sequence is independently
       flipped to a different base with probability equal to ``snpRate``. Sequence
       length is **never changed** (no insertions or deletions).
   * - ``MUTATION_MODEL_GEOMETRIC_MUTATOR``
     - 1
     - **SubIndel mutation model.** At each position, a substitution, deletion, or
       stay event occurs based on probabilities (:math:`e_s`, :math:`e_d`).
       Additionally, insertions of geometrically-distributed length are drawn at every
       position. Sequence length **can change**. Implemented by
       ``SequenceDataMutatorGeometric``.
 
Edit the initializer of ``g_mutation_model`` in ``LSHGlobals.cpp``:
 
.. code-block:: cpp
 
   // ...
   // Change to MUTATION_MODEL_SIMPLE_SNP_ONLY (0) or MUTATION_MODEL_GEOMETRIC_MUTATOR (1)
   uint32_t g_mutation_model = MUTATION_MODEL_SIMPLE_SNP_ONLY;
   // ...
 
----

Setting the Mutation Expression
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
When using ``MUTATION_MODEL_GEOMETRIC_MUTATOR``, these macros control **the
relationship** between the insertion mean length given by geometric mean parameter
:math:`\mu_i` and the resulting substitution/deletion rates. They are set via the
global constant ``g_mutation_expression_type``.
 
.. list-table::
   :header-rows: 1
   :widths: 35 10 55
 
   * - Macro
     - Value
     - Description
   * - ``MUTATION_EXPRESSION_BALANCED``
     - 0
     - **Balanced.** Substitution, deletion, and length-1 insertion rates are all equal.
   * - ``MUTATION_EXPRESSION_SUB_ONLY``
     - 1
     - **Substitution only.** No deletions occur; insertion lengths are forced to 0.
       Only substitutions happen even though SubIndel framework is the selected model.
   * - ``MUTATION_EXPRESSION_DEL_LITE``
     - 2
     - **Deletion-lite.** Deletions occur at 1/5th the rate of substitutions. Sequences
       tend to grow slightly.
   * - ``MUTATION_EXPRESSION_INS_LITE``
     - 3
     - **Insertion-lite.** Both substitution and deletion rates are 5× higher, meaning
       insertions are relatively less impactful. Sequences mutate more aggressively.
   * - ``MUTATION_EXPRESSION_SUB_LITE``
     - 4
     - **Substitution-lite.** Substitutions occur at 1/5th the rate of deletions.
       Sequences tend to shrink.
 
.. code-block:: cpp
 
   // ...
   // Change to any of: MUTATION_EXPRESSION_BALANCED (0), MUTATION_EXPRESSION_SUB_ONLY (1),
   // MUTATION_EXPRESSION_DEL_LITE (2), MUTATION_EXPRESSION_INS_LITE (3), MUTATION_EXPRESSION_SUB_LITE (4)
   const uint32_t g_mutation_expression_type = MUTATION_EXPRESSION_BALANCED;
   // ...
 
----
 
Quick Reference
---------------
 
.. list-table::
   :header-rows: 1
   :widths: 45 25 30
 
   * - What to change
     - File
     - Variable
   * - Mutation model (SNP-only vs Geometric)
     - ``LSHGlobals.cpp``
     - ``g_mutation_model``
   * - Mutation expression (rate relationships)
     - ``LSHGlobals.cpp``
     - ``g_mutation_expression_type``
 
Both sets of macros are defined in ``LSHGlobals.h``, initialized in ``LSHGlobals.cpp``,
and consumed by the mutation engines in ``BioDataGeneration.cpp``.
