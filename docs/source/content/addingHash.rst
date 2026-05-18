Registering a new Hash Family for Testing
=================================================

We support two approaches to add a new candidate LSH to BioLSHasher: using the **interactive template generator script**, or **manually** using the `EXAMPLE_TEMPLATE.cpp <https://github.com/bpanda-dev/BioLSHasher/blob/main/hashes/EXAMPLE_TEMPLATE.cpp>`_ file.

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
     - Confirms the hash is an LSH candidate; exits if not (BioLSHasher is LSH-only)
   * - 8
     - Similarity Name
     - Built-in (``Hamming``, ``Jaccard``, ``Cosine``, ``Angular``, ``Edit``) or custom name
   * - 9
     - Similarity Function
     - Auto-set for built-in metrics; prompts for a C++ function name if custom
   * - 10
     - Hash Parameters
     - (Optional) Add configurable parameters (e.g., k-mer size, window size)

.. note::
  
  Every input is validated for naming rules and C++ keyword checks. The script will re-prompt if bad input is provided. If the hash is not an LSH candidate, the script stops with a message that BioLSHasher only supports LSH-related tests and the tests may be ineligible for other Hash types.

**It generates:**

1. A compilable C++ template file at ``hashes/<hashname>.cpp`` containing:

   - Copyright header with your chosen license
   - A hash function stub for each selected output bit size
   - The similarity function implementation (included automatically for built-in metrics like
     Hamming or Edit; a stub for custom metrics)
   - A default equality checker for hash outputs, defining the condition under which two outputs
     produced by the hash function are considered identical
   - Macros defined for parameters of the hash function

2. An updated ``hashes/Hashsrc.cmake`` with the new hash registered.

.. note::

   **Full documentation:** See :doc:`Hash Template Generation Script documentation <hashTemplateGenerationScript>` for
   the complete reference including validation rules, example sessions, and troubleshooting.

Option B: Manual Creation
~~~~~~~~~~~~~~~~~~~~~~~~~~



.. .. _addingHash-nonLSHfunctions:

.. Adding a non-Locality-Sensitive Hashing Family 
.. ----------------------------------------------