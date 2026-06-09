Custom Output CSV formats
=========================
BioLSHasher returns the results of the tests in a custom csv format. Running the ``generate_report.py`` in analysis directory generates the standard csv files for the specified output files along with the plots.

Collision Curve Test Output Tags
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table::
   :header-rows: 1


   * - Tag
     - Content
   * - ``:1:``
     - Section header: ``LSH Collision Test Results``
   * - ``:2:``
     - Column names: ``Hashname, SequenceLength, Distance Metric, Mutation Model, Mutation Expression``
   * - ``:3:``
     - Values: hash name, sequence length, similarity metric name, mutation model enum, mutation expression type
   * - ``:4.x:``
     - Hash-specific parameters
   * - ``:13:``
     - Raw aggregation error params
   * - ``:5:``
     - Similarity values
   * - ``:6:``
     - substitution rates
   * - ``:7:`` - ``:9:``, ``:14:``
     - Geometric mutation params: deletion rate(7), insertion mean(8), stay probability(9), insertion rate(14)
   * - ``:10:`` - ``:12:``
     - Repeated per ``(b, r)``: AND-OR header(10), (b,r) parameter values(11), collision rates(12)


Threshold Based Similarity Search Test Output Tags
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Tag
     - Content
   * - ``:1:``
     - Section header: ``LSH Approx Nearest Neighbour Summary``
   * - ``:2:``
     - Column names: ``Hashname, SequenceLength, Distance Metric, Mutation Model, Mutation Expression``
   * - ``:3:``
     - Values: hash name, sequence length, similarity metric name, mutation model enum, mutation expression type
   * - ``:4.x:``
     - Hash-specific parameters
   * - ``:5:``
     - Results table column header: ``b, r, c, Avg_Recall, Avg_Precision, Avg_FPR, Avg_Query_Time_ms, Avg_Index_Size_MB``
   * - ``:6:``
     - Per ``(b, r, 1)`` result row: b, r, 1 value, avg recall, avg precision, avg FPR, avg query time (ms), avg index size (MB) : one row per combination, averaged over ``NUM_RUNS`` runs

**Notes:**
The defunct parameter ``c`` is set to 1. This parameter is carried from an old implementation which is not functional in this version. It was for c-ANN implementation and it not used now.