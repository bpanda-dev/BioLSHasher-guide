Output Custom CSV format
========================


Collision Curve Test Output Tags
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Tag
     - Content
   * - ``:1:`` - ``:3:``
     - Header, column names, values
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
     - Hash-specific parameters (written by ``hinfo->printParameters(out_file)``, same mechanism as Collision Curve)
   * - ``:5:``
     - Results table column header: ``b, r, c, Avg_Recall, Avg_Precision, Avg_FPR, Avg_Query_Time_ms, Avg_Index_Size_MB``
   * - ``:6:``
     - Per ``(b, r, c)`` result row: b, r, c value, avg recall, avg precision, avg FPR, avg query time (ms), avg index size (MB) — one row per combination, averaged over ``NUM_RUNS`` runs

**Notes:**
- ``:13:`` (raw aggregation error parameters) is present in the code but **commented out** — it is not emitted in the current build.
- Tags ``:7:``–``:9:``, ``:14:`` (geometric mutation params) and ``:10:``–``:12:`` (AND-OR collision rates per ``(b,r)``) do **not** appear in this test — those belong exclusively to the Collision Curve test.
