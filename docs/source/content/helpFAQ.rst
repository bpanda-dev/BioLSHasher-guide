Frequently Asked Questions
===========================

General
--------

**What types of hash functions does BioLSHasher support?**
   BioLSHasher supports both LSH candidate hash functions and non-LSH helper hash functions. The testing framework is designed specifically for LSH candidates. Non-LSH hash functions(or helper hash functions) can still be added using ``createHashTemplate.py`` by marking them as not being LSH candidates in step 7 of the script, or they can be added manually using the ``EXAMPLE_TEMPLATE.cpp``. These helper hashes are intended for internal use inside LSH implementations, such as in MinHash, where a general-purpose scattering function like FNV or MurmurHash is used to hash individual tokens before selecting the minimum value.

**Which similarity metrics are supported out of the box?**
   BioLSHasher includes built-in support for ``Hamming``, ``Jaccard``, ``Cosine``, ``Angular``, and ``Edit`` distance. Custom similarity metrics can also be registered by providing a C++ function implementation.


Adding a New Hash Function
---------------------------

**What is the recommended way to add a new LSH function?**
   Use the interactive script ``createHashTemplate.py`` in the Project root directory. It scaffolds the ``.cpp`` file and updates ``hashes/Hashsrc.cmake`` automatically. See :doc:`Adding a Hash Function <addingHash>` for more information.

**Can I add a hash function that operates on a custom similarity metric?**
   Yes. At Step 8 of ``createHashTemplate.py``, enter a custom similarity name instead of selecting a built-in metric. You will then be prompted to provide a C++ function name for your similarity metric implementation. A stub with the correct signature will be generated automaticall and registered along with your hash family. You must then provide the custom implementation logic before building.

**My hash function has tunable parameters (e.g., k-mer size). How do I handle this?**
   Use Step 10 of ``createHashTemplate.py`` to register named parameters. Each parameter is emitted as a ``#define`` macro at the top of the generated ``.cpp`` file, making it easy to adjust values without modifying the core logic. Registering your hash parameters allows BioLSHasher to associate, each test result with the exact parameters it ran with.

**My LSH candidate function requires another helper hash function to run. But since the helper hash function is not an LSH how do I add it into the module.**


Running Tests
--------------

**How do I run both collision and ANN tests together?**
   Pass both test names as a comma-separated list to the ``--test`` flag:

   .. code-block:: bash

      ./BioLSHasher --test=LSHCollision,LSHApproxNearestNeighbour <HashName> --ncpu=4


Output and Reporting
---------------------

**Why does the BioLSHasher output CSV require post-processing?**
   BioLSHasher writes results in a custom CSV format optimised for streaming large parameter grids efficiently. The ``generate_report.py`` script converts these into standard CSVs and produces plots and an interactive HTML dashboard.

**Where are the processed results saved?**
   All processed CSVs, plots, and the HTML dashboard are written to ``results/<SimilarityName>/`` under the Project root directory. For example, results for a Hamming-distance hash are saved to ``results/Hamming/``.

**The HTML dashboard does not load in my browser. What should I do?**
   In the case of a large number of experiments compared together, plots may take a moment to load. We recommend Firefox for visualisation.

**The output filename reflects the similarity metric, not my hash name. Is this a bug?**
   This is intentional. Since hash function comparisons are only meaningful under the same similarity metric, encoding the metric in the output name ensures results are never inadvertently compared across different metrics.

Plotting Error
--------------

If you encounter an error similar to the following while generating reports and plots:

   .. code-block:: bash

      --- Processing Collision Curve data: ../results/collisionResults_Hamming.csv ---
      Reading: ../results/collisionResults_Hamming.csv
      Traceback (most recent call last):
      File "~/BioLSHasher/analysis/plot_collisioncurve.py", line 1208, in <module>
         main()
      File "~/BioLSHasher/analysis/plot_collisioncurve.py", line 1079, in main
         sections, df = read_collision_data_complete(args.csvfile)
                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      File "~/BioLSHasher/analysis/plot_collisioncurve.py", line 102, in read_collision_data_complete
         collision_rates = [float(x.strip()) for x in line_content.split(',')]
                           ^^^^^^^^^^^^^^^^
      ValueError: could not convert string to float: ''
      Error occurred while plotting collision curve.


this usually indicates that the output CSV file has been corrupted or partially overwritten during either the current run or during previous experiment(s). This is applicable for both: Collision Test and Similarity Search Test.

**Solution:** Delete the existing results CSV file and rerun the experiment so that a clean output file is generated.

For example:

   .. code-block:: bash
      
      rm ../results/collisionResults_Hamming.csv
   
Then rerun the experiment and regenerate the plots.