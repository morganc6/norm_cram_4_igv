
# norm_cram_4_igv
Normalisation of cram files for visualisations on IGV

It is possible to [generate normalised coverage files in IGV if a TDF file is generated using igvtools](https://software.broadinstitute.org/software/igv/igvtools) 
This process was put together to convert a CRAM file to TDF with the end goal of comparing normalized alignments between samples so structural variants could be visually validated on IGV.


##### Programs needed for execution
* bedtools(v30.0)
* samtools(v1.15)
* igvtools (2.5.3)
* igv(v2.12.3)

The above compliation of programs are available for download as a docker image if required.
```
docker pull morganc6/morganc6-norm4igv
```

##### setup of variables
The reference genome used in the below example is the human UCSC hg19 genome. The human genome FASTA file is "genome.fa".
The [hg19.chrom.sizes](http://hgdownload.cse.ucsc.edu/goldenpath/hg19/bigZips/hg19.chrom.sizes) is available for download on UCSC
```
sample=$1
chr=$2
reference_fa=genome.fa
mygenome=hg19.chrom.sizes
```

#### Series of commands to convert cram to TDF file
convert bam to cram using samtools.
Do this for all chromosomes, a specific chromosome or set of co-ordinates. This is defined in the last arugment in the following command.

```
samtools view -b -T $reference_fa $sample.cram $chr > $sample.bam
```

convert bam file to bed file with with bedtools
```
bamToBed -i $sample.bam > $sample.bed
```

convert bed file to wiggle file with bedtools
```
genomeCoverageBed –i $sample.bed -bg –g $mygenome > $sample.wig
```

convert wiggle file to tdf file using igvtools
```
igvtools toTDF $sample.wig $sample.tdf hg19
```

Your normalised file is now ready for viewing.
* Open up IGV GUI.
* Load your tdf file.
* Ensure that "normalization" is checked by selecting View>Preferences>Tracks tab and selecting the Normalize Coverage Data checkbox.
