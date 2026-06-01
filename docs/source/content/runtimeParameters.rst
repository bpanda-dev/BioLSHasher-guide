Runtime Configurations Guide
===============================================

The `config.toml <https://github.com/bpanda-dev/BioLSHasher/blob/main/config.toml>`_ file controls the runtime parameters of the tests in BioLSHasher. This TOML(Tom's Obvious Minimal Language) file is provided as a convenience, so that one does not need to dig into the cpp files to modify them.

Contents and Sections in `config.toml`
------------------------------------
The `config.toml` file is organized into the following sections based on the test and the scale(common or specific to a test) of the properties.:

.. note::
	
	The values assigned to each parameter here serve as its default.

1. Mutation Model Configuration
................................
For a detailed description of the mutations models and their parameters, please refer to :doc:`Mutation Models <mutationModels>`.
The two we set here is the mutation model and the mutation expression type.

- ``Mutation_Model = 1`` 
- ``Mutation_Expression_Type = 0``
.. admonition:: Setting Values for ``Mutation_Model``

	.. list-table::
		:header-rows: 1
		:widths: 35 65
		
		* - Mutation Model
		  - Value to Set
		* - **Substitution-only model.**
		  - 0
		* - **SubIndel mutation model.**
		  - 1

.. admonition:: Setting Values for ``Mutation_Expression_Type``
	
	Please note that the mutation expression type is only relevant when using the **SubIndel mutation model**. For more details, please refer to :doc:`Mutation Models <mutationModels>`.

	.. list-table::
		:header-rows: 1
		:widths: 35 10
		
		* - Mutation Expression Type
		  - Value to Set
		* - **Balanced.**
		  - 0
		* - **Substitution only.**
		  - 1
		* - **Deletion-lite.**
		  - 2
		* - **Insertion-lite.**
		  - 3
		* - **Substitution-lite.**
		  - 4

----


2. Collision Test Configuration
................................

To control the AND-OR amplification for the collision test, we set the following parameters in the `config.toml` file. These parameters set the range of :math:`b` (number of concatenated hashes) and :math:`r` (number of hash tables) to be swept during the collision test.

- **AND Parameter Sweep** (b: number of concatenated hashes)

	- ``Coll_Num_Hashes_To_And_Sweep_Start = 1``
	- ``Coll_Num_Hashes_To_And_Sweep_End = 1``

- **OR Parameter Sweep** (r: number of hash tables)

	- ``Coll_Num_Hashes_To_Or_Sweep_Start = 1``
	- ``Coll_Num_Hashes_To_Or_Sweep_End = 1``

.. dropdown:: Example Configuration for Collision Test

	Here is an example configuration for the collision test parameters in the `config.toml` file:

	.. code-block:: toml

		[Collision_Test_Configuration]
		Coll_Num_Hashes_To_And_Sweep_Start = 1
		Coll_Num_Hashes_To_And_Sweep_End = 5
		Coll_Num_Hashes_To_Or_Sweep_Start = 1
		Coll_Num_Hashes_To_Or_Sweep_End = 5

	This sets :math:`b \in \{1, 2, 3, 4, 5\}` and :math:`r \in \{1, 2, 3, 4, 5\}`. The collision test runs once for each of the 25 combinations :math:`(b, r)`, sweeping the full grid from :math:`(1, 1)` to :math:`(5, 5)`.
	
----

3. Similarity Search Test Configuration
.......................................

- **Database and Query Configuration**

	- ``Total_Sequences_In_Database_To_Search_From = 50000``
	- ``Total_Query_Sequences_To_Search = 2000``

- **Statistical Averaging**

	- ``Number_of_Runs_to_get_Average = 5``

- **AND and OR Parameter Sweep** similar to collision test, but for similarity search test

	- ``SimSearch_Num_Hashes_To_And_Sweep_Start = 1``
	- ``SimSearch_Num_Hashes_To_And_Sweep_End = 1``
	- ``SimSearch_Num_Hashes_To_Or_Sweep_Start = 1``
	- ``SimSearch_Num_Hashes_To_Or_Sweep_End = 1``

- **Similarity threshold** for neighbor classification

	This variable specifies the minimum similarity threshold for classifying sequences as neighbors.

	- ``Similarity_Threshold = 0.95``

----

4. Common Configuration
.................................

- **Sequence Length** Parameter
	
	This variable specifies the length of the sequences in nucleotides(bytes) which are input to the hash functions.

	- ``Sequence_Length = 50``

- **Base Distribution for Nucleotide Generation**
	
	if set to *true* = Uniform distribution across {A, C, G, T} and if *false* = Use custom categorical distribution.
	
	- ``are_Bases_Drawn_From_Uniform_Distribution = true``

- **Categorical Distribution Probabilities**
	
	Used only when ``are_Bases_Drawn_From_Uniform_Distribution = false``.
	Probabilities for {A, C, G, T} - must sum to 1.0
	
	- ``Categorical_Distribution_Probabilities = [0.25, 0.25, 0.25, 0.25]``


- There are three **speed levels** which depends on the hash computation time. There are very_slow, slow, and default.Increase any of the values will increase the runtime of the tests. 

	- ``very_Slow_NAggCases = 200000``
	- ``very_Slow_NSeq = 1000``	
	- ``very_Slow_NHashes = 1000``
	- ``Slow_NAggCases = 200000``
	- ``Slow_NSeq = 3000``	
	- ``Slow_NHashes = 1500``
	- ``NAggCases = 200000``	
	- ``NSeq = 5000`` 		
	- ``NHashes = 2000`` 

----