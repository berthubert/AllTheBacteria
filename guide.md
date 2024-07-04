**XXX This is likely 90% correct at best. Meant as a first attempt to get it right eventually**

Recently a group of scientists associated with the European Molecular Biology Laboratory released [a compilation of 1,932,812 assemblies of bacterial chromosomes](https://www.biorxiv.org/content/10.1101/2024.03.08.584059v1.full), building on earlier work by [Grace Blackwell et al](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.3001421). 

This assembly was performed on FASTQ reads available on the European Nucleotide Archive as of May 2023. Only eubacterial (so not archaeal) data was included, and only Illumina produced reads were processed. [After I asked](https://github.com/AllTheBacteria/AllTheBacteria/issues/28), it now appears that archaeal chromosomes could now also be included in upcoming versions, which is wonderful news. 

It is important to note that this project is not a strict alternative to the selection of complete chromosomes that you can find in the [NCBI Reference Sequence Database](https://www.ncbi.nlm.nih.gov/refseq/). AllTheBacteria delivers contigs, not reference genomes or scaffolds.

The project very much aims to benefit from community effort, and in this spirit, I want to explain a bit what is actually in the AllTheBacteria data, and how to use it.

# AllTheBacteria releases
The project has a [GitHub page](https://github.com/AllTheBacteria/AllTheBacteria) through which we can find links to releases. The current release is [0.2](https://ftp.ebi.ac.uk/pub/databases/AllTheBacteria/Releases/0.2/). 

In there we find a bunch of directories, as follows.

## assembly/
[This directory](https://ftp.ebi.ac.uk/pub/databases/AllTheBacteria/Releases/0.2/assembly/) has a cleverly arranged set of tar.xz files, grouped by bacterial species, containing FASTA files. If you know what you are looking for, you can find the right tar.xz files. The FASTA files within these tar files have been arranged using [smart software called MiniPhy](https://github.com/karel-brinda/MiniPhy) so that they compress really well.

If you are looking for a popular organism, say E. coli, you'll still have to pick from many dozens of tar files though, and see below on how the 'metadata/' directory will help you there.

In addition to tar.xz files named after popular organisms, there are also 'dustbin' files containing all remaining chromosomes.

It is important to note that the FASTA files contain contigs, they are not reference sequences. As an example, let's look at `escherichia_coli__01/SAMD00075771.fa`, this contains entries like:

```
>SAMD00075771.contig00001 len=256900 cov=25.5 corr=0 origname=NODE_1_length_256900_cov_25.545194_pilon sw=shovill-spades/1.1.0 date=20230709
CTCCGCTATGTCCTTGACGTCATAGCCGACTGGCCGATAAACCGGGTCGGCGAACTGCTCC....
```
We can look up SAMD00075771 over at the ENA, on [https://www.ebi.ac.uk/ena/browser/view/SAMD00075771?dataType=BIOSAMPLE](https://www.ebi.ac.uk/ena/browser/view/SAMD00075771?dataType=BIOSAMPLE) and there we learn that this data was submitted as 'ASM276431v1 assembly for Escherichia coli O26:H11'. We can also download the paired FASTQ there, plus the originally submitted assembly.

## metadata/
In here we find a whole bunch of files:

 * assembly-stats.tsv.gz: per sample how many contigs there are, the N50, N70, N90 
 * checkm2.tsv.gz: CheckM2 is a tool for assessing microbial genome quality using machine learning. This file features estimates for completeness, contamination, number of detected coding sequences
 * ena_metadata.tsv.gz: All in one view of the many accession numbers and identifiers from ENA. This is all input related, where did the data come from
 * hq_set.sample_list.txt.gz: A list of samples that pass more stringent quality checks, as outlined in the AllTheBacteria preprint. 
 * hq_set.removed_samples.tsv.gz: Samples that did not make it to the high-quality list, together with reasons why not
 * nucmer_human.gz: reads from samples that were identified as human in origin
 * sample_list.txt.gz: names of all ENA samples included, presumably the sum of hq_set.sample_list and hq_set.removed_samples above	
 * species_calls.tsv.gz: automated majority species call per sample, plus indication if this is a 
high quality sample or not	
 * sylph.tsv.gz: the Sylph program compares samples to GTDB reference genomes, and this file contains the best fits for each sample. The top call ends up in species_calls.tsv above.
 * sylph.no_matches.txt.gz: files for which Sylph had no idea
 
 

