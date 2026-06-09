Output File Formats
====================


Output Tags
~~~~~~~~~~~

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
     - SNP rates
   * - ``:7:`` - ``:9:``, ``:14:``
     - Geometric mutation params: deletion rate, insertion mean, stay probability, insertion rate
   * - ``:10:`` - ``:12:``
     - Repeated per ``(b, r)``: headers, parameter values, collision rates



Output Tags
~~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Tag
     - Content
   * - ``:1:`` - ``:3:``
     - Header, column names, values
   * - ``:4.x:``
     - Hash-specific parameters
   * - ``:5:``
     - Column headers: ``b,r,c,Avg_Recall,Avg_Precision,Avg_FPR,Avg_Query_Time_ms,Avg_Index_Size_MB``
   * - ``:6:``
     - Repeated per ``(b, r, c)``: metric values