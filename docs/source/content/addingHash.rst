Adding a novel Hash Family for Testing
=================================================

We support two approaches for adding a new hash function(LSH or non-LSH) to BioLSHasher: using the **interactive template generator script**, or **manually** using the `EXAMPLE_TEMPLATE.cpp <https://github.com/bpanda-dev/BioLSHasher/blob/main/hashes/EXAMPLE_TEMPLATE.cpp>`_ file.


.. note::
  In BioLSHasher, we refer to non-LSH hash functions as helper hash functions. This is because BioLSHasher focuses on LSH hash functions, while non-LSH hash functions are used only as utility functions within LSH implementations when needed.

Option A: Using ``createHashTemplate.py`` (Recommended)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BioLSHasher ships with an interactive Python script that scaffolds a new hash ``.cpp`` file and registers it in the build system automatically. Run it from the repository root:

.. code-block:: bash

   python3 createHashTemplate.py

The script walks you through **10 guided steps**:

.. list-table::
   :header-rows: 1
   :widths: 10 25 65

   * - Step
     - Prompt
     - What it sets
   * - 1
     - Hash Name
     - C++ function name, ``REGISTER_HASH`` identifier, and output filename
   * - 2
     - Author Name
     - Copyright header
   * - 3
     - License
     - License text in file header (MIT default, 8 options)
   * - 4
     - Family Name
     - ``REGISTER_FAMILY(...)`` grouping (defaults to hash name)
   * - 5
     - Description
     - Human-readable description in ``REGISTER_HASH``
   * - 6
     - Output Bits Size
     - 32, 64, 128, 256, 512, 1024 or custom (multiple allowed)
   * - 7
     - LSH Candidacy
     - If yes, the hash family is registered as an LSH candidate; otherwise, it is registered as a helper hash.
   * - 8
     - Similarity Name
     - (Only valid for LSH Candidates) Built-in (``Hamming``, ``Jaccard``, ``Cosine``, ``Angular``, ``Edit``) or custom name
   * - 9
     - Similarity Function
     - (Only valid for LSH Candidates) Auto-set for built-in metrics; prompts for a C++ function name if custom
   * - 10
     - Hash Parameters
     - (Optional) Add configurable parameters (e.g., k-mer size, window size)

.. note::
  
  Every input is validated for naming rules and C++ keyword checks. The script will re-prompt if bad input is provided. Our tests do not provide relevant results for other Hash types like the scattering Hashes.

**It generates:**

1. A compilable C++ template file at ``hashes/<hashname>.cpp`` containing:

   - Copyright header with your chosen license.
   - A hash function stub for each selected output bit size.
   - The similarity function implementation (included automatically for built-in metrics like Hamming or Edit; a stub for custom metrics) - In case of LSH candidate.
   - A default equality checker for hash outputs, defining the condition under which two outputs produced by the hash function are considered identical.
   - Macros defined for parameters of the hash function.

2. An updated ``hashes/Hashsrc.cmake`` with the new hash registered.

.. note::

   **Full documentation:** See :doc:`Hash Template Generation Script documentation <hashTemplateGenerationScript>` for
   the complete reference including validation rules, example sessions, and troubleshooting.

Option B: Manual Creation
~~~~~~~~~~~~~~~~~~~~~~~~~~



.. .. _addingHash-nonLSHfunctions:

.. Adding a non-Locality-Sensitive Hashing Family 
.. ----------------------------------------------