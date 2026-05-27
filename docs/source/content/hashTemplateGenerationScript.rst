Hash Template Generator for BioLSHasher
========================================

**Filename:** ``createHashTemplate.py``

We provide an interactive command-line script that scaffolds a new C++ hash function file and registers it within the BioLSHasher build system. It walks you through **10 guided steps**, validates every input, generates a ready-to-compile ``.cpp`` file in the ``hashes/`` directory, and automatically updates ``hashes/Hashsrc.cmake`` so the new hash is included in the next build.


Quick Start
-----------

.. code-block:: bash

   # From the BioLSHasher root directory
   python3 createHashTemplate.py

Follow the interactive prompts. At the end you will have:

1. A new file ``hashes/<hashname>.cpp`` containing a compilable template with your hash function stub, similarity function (built-in or custom stub), and all registration macros.
2. An updated ``hashes/Hashsrc.cmake`` with the new file registered.

Then build and verify:

.. code-block:: bash

   mkdir -p build && cd build
   cmake ..
   make
   ./BioLSHasher --list | grep <YourHashName>

----

Step-by-Step Walkthrough
------------------------

Step 1 : Hash Name
~~~~~~~~~~~~~~~~~~
**Description**: The primary identifier for your hash function. This name is used for:

- The C++ function name (e.g., ``MyHash(...)``) (assume the Hash name is `MyHash`)
- The ``REGISTER_HASH(MyHash, ...)`` macro call
- The output filename (``myhash.cpp``, automatically lowercased)

.. dropdown:: Validation Rules

   .. list-table::
      :header-rows: 1
      :widths: 25 75

      * - Rule
        - Constraint
      * - Length
        - 2–50 characters
      * - First character
        - Must be a letter (``a-z``, ``A-Z``)
      * - Allowed characters
        - Letters, digits, underscores only (``^[a-zA-Z][a-zA-Z0-9_]*$``)
      * - Reserved words
        - Cannot be a C++ keyword (``if``, ``class``, ``void``, ``template``, etc.)

.. .. admonition:: Validation Rules

..    .. list-table::
..       :header-rows: 1
..       :widths: 25 75

..       * - Rule
..         - Constraint
..       * - Length
..         - 2–50 characters
..       * - First character
..         - Must be a letter (``a-z``, ``A-Z``)
..       * - Allowed characters
..         - Letters, digits, underscores only (``^[a-zA-Z][a-zA-Z0-9_]*$``)
..       * - Reserved words
..         - Cannot be a C++ keyword (``if``, ``class``, ``void``, ``template``, etc.)

.. .. card:: Validation Rules

..    .. list-table::
..       :header-rows: 1
..       :widths: 25 75

..       * - Rule
..         - Constraint
..       * - Length
..         - 2–50 characters

----

Step 2 : Author Name
~~~~~~~~~~~~~~~~~~~~

**Description**: Used in the copyright header of the generated file:

.. .. code-block:: cpp

..    /*
..    * MyHash
..    * Copyright (C) 2026 <Your Name Here>
..    * ...
..    */

----

Step 3 : License
~~~~~~~~~~~~~~~~

**Description**: Select the open-source license for the copyright header. Pressing **Enter** defaults to **MIT License**.

.. dropdown:: Supported Licenses

  .. list-table::
    :header-rows: 1
    :widths: 15 85

    * - Choice
      - License
    * - 1
      - MIT License **(default)**
    * - 2
      - Apache 2.0
    * - 3
      - BSD 2-Clause
    * - 4
      - BSD 3-Clause
    * - 5
      - GPL v2
    * - 6
      - GPL v3
    * - 7
      - Public Domain / CC0
    * - 8
      - Custom

If you select **8 (Custom)**, you will be prompted to enter free-form license text.

----

Step 4 : Hash Family Name
~~~~~~~~~~~~~~~~~~~~~~~~~

**Description**: The family name groups related hash variants(with common similarity metric) under a single ``REGISTER_FAMILY(...)`` block. This is useful when you have multiple bit-width variants (e.g., ``MyHash_32``, ``MyHash_64``) that belong to the same logical family ``MyHash``.  

Press **Enter** to reuse the hash name from Step 1.

.. dropdown:: Validation Rules

   .. list-table::
      :header-rows: 1
      :widths: 25 75

      * - Rule
        - Constraint
      * - Length
        - 2–50 characters
      * - First character
        - Must be a letter (``a-z``, ``A-Z``)
      * - Allowed characters
        - Letters, digits, underscores only (``^[a-zA-Z][a-zA-Z0-9_]*$``)
      * - Reserved words
        - Cannot be a C++ keyword (``if``, ``class``, ``void``, ``template``, etc.)

----

.. Step 5 :Repository URL
.. ~~~~~~~~~~~~~~~~~~~~~~~

.. **Prompt:** ``Repository URL [https://github.com/username/repo_name]:``

.. The URL of the source repository where the hash implementation originates. This is stored in the ``REGISTER_FAMILY`` macro:

.. .. code-block:: cpp

..    REGISTER_FAMILY(MyHash,
..    $.src_url = "https://github.com/you/your-hash",
..    $.src_status = HashFamilyInfo::SRC_STABLEISH
..    );

.. **Validation:**

.. .. list-table::
..    :header-rows: 1
..    :widths: 20 80

..    * - Rule
..      - Constraint
..    * - Format
..      - Must start with ``http://`` or ``https://``
..    * - Length
..      - ≤ 500 characters
..    * - Characters
..      - No whitespace, angle brackets, or shell-unsafe characters

.. **Default:** ``https://github.com/username/repo_name`` (press Enter to accept).

.. ---

.. Step 6 :Source Status
.. ~~~~~~~~~~~~~~~~~~~~~~

.. **Prompt:** ``Select source status:``

.. Indicates the maturity level of the hash implementation. This is embedded in the ``REGISTER_FAMILY`` macro as ``HashFamilyInfo::SRC_<STATUS>``.

.. .. list-table::
..    :header-rows: 1
..    :widths: 20 25 55

..    * - Choice
..      - Macro Value
..      - Meaning
..    * - ``UNKNOWN``
..      - ``SRC_UNKNOWN``
..      - No information available about the source status
..    * - ``FROZEN``
..      - ``SRC_FROZEN``
..      - Code is finalized; no further changes expected
..    * - ``STABLEISH``
..      - ``SRC_STABLEISH``
..      - Code is stable but minor updates are possible **(default)**
..    * - ``ACTIVE``
..      - ``SRC_ACTIVE``
..      - Code is actively being developed

.. You can enter the choice number (``1``–``4``) or type the status name directly (e.g., ``ACTIVE``).

.. ---

Step 5 : Description
~~~~~~~~~~~~~~~~~~~~

**Description**: A brief, human-readable description of your hash function. Useful to provide description to new users about the hash function.

.. .. code-block:: cpp

..    $.desc = "Your description here",

----

Step 6 : Hash Output Size (Bits)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description**: Select one or more output bit sizes. You can choose multiple variants in a single run.

.. dropdown:: Supported Output Sizes

  .. list-table::
    :header-rows: 1
    :widths: 15 85

    * - Choice
      - Bit size
    * - 1
      - 32 bits **(default)**
    * - 2
      - 64 bits
    * - 3
      - 128 bits
    * - 4
      - 256 bits
    * - 5
      - 512 bits
    * - 6
      - Custom (must be a multiple of 8, ≤ 1024)

For **Multiple selection**, Separate choices with commas. For e.g.

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Input
     - Result
   * - ``1``
     - 32-bit variant only
   * - ``1,2``
     - 32-bit and 64-bit variants
   * - ``32,64,128``
     - Direct bit values work too

When multiple sizes are selected, the script generates:

- A **separate C++ function** for each size (suffixed: ``MyHash_32``, ``MyHash_64``, ...)
- A **separate** ``REGISTER_HASH`` **block** for each size
- A **single shared** ``REGISTER_FAMILY`` **block**

----

Step 7 : LSH Candidacy
~~~~~~~~~~~~~~~~~~~~~~

**Description**: This step is used to get confirmation from the user about the LSH status of the hash function. 

BioLSHasher is a framework specifically designed for testing Locality-Sensitive Hashing (LSH) functions. All test suites in BioLSHasher evaluate LSH properties.

- **If you answer** ``Y`` **(default):** The hash is marked as an LSH candidate and ``FLAG_HASH_LOCALITY_SENSITIVE`` will be set in the generated ``REGISTER_HASH`` block.
- **If you answer** ``N``**:** The script exits immediately with the following message:


<FUTURE USE: RIGHT NOW ITS JUST FOR ADDING LSH FUNCTIONS LATERON IF WE can have two paths for the code based on if its to be tested as LSH or used as a utility(nonLSH) inside an LSH.>

.. code-block:: text

   ============================================================
     BioLSHasher only contains tests related to Locality-Sensitive
     Hashing (LSH). Non-LSH hash functions are not supported by
     the current test infrastructure.

     If you believe your hash has LSH properties, please rerun
     this script and select 'Y' at the LSH candidacy step.
   ============================================================

.. note::

   This is the only step where the script can exit early. All other steps re-prompt on invalid input.

----

Step 8 : Similarity Name
~~~~~~~~~~~~~~~~~~~~~~~~~

**Description**: Enter the name of the similarity measure that your hash function preserves. BioLSHasher has built-in implementations for the following similarity metrics:

.. admonition:: In-Built Similarity Metrics

  .. list-table::
    :header-rows: 1
    :widths: 25 75

    * - Built-in Name
      - Description
    * - ``Hamming``
      - Fraction of matching positions (equal-length sequences)
    * - ``Jaccard``
      - Set intersection over set union
    * - ``Cosine``
      - Cosine of the angle between frequency vectors
    * - ``Angular``
      - Normalised angular distance
    * - ``Edit``
      - 1 − (edit distance / Length of the longer sequence)

You may also enter a **custom similarity name** if your metric is not listed above.

.. dropdown:: Validation Rules

  .. list-table::
    :header-rows: 1
    :widths: 25 75

    * - Rule
      - Constraint
    * - Length
      - 1–50 characters
    * - First character
      - Must be a letter
    * - Allowed characters
      - Letters, digits, underscores only (``^[a-zA-Z][a-zA-Z0-9_]*$``)
    * - Reserved words
      - Cannot be a C++ keyword (``if``, ``class``, ``void``, ``template``, etc.)

.. tip::

   For edit similarity, just write ``Edit`` , not ``Edit Similarity`` or ``EditSimilarity``.

----

Step 9 : Similarity Function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Description**: This step behaves differently depending on whether the similarity name from Step 10 is a built-in or custom metric.

**If built-in** (e.g., ``Edit``, ``Hamming``, etc.):

The script automatically sets the function name and informs you that the implementation will be included in the generated file:

.. code-block:: text

   'Edit' is a built-in similarity metric in BioLSHasher.
   The implementation of 'EditSimilarity' will be included in your hash file.
     -> Similarity function set to: EditSimilarity

No further input is needed. The full C++ implementation of the similarity function will be injected into your hash file.

**If custom:**

You are asked to name your C++ similarity function. This function must follow the ``SimilarityFn`` signature:

.. code-block:: cpp

   double FnName(const std::string& seq1, const std::string& seq2,
                 const uint32_t in1_len, const uint32_t in2_len);

The script generates a stub function in the output file that you need to implement:

.. code-block:: cpp

   //------------------------------------------------------------
   // MyMetricSimilarity: Custom similarity function - implement your logic here.
   // Must return a value in [0.0, 1.0] where 1.0 means identical.
   double MyMetricSimilarity(const std::string& seq1, const std::string& seq2, const uint32_t in1_len, const uint32_t in2_len) {
       // TODO: Implement your similarity computation.
       return 0.0;
   }

**Validation:** Must be a valid C++ identifier (letters, digits, underscores; starts with a letter or underscore).

----

Step 10 : Hash Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
**Description**: Define configuration parameters that control your hash function's behavior and performance. When comparing hash performance across runs with different parameter sets, these registered parameters allows for identifying which configuration produced each result set.

For each parameter, provide:

- Parameter name: Valid C++ identifier (e.g., k, window_size)
- Parameter description: in few words (e.g., k-mer size, Sliding window length)
- Parameter value: Numeric value or C++ constant name (e.g., 10, WINDOW_SIZE_DEFAULT)

----

.. Generated C++ File Structure
.. -----------------------------

.. For a hash named ``MyHash`` with **32-bit and 64-bit** variants using **Edit** similarity (built-in), the generated ``hashes/myhash.cpp`` looks like:

.. .. code-block:: cpp

..    /*
..     * MyHash
..     * Copyright (C) 2026 Your Name
..     *
..     * Licensed under the MIT License.
..     */
..    #include "specifics.h"
..    #include "Hashlib.h"
..    #include "LSHGlobals.h"

..    #include <vector>
..    #include <cassert>
..    #include <cmath>
..    #include <set>
..    #include <map>
..    //------------------------------------------------------------
..    // MyHash implementation

..    //------------------------------------------------------------
..    static void MyHash_32( const void * in, const size_t len, const seed_t seed, void * out ) {
..        // Output: 32 bits
..        uint32_t hash = 0;
..        // Copy the hash state to the output.
..        PUT_U32(hash, (uint8_t *)out, 0);
..    }

..    //------------------------------------------------------------
..    static void MyHash_64( const void * in, const size_t len, const seed_t seed, void * out ) {
..        // Output: 64 bits
..        uint64_t hash = 0;
..        // Copy the hash state to the output.
..        PUT_U64(hash, (uint8_t *)out, 0);
..    }

..    //------------------------------------------------------------
..    // Edit Similarity: 1 - (edit_distance / max_length)
..    double EditSimilarity(const std::string& seq1, const std::string& seq2, const uint32_t in1_len, const uint32_t in2_len) {
..        // ... full implementation auto-generated ...
..    }

..    //------------------------------------------------------------
..    REGISTER_FAMILY(MyHash,
..       $.src_url    = "https://github.com/you/your-hash",
..       $.src_status = HashFamilyInfo::SRC_STABLEISH
..     );

..    REGISTER_HASH(MyHash_32,
..       $.desc            = "My custom hash (32-bit)",
..       $.hash_flags      =
..             FLAG_HASH_LOCALITY_SENSITIVE,
..       $.impl_flags      =
..             0,
..       $.bits            = 32,
..       $.hashfn          = MyHash_32,
..       $.similarity_name = "Edit",
..       $.similarityfn    = EditSimilarity
..     );

..    REGISTER_HASH(MyHash_64,
..       $.desc            = "My custom hash (64-bit)",
..       $.hash_flags      =
..             FLAG_HASH_LOCALITY_SENSITIVE,
..       $.impl_flags      =
..             0,
..       $.bits            = 64,
..       $.hashfn          = MyHash_64,
..       $.similarity_name = "Edit",
..       $.similarityfn    = EditSimilarity
..     );

.. If you select a **single bit size**, the suffix is omitted (e.g., ``MyHash`` instead of ``MyHash_32``).

.. ----

.. Non-Standard Bit Sizes
.. -----------------------

.. For bit sizes other than 32 or 64 (e.g., 128, 256, 512), the generator creates multiple ``uint64_t`` / ``uint32_t`` variables with warning comments prompting you to adapt the storage and copy mechanism:

.. .. code-block:: cpp

..    static void MyHash_128( const void * in, const size_t len, const seed_t seed, void * out ) {
..        // Output: 128 bits
..        // WARNING: This is not a standard output size (32 or 64 bits).
..        // Please change the datatype of hash value storage and
..        // the memory copy mechanism below according to your implementation.
..        //
..        uint64_t hash0 = 0;  // bits 0 to 63
..        uint64_t hash1 = 0;  // bits 64 to 127
..        // Copy the hash state to the output.
..        PUT_U64(hash0, (uint8_t *)out, 0);
..        PUT_U64(hash1, (uint8_t *)out, 8);
..    }

.. ----

.. Hashsrc.cmake Update
.. ---------------------

.. The script automatically appends the new file to ``hashes/Hashsrc.cmake``:

.. .. code-block:: cmake

..    set(HASH_SRC_FILES
..      ...
..      hashes/myhash.cpp    # ← newly added
..    )

.. The entry is inserted just before the closing ``)`` of the ``set()`` command. If the entry already exists, it is skipped with a message.

.. ----

After Running the Script
------------------------

Once the hash file is generated, follow these steps to fill the boilerplate code:

1. Implement Your Hash Logic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Open ``hashes/<hashname>.cpp`` and replace the placeholder code inside the hash function with your actual hash implementation:

.. code-block:: cpp

   static void MyHash( const void * in, const size_t len, const seed_t seed, void * out ) {
       // TODO: Replace this with your hash logic
       uint32_t hash = 0;

       // Your implementation goes here...
       // 'in'   = pointer to input data (genomic sequence, unencoded)
       // 'len'  = length of input in bytes
       // 'seed' = seed value for the hash

       PUT_U32(hash, (uint8_t *)out, 0);
   }

2. Implement Custom Similarity (if applicable)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you entered a custom similarity name in Step 10, the generated file contains a stub function that returns ``0.0``. You need to implement your similarity logic there. The function must return a value in ``[0.0, 1.0]`` where ``1.0`` means identical.

If you chose a built-in similarity metric (Hamming, Jaccard, Cosine, Angular, or Edit), the full implementation is already included.

3. Build and Verify Integration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   cd build
   cmake ..
   make
   ./BioLSHasher --list | grep MyHash

You should see your hash listed in the output.

----


Troubleshooting
---------------

.. list-table::
   :header-rows: 1
   :widths: 35 45

   * - Problem
     - Remark
   * - ``Warning: Could not find .../Hashsrc.cmake``
     - Run the script from the BioLSHasher root directory
   * - File already exists prompt
     - Hash name already taken. Either choose a different name or confirm overwrite.
   * - Hash not showing in ``--list``
     - Most probably CMake cache stale; Delete build directory and rebuild ``rm -rf build && mkdir build && cd build && cmake .. && make``

----

Notes
-----

- The script can be cancelled at any point with ``Ctrl+C`` - no files will be written until final confirmation.
- If you select a **single bit size**, the function and registration use the bare hash name (e.g., ``MyHash``). If you select **multiple bit sizes**, each variant gets a suffix (e.g., ``MyHash_32``, ``MyHash_64``).
- If you choose a **built-in similarity metric**, the full C++ implementation is injected into the generated file along with the necessary ``#include`` headers. If you choose a **custom metric**, a stub function is generated that you must implement.
.. - The only early exit point is **Step 9 (LSH Candidacy)** - if you indicate the hash is not an LSH candidate, the script terminates because BioLSHasher's test suites only evaluate LSH properties.