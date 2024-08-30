---
# Metagenome Assembly

### Questions:
- Why should genomic data be assembled?
- What is the difference between reads and contigs?
- How can we assemble a metagenome?
### Objectives: 
- Understand what an assembly is. 
- Run a metagenomics assembly workflow.
- Use an environment in a bioinformatic pipeline.
### Keypoints:
- Assembly groups reads into contigs.
- De Bruijn Graphs use Kmers to assembly cleaned reads.
- Program screen allows you to keep open remote sessions.
- MetaSPAdes is a metagenomes assembler.
- Assemblers take FastQ files as input and produce a Fasta file as output.
---

## Assembling reads
The assembly process groups reads into contigs and contigs into 
scaffolds to obtain (ideally) the sequence of a whole 
genome. There are many programs devoted to
[genome](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2874646/) and 
metagenome assembly, some of the main strategies they use are Greedy extension, Overlap consensus (OLC), and De Bruijn charts. Contrary to metabarcoding, shotgun metagenomics needs an assembly step, which does not mean that metabarcoding never uses an assembly step but sometimes is unnecessary.

<a href="../fig/03-04-01.png">
  <img src="../fig/03-04-01.png" width="868" height="777" alt="Three diagrams depicting the three assembly algorithms: The Greedy extension starts with any read, extends it whit the reads that make a match to make a contig, it continues with a different read when the previous contig can not be extended anymore. The Overlap Layout consensus finds every pairwise overlap, makes a layout graph with all the overlaps and chooses consensus sequences to make the contigs. The De Bruijn Graphs divides the reads in k-mers, makes a k-mer graph that shows all the overlapping k-mers, and chooses paths from the graph to make the contigs. " />
</a>

[MetaSPAdes](https://github.com/ablab/spades) is an NGS de novo assembler 
for assembling large and complex metagenomics data, and it is one of the 
most used and recommended. It is part of the SPAdes toolkit, which 
contains several assembly pipelines.

Some of the problems faced by metagenomics assembly are:  
* Differences in coverage between the genomes due to the differences in abundance in the sample.  
* The fact that different species often share conserved regions.  
* The presence of several strains of a single species in the community.   

SPAdes already deals with the non-uniform coverage problem in its algorithm, so it is helpful for the assembly of simple communities, but the [metaSPAdes](https://pubmed.ncbi.nlm.nih.gov/28298430/) algorithm deals with the other problems as well, allowing it to assemble metagenomes from complex communities. 


Let's see if our program is installed correctly.
Quick check! Are you on an interactive node? It won't find the container below if not.
```
$ interactive -m 24GB -a bh_class
$ /contrib/singularity/shared/bhurwitz/spades:3.15.5--h95f258a_1.sif metaspades.py --help
```

```
SPAdes genome assembler v3.15.0 [metaSPAdes mode]

Usage: spades.py [options] -o <output_dir>

Basic options:
  -o <output_dir>             directory to store all the resulting files (required)
  --iontorrent                this flag is required for IonTorrent data
  --test                      runs SPAdes on a toy dataset
  -h, --help                  prints this usage message
  -v, --version               prints version

Input data:
  --12 <filename>             file with interlaced forward and reverse paired-end reads
  -1 <filename>               file with forward paired-end reads
  -2 <filename>               file with reverse paired-end reads    
```

## MetaSPAdes is a metagenomics assembler

The help we just saw tells us how to run `metaspades.py`. We are going 
to use the most straightforward options, just specifying our forward paired-end 
reads with `-1` and reverse paired-end reads with `-2`, and the output 
directory where we want our results to be stored.

Note that spades needs a lot of memory (we are asking for 24G!, and you need to be running the container on a compute node. Make sure you have run the `interactive -m 24GB -a bh_class` command above.

 ```
$ cd /xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/06_qc_trimming
$ /contrib/singularity/shared/bhurwitz/spades:3.15.5--h95f258a_1.sif metaspades.py -1 JC1A_R1.trim.fastq.gz -2 JC1A_R2.trim.fastq.gz -o ../08_assembly/assembly_JC1A
```

When the run is finished, it shows this message:

```
======= SPAdes pipeline finished.

SPAdes log can be found here: /xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/08_assembly/assembly_JC1A/spades.log

Thank you for using SPAdes!

```

Now, let's go to the output files: 
```
$ cd ../08_assembly/assembly_JC1A
$ ls -F
```


```
assembly_graph_after_simplification.gfa  corrected               K55              scaffolds.fasta
assembly_graph.fastg                     dataset.info            misc             scaffolds.paths
assembly_graph_with_scaffolds.gfa        first_pe_contigs.fasta  params.txt       spades.log
before_rr.fasta                          input_dataset.yaml      pipeline_state   strain_graph.gfa
contigs.fasta                            K21                     run_spades.sh    tmp
contigs.paths                            K33                     run_spades.yaml
```


As we can see, MetaSPAdes gave us a lot of files. The ones with the assembly are the `contigs.fasta` and the `scaffolds.fasta`. 
Also, we found three `K` folders: _K21, K33, and K55_; this contains the individual result files for an assembly 
with k-mers equal to those numbers: 21, 33, and 55. The best-assembled results are 
the ones that are displayed outside these k-folders. The folder `corrected` holds the corrected reads 
with the SPAdes algorithm. Moreover, the file 
`assembly_graph_with_scaffolds.gfa` has the information needed to visualize 
our assembly by different means, like programs such as [Bandage](https://rrwick.github.io/Bandage/).

The contigs are created from assembled reads, but the scaffolds are the result 
from a subsequent process in which the contigs are ordered, oriented, and connected with Ns.

We can recognize which sample our assembly outputs corresponds to because they are inside 
the assembly results folder: `assembly_JP4D/`. However, the files within it do not have the 
sample ID. If we need the files out of their folder, it is beneficial to rename them.

<br>

> ## Exercise 1: Compare two fasta files from the assembly output 
> You may want to know how many contigs and scaffolds result from the assembly. 
> Use `contigs.fasta`  and `scaffolds.fasta ` files and sort the commands to create correct code lines.   
> Do they have the same number of lines? Why?  
> **Hint**: You can use the following commands: `grep`, `|` (pipe), `-l`, `">"`, `wc`, `filename.fasta`
> 
<details>
  <summary markdown="span">Solution</summary>
  <ul> 

```
$ grep '>' contigs.fasta | wc -l
$ grep '>' scaffolds.fasta | wc -l
```

A contig is created from reads and then a scaffold from a group of contigs, so we expect fewer lines in the `scaffolds.fasta `. But, why is the difference so small between these two files for this sample?

hint: This sample has very shallow sequencing.

</details>

