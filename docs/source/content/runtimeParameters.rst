.. Runtime Parameters Reference
.. ===============================================


.. "How to modify and test" walkthrough : a short step-by-step:
.. 1. Open include/LSHGlobals.h
.. 2. Change the parameter (e.g. DEFAULT_K from 15 -> 21)
.. 3. Recompile: cmake --build build/
.. 4. Run: ./biohash --mode ann ...