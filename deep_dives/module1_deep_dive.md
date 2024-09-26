# Module 1 Deep Dive

See the complete instructions for a deep dive in Deep_Dive_Overview. After each module, you will write a short report on the homework assignments included in the module. This write-up should be a self-contained representation of your work in this module.

Your deep dive must incude the following sections:

### Title: Data Access & Read Quality

### Overview:  Provide a brief overview of each homework assignment:

* Data tidiness & metadata
* Retrieving data from the sequence read archive (SRA)
* Assessing read quality, trimming and filtering

### Objective: The objective sets the stage for your analysis. It places your work in a broad theoretical context and gives the reader enough information to appreciate the objectives of the study.

### Materials and Methods: M&M provides the context for evaluating data in each homework. How did you obtain the data, what tools and parameters were used, what protocols did you use. What would another scientist need to know to reproduce your work?

* Data tidiness & metadata
* Retrieving data from the sequence read archive (SRA)
* Assessing read quality, trimming and filtering

### Results: The results section should (1) summarize the data emphasizing the important trends or patterns, and (2) illustrate and support your generalizations with explanatory details, Tables, or Figures. 

* Data tidiness & metadata
* Retrieving data from the sequence read archive (SRA)
* Assessing read quality, trimming and filtering

### Discussion: In the results section you reported your findings; now, in the discussion you need to tell the reader what the findings mean. Refer to figures or tables where necessary, but without reporting the data already in the results. Are your findings consistent with those reported from other researchers (Journal club?).

### Tables: All Tables should be cited throughout the text, and numbered consecutively. Be sure to include a title for the Table and a short description.

### Figures. All Figures should be cited throughout the text and numbered consecutively. Be sure to include a title for the Figure and a short legend.

### References: References are cited in the text, and presented in a consistent format in the “References” section.

## Questions to Answer in this Deep Dive:

* Data tidiness & metadata

1. What metagenomics sequencing technique was used to sequence the reads (WGS or amplicon)?
2. Which sequencing machine was used?
3. How many total samples were sequenced?
4. How many families are included in your project?
5. How many babies (or mothers) are included?
6. What time points did the samples come from?
7. How many mothers had a C-section vs vaginal birth?
8. Were the mothers given antibiotics and for which birth mode?

* Retrieving data from the sequence read archive (SRA)

1. What tool, version, commands and options did you use to download the fastq files?
2. What is the name of the data repository where the data downloaded from?

* Assessing read quality, trimming and filtering

1. What tool and version did you use to look at the quality of the fastq files?
2. What tool, version, and options did you use to trim raw fastq files?
3. Create a table showing the stats before and after trimming.
4. Look at a few of the before/after plots from fastqc, why did we use the trimming parameters we did? Describe how the parameters we used improved the reads.
