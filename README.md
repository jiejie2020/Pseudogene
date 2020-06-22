# Pseudogene identification pipeline

This pipeline is a manually modified version based on the program suite Psi-Phi (Lerat and Ochman, 2004), which identifies pseudogenes by performing tblastn within a cluster of closely related genomes.

The first module is designed both to analyze tblastn output made using the proteins of the query genome against the complete sequence of the target genome, and to obtain the lengths of query CDSs by searching in the Genbank file of the query species.
The second module of the program compares the output of the first module to the Genbank file of the target genome to identify and retain sequences whose characteristics denote assignment as a potential pseudogene.

# Input data

* Amino acid files of all genomes named *.faa
* Genome fasta files named *.fasta (draft genome) or *.fas (complete genome)
* Genbank files of all genomes named *.gbk

Original Psi-Phi scripts are designed for complete genome. For genome assemblies that have not been completely assembled into a single chromosome, all contigs of a genome should be concatenated to an artificially assembled genome first (*.fasta->*.fas) as the input of Psi-Phi.

# Instructions for pseudogene identification
 
1. Makeblastdb for tblastn

>In this step, command line will be written as below to make nucleotide database of all artificially assembled genome.

```
makeblastdb -in input.fas -dbtype nucl -parse_seqids -out input.fas
```

2. Run tblastn jobs

>In this step, command line will be written as below to submit blast jobs for all pairs of the genome sequences.

```           
tblastn -query query.faa -db target.fas -outfmt 6 -evalue 1e-15 -out query.vs.target.blast
```

3. Batch run module1 of Psi-Phi using `batch.module1.pl`

>This step retrieves particular data from a blast file, output results will be found in `query.vs.target.sortie.out` files.

4. Batch run module2 of Psi-Phi using `batch.module2.pl`

>This step determines the list and category of each pseudogene candidate using the output of the first module. Potential pseudogenes will be found in `pseudo_target_use_query` files.

# Instructions for pseudogene filtering

>The filtering step is further applied to filter out misidentified pseudogenes by Psi-Phi.

# Citation

If you are interested in using this modified pipeline for pseudogene identification with filtering, please cite:

Li S, Chu X, Luo D, Wang S, Luo H. Gene Loss through Pseudogenization Contributes to the Ecological Diversification of a Generalist Marine Bacterial Group. (unpublished)
