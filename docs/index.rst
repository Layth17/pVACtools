pVACtools
=========

pVACtools is a cancer immunotherapy tools suite consisting of the following
tools:

**pVACseq**
   A cancer immunotherapy pipeline for identifying and prioritizing neoantigens from a VCF file.

**pVACbind**
   A cancer immunotherapy pipeline for identifying and prioritizing neoantigens from a FASTA file.

**pVACfuse**
   A tool for detecting neoantigens resulting from gene fusions.

**pVACvector**
   A tool designed to aid specifically in the construction of DNA-based
   cancer vaccines.

**pVACviz**
   A browser-based user interface that assists
   users in launching, managing, reviewing, and visualizing the results of
   pVACtools processes.

.. image:: images/pVACtools_main-figure_v5a.png
    :align: center
    :alt: pVACtools immunotherapy workflow


.. toctree::
   :maxdepth: 2

   pvacseq
   pvacbind
   pvacfuse
   pvacvector
   pvacviz

.. toctree::
   :maxdepth: 1

   install
   tools
   frequently_asked_questions
   releases
   license
   citation
   contact
   mailing_list

New in release |release|
------------------------

This is a hotfix release. It fixes the following issues:

- The standalone ``pvacbind top_score_filter`` command woul throw an error
  because it wasn't set up correctly. This has now been fixed.
- The standalone ``pvacfuse generate_protein_fasta`` would fail when run with
  the ``--input-tsv`` option because it wasn't able to associate TSV entries
  with the fasta entries correctly. Using this option will now correctly limit
  the output to only entries from the input TSV file.
- In certain situation the trimming of problematic peptides in pVACvector
  would not work correctly. This issue has now been addressed.

New in version |version|
------------------------

This version adds the following features:

- This version introduces a new tool, ``pVACbind``, which can be used
  to run our immunotherapy pipeline with a peptides
  FASTA file as input. This new tool is similar to pVACseq but certain
  options and filters are removed:

  - All input sequences are interpreted in isolation so corresponding
    wildtype sequence and score information are not assigned. As a consequence,
    the filter threshold option on fold change is removed.
  - Because the input format doesn't allow for association of readcount,
    expression or transcript support level data, pVACbind doesn't run the coverage
    filter or transcript support level filter.
  - No condensed report is generated.

  Please see the :ref:`pvacbind` documentation for more information.

- pVACfuse now support annotated fusion files from `AGFusion <https://github.com/murphycj/AGFusion>`_ as input. The
  :ref:`pvacfuse` documentation has been updated with instructions on how to
  run AGFusion in the Prerequisites section.
- The top score filter has been updated to take into account alternative known
  transcripts that might result in non-indentical peptide sequences/epitopes.
  The top score filter now picks the best epitope for every available transcript of a
  variant. If the resulting list of epitopes for one variant is not identical,
  the filter will output all eptiopes. If the resulting list of epitopes for one
  variant are identical, the filter only outputs the epitope for the transcript with the highest
  transcript expression value. If no expression data is available, or if
  multiple transcripts remain, the filter outputs the epitope for the
  transcripts with the lowest transcript Ensembl ID.
- This version adds a few new options to the ``pvacseq
  generate_protein_fasta`` command:

  - The ``--mutant-only`` option can be used to only output mutant peptide
    sequences instead of mutant and wildtype sequences.
  - This command now has an option to provide a pVACseq all_eptiopes or
    filtered TSV file as an input (``--input-tsv``). This will limit the
    output fasta to only sequences that originated from the variants in that file.

- This release adds a ``pvacfuse generate_protein_fasta`` command that works
  similarly to the ``pvacseq generate_protein_fasta`` command but works with
  Integrate-NEO or AGFusion input files.
- We removed the sorting of the all_epitopes result file in order to reduce
  memory usage. Only the filtered files will be sorted. This version also updates the sorting algorithm of the
  filtered files as follows:

  - If the ``--top-score-metric`` is set to ``median`` the results are first
    sorted by the ``Median MT Score``. If multiple epitopes have the same
    ``Median MT Score`` they are then sorted by the ``Corresponding Fold
    Change``. The last sorting criteria is the ``Best MT Score``.
  - If the ``--top-score-metric`` is set to ``lowest`` the results are first
    sorted by the ``Best MT Score``. If multiple epitopes have the same
    ``Best MT Score`` they are then sorted by the ``Corresponding Fold
    Change``. The last sorting criteria is the ``Median MT Score``.

- pVACseq, pVACfuse, and pVACbind now calculate manufacturability metrics
  for the predicted epitopes. Manufacturability metrics are also
  calculated for all protein sequences when running the ``pvacseq generate_protein_fasta``
  and ``pvacfuse generate_protein_fasta`` commands. They are saved in the ``.manufacturability.tsv``
  along to the result fasta.
- The pVACseq score that gets calculated for epitopes in the condensed report
  is now converted into a rank. This will hopefully remove any confusion about
  whether the previous score could be treated as an absolute measure of
  immunogencity, which it was not intended for. Converting this score to a
  rank ensures that it gets treated in isolation for only the epitopes in the
  condensed file.
- The condensed report now also outputs the mutation position as well as the
  full set of lowest and median wildtype and mutant scores.
- This version adds a clear cache function to pVACapi that can be called by
  running ``pvacapi clear_cache``. Sometimes pVACapi can get into a state
  where the cache file contains conflicting data compared to the actual
  process outputs which results in errors. Clearing the cache using the ``pvacapi clear_cache``
  function can be used in that situation to resolve these errors.

Past release notes can be found on our :ref:`releases` page.

To stay up-to-date on the latest pVACtools releases please join our :ref:`mailing_list`.

Citations
---------

Jasreet Hundal , Susanna Kiwala , Joshua McMichael, Chris Miller, Huiming Xia, 
Alex Wollam, Conner Liu, Sidi Zhao, Yang-Yang Feng, Aaron Graubert, Amber Wollam, 
Jonas Neichin, Megan Neveau, Jason Walker, William Gillanders,
Elaine Mardis, Obi Griffith, Malachi Griffith. pVACtools: A Computational Toolkit to
Identify and Visualize Cancer Neoantigens. Cancer Immunology Research. 
2020 Mar;8(3):409-420. doi: 10.1158/2326-6066.CIR-19-0401. 
PMID: `31907209 <https://www.ncbi.nlm.nih.gov/pubmed/31907209>`_.

Jasreet Hundal, Susanna Kiwala, Yang-Yang Feng, Connor J. Liu, Ramaswamy Govindan, William C. Chapman, 
Ravindra Uppaluri, S. Joshua Swamidass, Obi L. Griffith, Elaine R. Mardis, and Malachi Griffith. 
`Accounting for proximal variants improves neoantigen prediction <https://www.nature.com/articles/s41588-018-0283-9>`_. 
Nature Genetics. 2018, DOI: 10.1038/s41588-018-0283-9. PMID: `30510237 <https://www.ncbi.nlm.nih.gov/pubmed/30510237>`_.

Jasreet Hundal, Beatriz M. Carreno, Allegra A. Petti, Gerald P. Linette, Obi
L. Griffith, Elaine R. Mardis, and Malachi Griffith. `pVACseq: A genome-guided
in silico approach to identifying tumor neoantigens <http://www.genomemedicine.com/content/8/1/11>`_. Genome Medicine. 2016,
8:11, DOI: 10.1186/s13073-016-0264-5. PMID: `26825632
<http://www.ncbi.nlm.nih.gov/pubmed/26825632>`_.

Source code
-----------
The pVACtools source code is available in `GitHub <https://github.com/griffithlab/pVACtools>`_.

License
-------
This project is licensed under `BSD 3-Clause Clear License <https://spdx.org/licenses/BSD-3-Clause-Clear.html>`_.
