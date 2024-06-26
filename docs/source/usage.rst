Usage
=====

.. _Usage:
.. _Parameters:
.. _Primer file:
.. _Sample description:
.. _Output files:


Running an analysis
------------
The basic command to run the analysis, after editing the settings in settingsfile.txt: ::

   bash START.sh settingsfile.txt

The previous command runs Guppy and the first clustering per sample.
To run the reclustering and Taxonomic identification: ::

   bash START_recluster.sh settingsfile.txt


Parameters
------------
Modify the following parameters in settingsfile.txt to run the pipeline:

**Main parameters**

+--------------------+-----------------------------------------------------------------------+
| Variable           | Description                                                           |
+====================+=======================================================================+
| RunName            | Run name of the analysis, this will be the name of the output folder. |
+--------------------+-----------------------------------------------------------------------+
| mail-user          | Email address to receive updates about the slurm job of the analysis. |
+--------------------+-----------------------------------------------------------------------+
| Targets            | Markers to analyze separated with ',' , these marker names must       |
|                    | correspond to the names in the primer file. For example: '18SV4,COI'  |
+--------------------+-----------------------------------------------------------------------+
| THREADS            | Total CPU threads per sample.                                         |
+--------------------+-----------------------------------------------------------------------+
| GPU                | GPU usage (only for Guppy basecalling): '1' for using GPU or          |
|                    | 'cpu' to run Guppy on CPU.                                            |
+--------------------+-----------------------------------------------------------------------+
| timepersample      | Total minutes per sample reserved by slurm.                           |
+--------------------+-----------------------------------------------------------------------+
| RunModules         | Modules to run.                                                       |
|                    | Options:                                                              |
|                    | all (default)                                                         |
|                    | , Guppy                                                               |
|                    | , Clustering                                                          |
|                    | , Consensus                                                           |
|                    | , Blast                                                               |
|                    | , Taxonomy                                                            |
|                    |                                                                       |
|                    | Another module called 'oldmode' can also be used, which runs          |
|                    | the tax identification per sample (instead of per dataset).           |
|                    | Needs to be used in combination with 'all' or other modules           |
|                    | (e.g. "oldmode,clustering,Consensus,Blast,taxonomy")                  |
+--------------------+-----------------------------------------------------------------------+
| modus              | "all" to analyze all datasets within OutDir or "one" to analyze       |
|                    | dataset with same RunName and OutDir.                                 |
+--------------------+-----------------------------------------------------------------------+


**File location parameters**

+-------------------+-----------------------------------------------------------------------------------+
| Variable          | Description                                                                       |
+===================+===================================================================================+
| FAST5Folder       | Folder containing the nanopore fast5/pod5 files                                   |
+-------------------+-----------------------------------------------------------------------------------+
| OutDir            | Path where the output of the DNA metabarcoding analysis is placed                 |
+-------------------+-----------------------------------------------------------------------------------+
| workdir           | Location of the Pimenta scripts on the HPC server, example: ~/pimenta             |
+-------------------+-----------------------------------------------------------------------------------+
| SampleDescription | tsv file containing sample names, see :ref:`id5<Sample description>`              |
+-------------------+-----------------------------------------------------------------------------------+
| PrimerFile        | file containing marker primers, see :ref:`id4<Primer file>`                       |
+-------------------+-----------------------------------------------------------------------------------+
| NT_dmp            | contains nodes.dmp and names.dmp from NCBI  :doc:`installation`                   |
+-------------------+-----------------------------------------------------------------------------------+
| DATABASE          | BLAST database, for example: /lustre/shared/wfsr-databases/BLASTdb/nt             |
+-------------------+-----------------------------------------------------------------------------------+
| ExSeqids          | file containing seqids that should be excluded from BLAST results,                |
|                   | default=$workdir/Excluded.NCBI.identifications.tsv                                |
+-------------------+-----------------------------------------------------------------------------------+
| ExTaxids          | file containing taxids that should be excluded from BLAST results,                |
|                   | default=$PWD/Excluded.NCBI.taxids                                                 |
+-------------------+-----------------------------------------------------------------------------------+



**Tool parameters**

+-----------------+--------+------------------------------------------------------------------------------------+
| Variable        | Tool   | Description                                                                        |
+=================+========+====================================================================================+
| MinionKit       | Guppy  | MinION kit used, for example: SQK-PBK004                                           |
+-----------------+--------+------------------------------------------------------------------------------------+
| MinionFlowCell  | Guppy  | Flow cell used for sequencing, for example: FLO-FLG001                             |
+-----------------+--------+------------------------------------------------------------------------------------+
| ExpansionKit    | Guppy  | Barcode expansion kit, set it to "none" if not used. example: EXP-NBD104           |
+-----------------+--------+------------------------------------------------------------------------------------+
| MinMaxLength    |Prinseq | Filtering on nt length with Prinseq, preferably around the length of your primers, |
|                 |        | for example: 100-500                                                               |
+-----------------+--------+------------------------------------------------------------------------------------+
| TrimLeft        |Prinseq | Total nt to be trimmed of on the left side of sequence, default=5                  |
+-----------------+--------+------------------------------------------------------------------------------------+
| TrimRight       |Prinseq | Total nt to be trimmed of on the right side of sequence, default=5                 |
+-----------------+--------+------------------------------------------------------------------------------------+
| MinQualMean     |Prinseq | Minimum quality score of reads that can pass the filtering, default=12             |
+-----------------+--------+------------------------------------------------------------------------------------+
| Ident1          |CD-HIT  | Minimum percentage identity for the first clustering with CD-HIT, default=0.93     |
+-----------------+--------+------------------------------------------------------------------------------------+
| Ident2          |CD-HIT  | Minimum percentage identity for the reclustering with CD-HIT, default=1            |
+-----------------+--------+------------------------------------------------------------------------------------+
| MinClustSize    |CD-HIT  | Minimum cluster size for first clustering                                          |
+-----------------+--------+------------------------------------------------------------------------------------+
| MinClustSize2   |CD-HIT  | Minimum cluster size for clusters in reclustering                                  |
+-----------------+--------+------------------------------------------------------------------------------------+
| Error           |Cutadapt| Maximum error rate that Cutadapt allows, default=0.15                              |
+-----------------+--------+------------------------------------------------------------------------------------+
| Evalue          |BLAST   | Maximum E-value that BLAST allows, default=0.001                                   |
+-----------------+--------+------------------------------------------------------------------------------------+
| Pident          |BLAST   | Minimum percentage identity for filtered BLAST results, default=90                 |
+-----------------+--------+------------------------------------------------------------------------------------+
| Qcov            |BLAST   | Minimum Query coverage for filtered BLAST results, default=90                      |
+-----------------+--------+------------------------------------------------------------------------------------+
| MaxTargetSeqs   |BLAST   | Maximum amount of hits BLAST outputs per consensus sequence, default=100           |
+-----------------+--------+------------------------------------------------------------------------------------+
| filtertax       |BLAST   | Keep all results in $filtertax, ex. "Metazoa", leave empty "" to keep everything   |
+-----------------+--------+------------------------------------------------------------------------------------+

Primer file
------------
The primer file is a csv file including primer sequences in the following format: ::

  Target;sequence;forward
  Target;sequence;reverse

Example:
::
   18SV9;GTACACACCGCCCGTC;forward
   18SV9;TGATCCTTCTGCAGGTTCACCTAC;reverse
   18SV4;AGGGCAAKYCTGGTGCCAG;forward
   18SV4;GRCGGTATCTRATCGYCTT;reverse

It is important that the target name is the same name given in the settingsfile.


Sample description
------------
If you want to give names to the different samples, you could create a sample description txt file. 
It has the following format: ::
   ##BarcodeID;SampleName
   barcode01;Sample_1
   barcode02;Sample_2


If you want to use oldmode, then this sampledescription is a requirement and also includes the markers used. 
example:
::
   ##BarcodeID;SampleName;DNAbarcodes
   barcode03;Mix_1;miniCOI,18SV9,18SV4
   barcode04;Mix_2;miniCOI,18SV9,18SV4

Output files
------------
While running PIMENTA, a lot of files are created in `OutDir` / `RunName` 
Beneath is an overview with a short explanation per output.

+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| file/folder                       | file/folder                       | file/folder    | explanation                                                                           |
+===================================+===================================+================+=======================================================================================+
| barcode*.`SampleName`             |                                   |                | Folder containing individual sample data                                              |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| barcode*.`SampleName`             | barcode*.`SampleName`.fastq       |                | "raw" reads basecalled and demultiplexed by Guppy                                     |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| barcode*.`SampleName`             | barcode*.`SampleName`.QC.fastq,   |                | QC filtered reads in fastq and fasta format                                           |
|                                   | barcode*.`SampleName`.QC.fasta,   |                |                                                                                       |
|                                   | barcode*.`SampleName`.QC.gd       |                |                                                                                       |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| barcode*.`SampleName`             | ClustCons                         | multi-seq      | folders containing clustering data and cluster fasta files                            |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| `RunName`.ClusterContent.tsv      |                                   |                | tsv file containing an overview of the reclustered clusters with more information of  |
|                                   |                                   |                | the size, taxonomy, etc. of each cluster                                              |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| `RunName`.PS.fasta                |                                   |                | fasta file containing consensus sequences from all samples from the first clustering  |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| `RunName`.settings.all.txt        |                                   |                | file containing the settings used during the analysis                                 |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| `RunName`.stats.txt               |                                   |                | file containing an overview of the cluster distribution over the different markers    |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| Taxonomy_per_sample_`Ident2`      |   `Target`                        |                |  folder containining tsv files with taxonomy overview with readcounts per sample      |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| Taxonomy_per_sample_`Ident2`      | krona                             |                |  folder containining krona plots per target and sample                                |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+
| job_scheduler                     | LOGS                              |                |  this folder contains all slurm log files (except Guppy logs)                         |
+-----------------------------------+-----------------------------------+----------------+---------------------------------------------------------------------------------------+

