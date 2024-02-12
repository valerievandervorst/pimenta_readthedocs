Usage
=====

.. _Usage:

Usage
------------
The basic command to run the example analysis, after editing the settings in settingsfile.txt: ::

   bash START.sh settingsfile.txt

The previous command runs Guppy and the first clustering per sample.
To run the reclustering and Taxonomic identification: ::

   bash START_recluster.sh settingsfile.txt


Modify the following parameters in settingsfile.txt to run the pipeline:</strong><br>
Main parameters
  +--------------------+---------------------------------------------------------------+
  | Variable             | Description                                               |
  +====================+===============================================================+
  | RunName              | Run name of the analysis, this will be the name of the output folder. |
  +--------------------+---------------------------------------------------------------+
  | mail-user             | Email address to receive updates about the job and results from the analysis. |
  +--------------------+---------------------------------------------------------------+
  | Targets              | Markers to analyze separated with ',', these marker names must correspond to the names in the [primer file](https://git.wur.nl/vorst016/dna-metabarcoding-combined-clustering/-/wikis/4-Primer-file), example: 18SV4,miniCOI |
  +--------------------+---------------------------------------------------------------+
  | THREADS              | Total CPU threads per sample.                                 |
  +--------------------+---------------------------------------------------------------+
  | GPU                  | GPU usage (only for Guppy basecalling): '1' for using GPU or 'cpu' to run Guppy on CPU. |
  +--------------------+---------------------------------------------------------------+
  | timepersample       | Total minutes per sample reserved by slurm.                     |
  +--------------------+---------------------------------------------------------------+
  | RunModules           | Modules to run, options: all (default), Guppy, Clustering, Consensus, Blast, Taxonomy.<br> Another module called 'oldmode' can also be used, which runs the tax identification per sample (instead of per dataset). Needs to be used in combination with 'all' or other modules (e.g. "oldmode,clustering,Consensus,Blast,taxonomy") (experimental). |
  +--------------------+---------------------------------------------------------------+
  | modus                | "all" to analyze all datasets within OutDir or "one" to analyze dataset with same RunName and OutDir. |
  +--------------------+---------------------------------------------------------------+

File location parameters
+-------------------+-----------------------------------------------------------------------------------+
| Variable          | Description                                                                       |
+===================+===================================================================================+
| FAST5Folder       | Folder containing the nanopore fast5/pod5 files                                   |
+-------------------+-----------------------------------------------------------------------------------+
| OutDir            | Path where the output of the DNA metabarcoding analysis is placed                 |
+-------------------+-----------------------------------------------------------------------------------+
| workdir           | Location of the Pimenta scripts on the HPC server, example: ~/pimenta             |
+-------------------+-----------------------------------------------------------------------------------+
| SampleDescription | tsv file containing sample names, see #sample-description                         |
+-------------------+-----------------------------------------------------------------------------------+
| PrimerFile        | file containing marker primers, #primer-file                                      |
+-------------------+-----------------------------------------------------------------------------------+
| NT_dmp            | contains nodes.dmp and names.dmp from NCBI  #installation                         |
+-------------------+-----------------------------------------------------------------------------------+
| DATABASE          | BLAST database, for example: /lustre/shared/wfsr-databases/BLASTdb/nt             |
+-------------------+-----------------------------------------------------------------------------------+
| ExSeqids          | file containing seqids that should be excluded from BLAST results, default=$workdir/Excluded.NCBI.identifications.tsv |
+-------------------+-----------------------------------------------------------------------------------+
| ExTaxids          | file containing taxids that should be excluded from BLAST results, default=$PWD/Excluded.NCBI.taxids |
+-------------------+-----------------------------------------------------------------------------------+


Tool parameters
+---------------+------+------------------------------------------------------------------------------------+
| Variable      | Tool | Description                                                                        |
+===============+======+====================================================================================+
| MinionKit     | Guppy| MinION kit used, for example: SQK-PBK004                                           |
+---------------+------+------------------------------------------------------------------------------------+
| MinionFlowCell| Guppy| Flow cell used for sequencing, for example: FLO-FLG001                              |
+---------------+------+------------------------------------------------------------------------------------+
| ExpansionKit  | Guppy| Barcode expansion kit, set it to "none" if not used. example: EXP-NBD104            |
+---------------+------+------------------------------------------------------------------------------------+
| MinMaxLength  |Prinseq| Filtering on nt length with Prinseq, preferably around the length of your primers,  |
|               |      | for example: 100-500                                                               |
+---------------+------+------------------------------------------------------------------------------------+
| TrimLeft      |Prinseq| Total nt to be trimmed of on the left side of sequence, default=5                    |
+---------------+------+------------------------------------------------------------------------------------+
| TrimRight     |Prinseq| Total nt to be trimmed of on the right side of sequence, default=5                   |
+---------------+------+------------------------------------------------------------------------------------+
| MinQualMean   |Prinseq| Minimum quality score of reads that can pass the filtering, default=12               |
+---------------+------+------------------------------------------------------------------------------------+
| Ident1        |CD-HIT| Minimum percentage identity for the first clustering with CD-HIT, default=0.93      |
+---------------+------+------------------------------------------------------------------------------------+
| Ident2        |CD-HIT| Minimum percentage identity for the reclustering with CD-HIT, default=1             |
+---------------+------+------------------------------------------------------------------------------------+
| MinClustSize  |CD-HIT| Minimum cluster size                                                               |
+---------------+------+------------------------------------------------------------------------------------+
| Error         |Cutadapt| Maximum error rate that Cutadapt allows, default=0.15                              |
+---------------+------+------------------------------------------------------------------------------------+
| Evalue        |BLAST | Maximum E-value that BLAST allows, default=0.001                                    |
+---------------+------+------------------------------------------------------------------------------------+
| Pident        |BLAST | Minimum percentage identity for filtered BLAST results, default=90                   |
+---------------+------+------------------------------------------------------------------------------------+
| Qcov          |BLAST | Minimum Query coverage for filtered BLAST results, default=90                        |
+---------------+------+------------------------------------------------------------------------------------+
| MaxTargetSeqs |BLAST | Maximum amount of hits BLAST outputs per consensus sequence, default=100             |
+---------------+------+------------------------------------------------------------------------------------+


