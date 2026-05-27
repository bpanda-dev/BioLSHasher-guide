Frequently Asked Questions
===========================

General
--------

**What types of hash functions does BioLSHasher support?**
   Tests in BioLSHasher are designed exclusively for LSH candidates. However it is still possible to add non-LSH functions manually (See :ref:`addingHash-nonLSHfunctions` for more details.) as helper hash functions used internally as an utility from within the LSH implementations. For example: in MinHash, which requires a general-purpose scattering function such as FNV or MurmurHash to hash individual tokens before computing the minimum.

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

**My LSH candidate function requires another helper hash function to run. But since the helper hash function is not an LSH how do I add it into the module. **

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