
# norm_cram_4_igv
normalisation of cram files for visualations in IGV
this process was put together with the goal of comparing normalized alignments between individauls so structural variants  could be visually validated on IGV
Using igv version(X>) allows you to normalize files using the tdf input


#### Programs needed for execution
* bedtools(v30.0)
* samtools(v1.15)
* igvtools (2.5.3)
* igv(v2.12.3)


## setup of variables

```
sample=$1
chr=$2
reference_fa=genome.fa
mygenome=hg19.chrom.sizes
```

convert bam to cram using samtools.
Do this for a specific chromosome or set of co-ordinates add this as the last arugment. 

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

Your file is now ready for viewing.
Open up IGV GUI, load your tdf file.

Ensure that "normalization" is checked by selecting View>Preferences>Tracks tab and selecting the Normalize Coverage Data checkbox.
