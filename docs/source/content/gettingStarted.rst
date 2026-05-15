Getting Started
===============================================

Follow these steps to get up and running with BioLSHasher.

1. Clone the Repository
------------------------

.. code-block:: bash

   git clone https://github.com/bpanda-dev/BioLSHasher.git
   cd BioLSHasher

2. Set Up Conda Environment
----------------------------

BioLSHasher is bundled with multiple Python scripts for plotting and for aiding users to connect their hash function to the BioLSHasher framework.

**Prerequisites:** `Miniconda <https://docs.conda.io/en/latest/miniconda.html>`_ or `Anaconda <https://www.anaconda.com/download>`_

2.1. Create the conda environment from the provided ``environment.yaml``:

.. code-block:: bash

   conda env create -f environment.yaml

This creates a conda environment named ``biolshasher``.

2.2. Activate the environment:

It is required by the generate_report.py (step 5).

.. code-block:: bash

   conda activate biolshasher
   # to deactivate biolshasher, use "conda deactivate"

3. Build BioLSHasher
---------------------

.. code-block:: bash

   mkdir build
   cd build
   cmake ..
   make -j$(nproc)

4. Run an Example Test
-----------------------

.. code-block:: bash

   # A sample test run of the two analyses: collision and similarity search on the included
   # example hash function (exampleHash), which preserves Hamming distance.
   ./BioLSHasher --test=LSHCollision,LSHApproxNearestNeighbour exampleHash --ncpu=4

The run should complete in under a minute. If everything works correctly, two output files are written to the ``results/`` directory under ``BioLSHasher``:

- ``collisionResults_Hamming.csv``
- ``ApproxNearestNeighbourResults_Hamming.csv``

.. note::

   1. The filename reflects the similarity metric the hash function operates on, rather than the hash function name.
   2. These files use a custom CSV format and require post-processing before they can be used as standard CSVs.

5. Process the Output Files
-----------------------------

To generate standard CSVs, plots, and an interactive visualisation:

.. code-block:: bash

   python ../analysis/generate_report.py \
       --coll=../results/collisionResults_Hamming.csv \
       --ann=../results/approxNearestNeighbourResults_Hamming.csv

This writes all plots and processed CSVs to the ``results/`` directory under the ``Hamming`` name. An HTML file is also generated, which aggregates all plots into a single interactive visualisation. To view it in Firefox:

.. code-block:: bash

   firefox ../results/Hamming/Hamming_dashboard.html