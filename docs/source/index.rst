Welcome to PIMENTA's documentation!
===================================

**PIMENTA** a pipeline for the rapid identification of species in forensic samples using MinION data. The pipeline consists of eight linked tools, and data analysis passes through 3 phases: 1) pre-processing the MinION data through read calling, demultiplexing, trimming sequencing adapters, quality trimming and filtering the reads, 2) clustering the reads, continued by MSA and consensus building per cluster, 3) reclustering of consensus sequences, followed by another MSA and consensus building per cluster, 4) Taxonomy identification with the use of a BLAST analysis.



Check out the :doc:`usage` section for further information, and 
how to :doc:`installation` the project.

.. note::

   This project is under active development.

Contents
--------

.. toctree::

   installation
   primer file
   sample description
   output files
