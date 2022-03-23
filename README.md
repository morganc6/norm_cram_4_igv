
# norm_cram_4_igv
normalisation of cram files for visualations in IGV
this process was put together with the goal of comparing normalized alignments between individauls so structural variants  could be visually validated on IGV

## Programs needed for execution
bedtools,samtools, igvtools, igv


## CRAM to BAM with extraction of

sample=$1
chr=$2
reference_fa=genome.fam
mygenome=hg19.chrom.sizes

##cram to bam # subset for chromosome of interest
samtools view -b -T $reference_fa $sample.cram $chr > $sample.bam

bamToBed -i $sample.bam > $sample.bed

genomeCoverageBed –i $sample.bed -bg –g $mygenome > $sample.wig

igvtools toTDF $sample.wig $sample.tdf hg19
